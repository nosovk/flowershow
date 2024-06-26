Мы хотим сделать урезанный wrapper.
Он не должен работать только по IP матчингу. Цель: уменьшить количество зависимостей и отказаться от интеграции IDFA.
## Flows
- [[ip-track]]
## SDK которые необходимо интегрировать
Необходимо интегрировать
- [firebase cloud messaging sdk](https://pub.dev/packages/firebase_messaging)

Запрос permissions на отправку пушей нужно делать сразу при старте приложения. Отказ от выдачи разрешений не должен блокировать запуск приложения.

> Важно: мы не хотим подключать никакие части Firebase кроме CloudMessaging (экономим место)

## Проверка на запуск рекламы
У нас есть три шага проверки на запуск рекламы.
### Проверка по ip при запуске
Мы делаем запрос на /sendAttribution со значением параметра `{"action": "launch"}`. Если в ответ приходит указание показать рекламу, показываем. Если приходит `{"action":"none"}`, то мы показываем игру. 

Если это первый запуск приложения мы добавляем специальный параметр: launchNo: 1. Далее мы увеличиваем его на 1 при каждом запуске приложения.


> Важно: url api метода должен быть вынесен в конфиг.

POST `/sendAttribution`
```json
{
  "data": {	}, // всегда пустой обьект
  "appsflyerid": "", // всегда пустая строка
  "appsflyer_app_id": "", // всегда пустая строка
  "imei": "IMEI not available due to new Android/iOS policies", // imei, может быть любая строка
  "idfa": "", // всегда пустая строка
  "idfv": "", // всегда пустая строка
  "att": -1, // всегда -1
  "customer_user_id": "uuid", // у нас нет ид устройств, поэтоу это наш ид инсталяции, просто uuid сохраненный в appdata, чтобы он не менялся при перезапуске
  "app_version_name": "1.0.1", // версия нашего билда
  "app_store": "com.apple", // источник установки
  "bundleIdentifier": "com.bigfishbonansa.app",, // id приложения в сторе для идентификации приложения
  "aie": false,
  "network_connection_type": "wifi", // mobile5G|mobile4G|mobile3G|otherMobile
  "battery_level": "58%",
  "device": {
    "device_dpr": 3,
    "device_width": 1170,
    "device_height": 2532,
    "model": "iPhone",
    "os_name": "iOS",
    "version": "17.5.1",
    "manufacture": "apple",
    "device_uuid": "2CEE1E8C-F463-459E-BD9F-A7D665E0C72D"
  },
  "fcmid": "dwreerd", // token to send firebase notifications
  "apple_id": "not iOS",
  "apple_id_prefix": "not iOS",
  "ads_url": "https://offer.com", // last url from attribution api
  "current_locale": "en_US",
  "action": "launch", // "launch"|"timeout"|"attribution"|"late_attribution",
  "last_url": "https://offer.com/deposit", // last visited url
  "launchNo": 1, // it is 1 for first launch, and more in next ones
}
```

В ответ мы получаем информацию о целевом действии:
```json
{"action": "none"}
```
В этом случаем мы запускаем игру, как обычно.
либо:
```json
{
  "action": "redirect",
  "payload": "https://offer.com", // куда отправить пользователя
  "preservSettings": true, // сохранять урл
  "resetSettings": true, // переписать уже существующие настройки
}
```
В этом случае: при получении команды на редирект мы должны открыть webview на весь экран с полученным урл. Опции закрыть webview не предусмотренно.
- preservSettings: активирует режим, в котором мы дополнительно сохраняем последний урл на котором был пользователь при закрытии приложения.
- resetSettings: сбрасывает текущий урл на новый payload

> Важно: если кратко, то мы должны сохранять payload, и переиспользовать его, если не пришел ответ resetSettings. При этом мы в любой момент можем переопределить старый оффер.

> Важно: если человек получил рекламный урл, и после этого получил ответ `{"action":"none"}`, то отключения рекламы не происходит, мы продолжаем показывать ранее полученные настройки

> Важно: если человек получил `{"action":"none"}`, а после этого получил реклманую информацию, то она переопределяет ранее полученный `none`

## preservSettings and preservUrl
Режим работы, при котором мы сохраняем на какой странице пользователь закончил взаимодействие. Этот режим должен сохранять preservUrl настройку при выходе из приложения. Таким образом при старте приложения должен запускаться не "ads_url" полученный ранее, а last_url, на котором человек закончил взаимодействие.
## resetSettings
при получении значения true сбрасываются так же и настройки preservSettings и все сохраненные в приложении урл (ads_url, last_url)

## launchNo
В этом параметре мы передаем номер запуска приложения. Счет начинается с 1. Таким образом мы можем легко отличить первый запуск от последующих.