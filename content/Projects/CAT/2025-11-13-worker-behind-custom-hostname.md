Goal: показать пример как установить cloudflare worker за BringYourIP, то есть со своим IP адресами.

### Preconditions
На необходим CloudFlare Enterprice domain, который будет источником для всех доменов с BYIP. Так же нам необходимо иметь доступные слоты Custom Hostname под каждый домен, который мы хотим вынести на отдельный ip.

## Steps
Загружаем CloudFlare Worker в аккаунт в котором находится Enterpice домен.

Заполняем DNS записи Enterprise домена каким-либо мусором, к примеру: 
`@ CNAME Google.com`
обязательно с включенным проксированием.

Пример:
![cf-enterprise-dummy-value.png](./attachments/cf-enterprise-dummy-value.png)

Настраиваем worker на перехват всех запросов по route (не перехват домена). Это важный шаг, т.к. в противном случае, если весь домен завести на worker - он не будет отрабатывать для custom hostnames. 

Поэтому нам необходим именно вариант привязки по роутам.

![cf-attach-worker-1.png](./attachments/cf-attach-worker-1.png)
1. выбираем нужный воркер
2. переходим в настройки
3. нажимаем добавить Domains & Routes
4. Выбираем тип добавления: Routes
![cf-attach-worker-2.png](./attachments/cf-attach-worker-2.png)
5. Выбираем наш enterprise домен
6. Вводим магический route: `*/*` (перехват всего)

После этого к домену можно подключать custom hostnames.

