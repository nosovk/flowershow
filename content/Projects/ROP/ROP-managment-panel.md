Goal: оператор РОП чаще всего имеет свои аппараты подключенные к системе. А это значит что у него есть техник, которому нужно обслуживать аппараты. Для техника и менеджера необходим доступ к админ функциям управления физическими аппаратами.

## API

Апи которое мы используем для управления:
https://api.rlslot.com/api/v1/documentation/#/
лежит в скоупе manage.

Важно: для manage апи мы используем другие наборы апи ключей, чем для основного апи. Это значит, что в /machine/list и /manage/machine/list будут разные наборы аппаратов (!!!)

Метод /managment не возвращает лобби, он показывает только serials машин.

#### Роли:

На данный момент мы можем выделить две роли:

- менеджер аппаратов (device manager): может включать аппараты, блокировать аппараты
- сервисный инженер (service engeneer): может переводить аппарат в сервисный режим и инициировать подключение в сервисную сессию.
  Важно: у пользователей с этими ролями может не быть доступа к списку игроков на продукте (в целом не должно быть)

## UI

Как основной вариант отображения предлагаю использовать таблицу

| serial   | is_alive | status | session_id      | user_id | commands       | service mode          |
| -------- | -------- | ------ | --------------- | ------- | -------------- | --------------------- |
| 02000002 | true     | busy   | skjkljwkdjelwde | mkcms   | [send command] | enter \| exit+connect |

Где:

- serial - серийный номер аппарата, ид
- is_alive - признак из апи
- status - текущий статус аппарата (важно разукрасить их разными цветами, список статусов: ???)
- session_id - идентификатор игровой сессии, если аппарат занят
- user_id: внешний идентификатор пользователя в казино (для нас это ссылка на профиль пользователя с этим ид)
- commands: место для кнопки command. Нажатие на нее должно открывать модальное окно с командами для аппарата.
- service mode: колонка с кнопками только для роли Сервисный инженер. В зависимости от состояния содержит либо одну кнопку - включить сервисный режим, либо две кнопки - выключить сервисный режим или подсоединится
- Основная проблема в том, что статусы у нас не будут интерактивными, поэтому нужна еще какая-то кнопка, которая бы дергала и обновляла статусы, а так же фоновое обновление статусов (к примеру раз в 5 минут). Так же нужно где-то показать информацию о том, когда последний раз были обновлены данные, возможно сразу показывать - 3 секунды назад, 2 секунды назад, чтобы человек видел насколько устаревшая у него информация.

#### Меню отправки команды

Предлагается показать данные аппарата (серийник и статус, время последнего обновления данных с аппарата) + набор кнопок для отправки действия аппарату:

- reboot
- cashout
- disable
- ....
  Нажатие на кнопку должно приводить к отправке команды

#### Массовые действия

один из базовых сценариев использования - временно выключить все аппараты, после чего включить все аппараты. Для реализации этого сценария нам необходима возможность выбрать список аппаратов (чекбоксы) или выбрать сразу все аппараты, для того чтобы одномоментно применить к ним команду (в к примеру перевести их в заблокированное состояние)

### Сервисный режим

Сервисный режим необходим для досутпа к инженерному меню, доступен только пользователям с ролью Сервисный инженер. В этом режиме можно получить специальную ссылку на подключение к аппарату, в котором не действуют ограничения на сессии и есть дополнительные кнопки. Соответственно в интерфейсе мы показываем кнопку - перевести аппарат в сервисный режим. Если же статус аппарата service, то вместо этого мы показываем две кнопки - выйти из сервисного режима, и подключится к аппарату.

## Примеры запросов

- `api_base=https://api.rlslot.com/api/v1`
- `key=d859ded8-e8d5-454b-9902-6f521a21c8de`

#### List

```bash
curl "$api_base/manage/machines?key=$key"
```

```json
{
  "machines": [
    {
      "serial": "0400000000000004",
      "is_alive": true,
      "status": "online",
      "tags": {}
    },
    {
      "serial": "FF00000000000002",
      "is_alive": false,
      "status": "online",
      "tags": {}
    }
  ]
}
```

#### Get Machine by Serial

```bash
curl -s "$api_base/manage/machines/FF00000000000002?key=$key"
```

```json
{
  "machine": {
    "serial": "FF00000000000002",
    "is_alive": false,
    "status": "online",
    "tags": {}
  }
}
```

#### Commands

##### Calibrate

```bash
curl -X POST \
  "$api_base/manage/machines/FF00000000000002/command?key=$key" \
  -H 'Content-Type: application/json' \
  -d '{"command":"calibrate"}'
```

##### Reboot

```bash
curl -X POST \
  "$api_base/manage/machines/FF00000000000002/command?key=$key" \
  -H 'Content-Type: application/json' \
  -d '{"command":"reboot"}'
```

#### Reset on-server state

```bash
curl -X POST "$api_base/manage/machines/FF00000000000002/reset?key=$key"
```

```json
{
  "machine": {
    "serial": "FF00000000000002",
    "is_alive": false,
    "status": "online",
    "tags": {}
  }
}
```

##### Reset all Machines

```bash
curl -X POST "$api_base/manage/machines/reset?key=$key"
```

```json
{
  "machines": [
    {
      "serial": "0400000000000004",
      "is_alive": false,
      "status": "online",
      "tags": {}
    },
    {
      "serial": "FF00000000000002",
      "is_alive": false,
      "status": "online",
      "tags": {}
    }
  ]
}
```

#### Update on-server state

NOTE: usage of this method is discouraged, because it doesn't respect the attached session during status change, which might need to make a payout first

```bash
curl -X POST \
  "$api_base/manage/machines/FF00000000000002/update?key=$key" \
  -H "Content-Type: application/json" \
  -d '{"status":"online"}'
```

```json
{
  "machine": {
    "serial": "FF00000000000002",
    "is_alive": false,
    "status": "online",
    "tags": {}
  }
}
```

```bash
curl -X POST \
  "$api_base/manage/machines/FF00000000000002/update?key=$key" \
  -H "Content-Type: application/json" \
  -d '{"game_start":false}'
```

```json
{
  "machine": {
    "serial": "FF00000000000002",
    "is_alive": false,
    "status": "online",
    "tags": {}
  }
}
```

#### Enter Service Mode

```bash
curl -X POST "$api_base/manage/machines/FF00000000000002/service/enter?key=$key"
```

```json
{
  "machine": {
    "serial": "FF00000000000002",
    "is_alive": true,
    "status": "service-payout",
    "tags": {}
  }
}
```

...then poll for Machine's status until it becomes `service`:

```bash
curl -s "$api_base/manage/machines/FF00000000000002?key=$key"
```

```json
{
  "machine": {
    "serial": "FF00000000000002",
    "is_alive": true,
    "status": "service",
    "tags": {}
  }
}
```

#### Exit Service Mode

```bash
curl -X POST "$api_base/manage/machines/FF00000000000002/service/exit?key=$key"
```

```json
{
  "machine": {
    "serial": "FF00000000000002",
    "is_alive": true,
    "status": "online",
    "tags": {}
  }
}
```

#### Create Service Session

```bash
curl -X POST "$api_base/manage/machines/FF00000000000002/service/session/init?key=$key"
```

```json
{
  "url": "https://game.rlslot.com/client?s=op_..."
}
```

The session will be on it's own, will dispose itself after server restart / client disconnect etc
