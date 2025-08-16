## Goal: 
иметь возможность предоставить пользователям apk, который бы позволял открывать продукт в странах с гео блокировкой + блокировками зеркал.

## UserFlow
[![](https://mermaid.ink/img/pako:eNp1VE1v2zAM_SuETh2aFI3bJLYPAQJ3G3woZiDZZciFtdVYiC15kpysK_rfR8kfSNPOJ1Pke3yPFPTKclVwFjPDf7dc5vxB4F5jvZPYWiXb-onrnQT6MLdKQwZoIKvwZThuUFuRiwalha1LbjXmB67XWfqxYOMKNhbp4GNy3TQuvZaFVqJw4Sc1WepqUmm5lliB79KVZTBdra5hG4NquIRKyAOchC1hz63jwNrA1Wa7_g4oC_i5ffzS4bYwna4czOCRg2jgGgwnD2UPGqtWK8ioihMaO3qrAJvD2J5oNjEU6iQrhcVFyoOFNBar6jJ17czGUGErqS0Oxt1AHJBMxiTGGCjQouvqxnBV8KPISbF8Vr0Vd-xbkRs0B3imhaWZR515XU07Ru_EcNSjVQIZlQu0vOhGl2ZnxE6Jk9lodRQFH0fgxn0h2JUJKayjghN_Ogp-6kpyzd3Z-U4Tt9EEjZDqHc0Ukn6X-VmyXzOl6IKW1MXxn8RwHZMO6RV4g91dTR_81pPWWFVTWLzr1M8frXVrf4f4ljymD_-XPvD98B7G6NLHmOhFnbGeUYyj8zVNa8pPxmpKdRpzO8kmbK9FwWKrWz5hRFSjC9mrzzNb8prvWEy_BerDju3kG2FI_S-l6gGmVbsvWfyMlaGobejGDA_BeKpJFNeJaqVl8d1sGXgWFr-yPxQHN9H9MgrC23kwD4LZcsJeWLyY31AYLuZ30Xx2H4TR24T99W1vb8L7RRiF0WwRzcJwOZtPmHtwNi8yH0TxQtCL89g9T_6VGqR99Zle2ds_Erd8TA?type=png)](https://mermaid.live/edit#pako:eNp1VE1v2zAM_SuETh2aFI3bJLYPAQJ3G3woZiDZZciFtdVYiC15kpysK_rfR8kfSNPOJ1Pke3yPFPTKclVwFjPDf7dc5vxB4F5jvZPYWiXb-onrnQT6MLdKQwZoIKvwZThuUFuRiwalha1LbjXmB67XWfqxYOMKNhbp4GNy3TQuvZaFVqJw4Sc1WepqUmm5lliB79KVZTBdra5hG4NquIRKyAOchC1hz63jwNrA1Wa7_g4oC_i5ffzS4bYwna4czOCRg2jgGgwnD2UPGqtWK8ioihMaO3qrAJvD2J5oNjEU6iQrhcVFyoOFNBar6jJ17czGUGErqS0Oxt1AHJBMxiTGGCjQouvqxnBV8KPISbF8Vr0Vd-xbkRs0B3imhaWZR515XU07Ru_EcNSjVQIZlQu0vOhGl2ZnxE6Jk9lodRQFH0fgxn0h2JUJKayjghN_Ogp-6kpyzd3Z-U4Tt9EEjZDqHc0Ukn6X-VmyXzOl6IKW1MXxn8RwHZMO6RV4g91dTR_81pPWWFVTWLzr1M8frXVrf4f4ljymD_-XPvD98B7G6NLHmOhFnbGeUYyj8zVNa8pPxmpKdRpzO8kmbK9FwWKrWz5hRFSjC9mrzzNb8prvWEy_BerDju3kG2FI_S-l6gGmVbsvWfyMlaGobejGDA_BeKpJFNeJaqVl8d1sGXgWFr-yPxQHN9H9MgrC23kwD4LZcsJeWLyY31AYLuZ30Xx2H4TR24T99W1vb8L7RRiF0WwRzcJwOZtPmHtwNi8yH0TxQtCL89g9T_6VGqR99Zle2ds_Erd8TA)
# Участники процесса
## Tracker API
Бекенд размещенный на CloudFlare workers. Трекеров у нас может быть большое количество, на разных доменах и в разных роутах. Поэтому необходима реализация в виде отдельного worker скрипта.

Сценарии использования:

`casino.com/my-internal-link?stag=xxx&utm=yyy` => `static-assets.com/casino.apk`

`track.sub-domain.com/?stag=xxx&utm=yyy` => `static-assets.com/casino.apk`

Воркер может быть опубликован как на поддомене, так может быть и опубликован внутри вложенного роута продукта.
При получении входящего запроса воркер записывает в KV или DO ip адрес и search params которые были в ссылке, после чего редиректит на апк полученное из ответа или из конфигурации.

Важно: воркер может находится в аккаунте отличном от аккаунта разворачивания Internal API, поэтому необходимо использовать HTTP API для доступа к Internal API. Это решает пробелму того, что апк может быть опубликованно внутри продукта, который находится на другом CF аккаунте, нежели основное апи мобильного приложения.

Важно: воркер должен сохранять копию всех запросов которые проходят через него в таблице редиректов в BQ (включая информацию по юзерагенту и т.п.)

Возможные проблемы: не факт, что для workera можно указать ipv4 only режим. В этом случае необходимо оставить инструкцию для активации ipv4 на домене целиком. (оптимально использовать только ipv4 адреса для детектирования пользователей)

Предполагаемые настройки worker:
```toml
tracker-api=https://some-api-url.com/path
bq-settings=standart bq env
```
## Static
Это R2 хранилище для публикации статичных ассетов. Мы хотим внутри публиковать APK. R2 мы выбираем по причине того, что APK больше 10 мегабайт, а значит публиковать его через pages или workers нельзя. При этом R2 имеет бесплатный ergress трафик, то есть нам просто еще и дешево размещать тут большие файлы. Так же мы можем подкрепить static под разные домены. 

Так же мы можем напрямую в R2 публиковать артефакты сборки в виде APK.
## Internal API
Внутренне апи приложения. Это апи поддерживает несколько методов.
### product-manifest.json
Это даже не апи метод, а фактически просто статичный json, внутри которого у нас лежит что-то вроде:
```json
{
"version": 1.0.2,
"url": "https://static.com/casino.apk"
}
```
Мы можем положить по одному такому манифесту на каждый продукт, к примеру `cat-manifest.json`, `daddy-manifest.json`
### /api/click
АПИ метод который мы вызываем при старте приложения.
Payload:
```json
{  
  "custom_data": {  // appsflyer data, json field
    "installConversionData": {  
      "install_time": "2023-12-22 13:05:47.167",  
      "af_status": "Organic",  
      "af_message": "organic install",  
      "is_first_launch": false  
    },  
    "appOpenAttribution": {},  
    "deepLinking": "DeepLinkResult:{\"status\":\"NOT_FOUND\",\"error\":null,\"deepLink\":null}"  
  },  
  "appsflyerId": "1703250345065-1975383134428243633",  
  "appId": "com.bigfishbonansa.app",  
  "deviceId": "MTcwMzI1MDM0NTIzOC1lbl9VUy0xMDgwLjAtMjIzNi4wLTIzMTU2Mw==", //custom device id, ubiq for each instalation  
  "network_connection_type": "wifi",  
  "battery_level": "100%",  
  "device": {  // данные об устройстве
    "device_dpr": 2.75,  
    "device_width": 1080,  
    "device_height": 2236,  
    "model": "Pixel 4",  
    "os_name": "android",  
    "version": "31",  
    "manufacture": "Google",  
    "device_uuid": "unknown"  
  },  
  "fcmid": "e_fAofeISYSkAaPtWODypM:APA91bHHqT9bVz09sZ3PgIehsJBAqATdc2UOBISGX_YSm8iHyOsdqxz6nQvbv4zHqqofX1dUa3FqPNwH6oB1TZdonGpcsiBh_lMRrcND0cwiSe7R5gkntKfT-LUah5RW4su2vJ2inO1L", // firebase messaging token  
  "platform": "android", //ios
  "adsUrl": "", // если мы уже ранее имели сохраненную ссылку, то мы передаем ее тут
  "appVersion": "0.0.6+81",  
  "currentLocale": "en_US",
  "retry": 1,
}
```
Важные для нас поля:
- appID: на основании этого поля мы понимаем к какому бренду принадлежит приложение, и определяем на какое зеркало отправить пользователя
- deviceId: позволяет нам трекать уникальные установки
- adsUrl: тут мы получаем информацию, какое поле ранее было доступно у человека
- retry: это поле имеет значение отличное от 0, если урл полученный от апи не получилось открыть. То есть при запуске приложение имеет значение 0. Но если ссылку из adsUrl не получилось открыть с кодом 200, то делается повторный запрос с кодом 1, 2 и так далее. За счет этого поля мы можем настроить фолбек для пользователей в случае блокировок зеркал.

В ответ мы получаем json вида:
```json
{
  action: 'redirect', 
  payload: 'https://casino.com/?stag=xxx', 
  force: true 
}
```
где:
- action: это целевое действие, сейчас у нас это всегда redirect
- payload: это урл который необходимо открыть. Важно: если у нас есть url params связанные с ип с которого идет запрос, то мы должны добавить их к урл из настроек.
- force: если true, то всегда будет открыт последний полученный payload. Если false - то будет использован последний сохраненный в приложении в adsUrl ранее. Сейчас мы всегда будем возвращать true.

Информация о всех запросах должна трекаться в отдельную таблицу в BQ.

Пример конфига:
```json
{
com.catcasino.app: 
["https://catcasino.com","https://mirrorcat.com","https://superrare.fallback.com"],
casino.daddy.app: ["https://daddy.casino","https://daddy-mirror.com"],
/// ...
}
```
В конфиге для каждого зеркала мы указываем какой урл открывать в 0ю попытку, 1ю, 2ю и т.д.
### /api/track
Апи метод, в который треккер будет отстукивать пользователей, проходящих через него.
payload:
```json
{
ip: "193.233.22.2",
host: "https://originhost.com",
params: "?stag=xxx&utm=yyy",
timestamp: 2323233,
prodcut: "kent"
}
```
При этом response может быть просто 200, или не 200 со ссылкой на apk в body:
```json
{
apk: "https://static.casino.com/android.apk"
}
```
Цель этого метода, сохранить данные для атрибуции пользователя к партнеру. Окно атрибуции - 1 день. У нас нет нужды хранить более дня информацию о кликах.

Важно: если использовать KV - окно его синхронизации между узлами CF может занимать до минуты.

[KV eventually consistent](https://developers.cloudflare.com/kv/concepts/how-kv-works/#consistency)

> In other global network locations changes may take up to 60 seconds or more to be visible as their cached versions of the data time-out

У нас уже возникали с этим проблемы на apay, и решением было отказаться от KV в сторону [Durable Object](https://developers.cloudflare.com/durable-objects/). [Пример реализации](https://github.com/apayments/apayments.src/blob/main/durable-object-kv/src/index.ts). DO хорош тем, что он consistant в любой момент времени, но при этом у него нет механизма expire встроенного, как в KV. Поэтому при использовании DO необходимо самим реализовать expire.

## Casino
Это сайт нашего казино. Со стороны казино необходимо сделать одну доработку — для авторизированных пользователей передавать ид пользователя.
```javascript
function passData(uid: string) {
typeof Android == "undefined" || Android.sendData !== "function") return;
Android.sendData(JSON.stringify({uid}))
}
```
Такого метода как `Android.sendData` нет в браузере, мы будем инджектить его из кода приложения. Если SG согласны, было бы хорошо расширить метод некоторыми дополнительными данными:
`Android.sendData(JSON.stringify({uid, email, tags}))`, в этом случаем мы не будем зависеть от интеграции customer.io и fundist, т.к. теги будут попадать к нам напрямую от браузера пользователя.

### блокировка не мобильных пользователей
Для блокировки не мобиьных пользователей мы будеми использовать block правило следующего вида:
```
(all(http.request.headers["origin"][*] ne "https://catcasino.com"))
```
Для веб клиента невозможно будет указать origin не совпадающий с hostname проекта, в то время как для мобильного клиента - вполне себе возможно.
## CustomerIO
Это платформа по интеграции user journey automation в продукт. Мы хотим использовать ее для отправки пушей на устройства пользователей. Для корректной сегментации нам необходимо в мобильное приложение интегрировать SDK. [Customer IO Android SDK](https://docs.customer.io/integrations/sdk/android/tracking/identify/). Для корректной работы с пушами нам необходимо выполнить два условия.
###  Идентификация пользователя
```dart
CustomerIO.instance.identify(userId: uid, traits: {
  "email": user.email,
});
```
Минимально необходимый для нас параметр это userID (он же fundist id). Остальные параметры не обязательны.
### Отправка FCMID
Необходимо привязывать FCMID (если он есть) к FirebaseMessagingService предоставляемому со стороны customer.io
[docs](https://docs.customer.io/integrations/sdk/flutter/push-notifications/push-setup/)

## Android App

Собственно говоря, сама андроид аппка. Flutter приложение, с интегрированными сдк и вебвью для открытия сайта казино.

Приложение должно поддерживать несколько специальных сценариев.

### Обновление версии
Если при старте приложения в манифесте будет обнаружена версия новее текущей версии приложения - необходимо показать экрана с предложением обновиться и кнопкой скачать (которая будет вести на новое апк)
### Ротация служебных доменов
В приложение должно быть встроен список служебных доменов (к примеру env переменная со списком через запятую). При старте приложения необходимо проверять наличие манифеста в формате /имяПродукта-manifest.json и ссылку для показа человеку через апи. Если при старте приложения не выходит получить ответ ни от одного из служебных доменов - необходимо показать экран о недоступности интернета. Домены предлагается перебирать последовательно.
### Retry for target domain
Если урл который нами получен в payload не выходит открыть (статус код отличны от 200), то мы повторить запрос на /api/click с параметром retry=1 и так далее, в зависимости от номера попытки.
### свайп движения
Мы хотим иметь два специальных движения свайпом
- свайп сверху вниз - должен запускать рефреш страницы
- свайп справа налево (назад) - должен вызывать действие back в history браузера
### специальный заголовок
Для того, чтобы нас не блокировал сайт (специальные зеркала под моб устройства не должны открываться из веба), нам необходимо использовать специальный признак по которому нас будет отключать WAF на уровне зеркала. 
Увы при замене useragent ломается гугл авторизация, поэтому мы вынуждены идти другим образом. При добавлении специального заголовка ломается CORS на API продукта.

Поэтому, для того чтобы идентифицировать себя я предлагаю использовать заголовок origin. В нем мы будем указывать основной домен продукта, хотя открывать будем фактически зеркало. Повторить такое поведение в браузере нельзя, поэтому обычных посетителей домена будет блокировать.
### список SDK
В приложение необходимо интегрировать несколько SDK:
- appsflyer sdk - сдк для аналитики
- firebase messaging sdk - необходим для отправки пушей
- customer.io sdk - необходим для интеграции с кастомером

### flavors
Нам необходимо сделать пачку однотипных приложений:
- daddy
- kent
- gamma
- cat
- kometa
- arkada
- alov
- mers
Фактически они отличаются только именем продукта, токенами для кастомера и фаербейса, логотипом и экраном который мы показываем при отсутствии интернета.

Хотелось бы переиспользовать кодовую базу во всех приложениях, просто подменяя параметры сборки (env и ассеты) в зависимости от ключа сборки.