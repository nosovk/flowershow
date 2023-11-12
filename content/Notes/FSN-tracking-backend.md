# Проблема:
Для покупки рекламы в Facebook критически важно отдавать в фейсбук данные по эффективности (регистрации, депозиты, они же покупки). Без этого фейсбук не может оптимизировать аудиторию для показа рекламы и закупка медийки становится бесполезной.
### Flow
https://obs.nodeart.app/Projects/dot/app-flow
### Решение
В таблице [clicks](https://mb.nodeart.app/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjo1OSwicXVlcnkiOnsic291cmNlLXRhYmxlIjo0OTMzLCJvcmRlci1ieSI6W1siZGVzYyIsWyJmaWVsZCIsODM5MjYsbnVsbF1dXX0sInR5cGUiOiJxdWVyeSJ9LCJkaXNwbGF5IjoidGFibGUiLCJ2aXN1YWxpemF0aW9uX3NldHRpbmdzIjp7fX0=) мы должны добавить поле appsflyer-installation-id. Наличие этого поля позволит нам интегрироваться с S2S событиями в appsflyer.
[Документация](https://dev.appsflyer.com/hc/reference/post_s2s_inappevent)
Мы должны добавить эндпоинт для получения конверсии от трекера.
Пример аналогичного функционала у конкурентов [keitaro](https://telegra.ph/Integraciya-s-TD-APPS-Keitaro-08-23), [binom](https://telegra.ph/Integraciya-s-TD-APPS-Binom-08-23)

Наш бекенд должен принимать постбек от трекера (keitao, binom etc) и по данным из постбека находить этого же пользователя в таблице clicks, забирать из нее данные по устройству необходимые для s2s вызова и отправлять в appsflyer с типом события (регистрация/депозит) и его суммой из постбека в аппсфлаер


#### Data
#### Clicks
- date: datetime, время события, utc-0
- Data: json, данные об атрибуции из appsflyer, разделены на 3 объекта - install, deeplink, attribution. Возможно вариант хранения в json не удобный и стоит разбить на 3 поля (наверное нужно)
- AdsUrl: presaved url in app, если наше приложение уже ранее получило оффер для показа, в этом параметре будет показано с каким офером приложение уже открылось. То есть если мы отдаем офер, он запоминается на устройстве. То есть все пользователи с непустым adsurl открывают приложение не в первые и уже получили разрешение на показ рекламы.
- AppsflyerID: app id in appsflyer, это внутренний userid от appsflyer, нужен для интеграции с его апи.
- FCMID: Firebase Cloud Messaging Token, этот токен необходим для отправки пушей, он обновляется с течением времени, нам всегда нужна последняя версия этого токена, вместе с информацией показываем ли мы рекламный офер устройству с этим токеном.
- Device: record with device info
- AppBundleID: application bundle id (to split different apps), все наши приложения должны писать результаты работы в одну бд, поле нужно, чтобы разделить данные по источнику.
- AppVersion: build version, нужно для отправки в appssflyer, данные атрибуции действительны только с той версией приложения с которой они были собранны
- ClientToken: app uniq install id, этот токен просто UUID, который мы генерируем при установке приложения и сохраняем. Фактически это intallation id приложения, сделан отдельно т.к. я не доверяю appsflyer.
- NetworkConnetcionType: 3g, edge, wifi. Пользователи с 3g и edge с меньшей вероятностью будут модераторами.
- Advertisement: IOS/Android tracking id, это IDFA токен, если он есть
- Battery Level: уровень батареи, если там 100% то это эмулятор и мы никогда не показываем рекламу
- IP: client IP
- User Agent: сохраняем для проформы, у нас это всегда dart
- Longitude:
- Latitude:
- Continent:
- ASN:
- Country:
- City:
- TimeZone:
- TrackerError: сюда мы сохраняем причину отказа от показа рекламы от трекера
- TrackerUrl: сюда мы сохраняем урл который мы получили от трекера

##### AppsFlyer
Для appsflyer есть два разных кейса.
- android IDS
- apple IDS
##### caveats
sharing_filter поле в appsflyer app traking необходимо ставить значение не all,  что было бы очевидно, а делать под запрос к appsflyer и получать список активированных интеграций. Этот список хранится в рамках одного аккаунта, поэтому для одного account id его вероятно можно закешировать.
поля вроде custom_data принимают json, не совсем ясно как принимать их от s2s


## Click modification
Согласно диаграмме, нам нужно расширять данные ссылки TrackerUrl при помощи ClickID и возможно других параметров. Я предлагаю принимать строку с параметрами для интерполяции, которые бы позволяли подставить любые данные из click info. Чтобы понимать итоговый результат — добавить в таблицу новую колонку, renderedTrackerUrl.
Так же при отдаче параметров от трекера, нам могут передать дополнительные параметры, param1, param2, param3 etc. Нам нужно их сохранять, т.к. они могут быть использованы для дальнейшей атрибуции.
Пример url:
от трекера нам приходит что-то вроде
```
https://supperoffer.com/clickid=${ClientToken}&AppBundleID=${AppBundleID}&param1=offerx&param2=offerXdescription
```
в примере выше ${ClientToken} должен быть подменен на ClientToken (фактически clickid), ${AppBundleID} на ид бандла, а offerx и offerxDesription должны быть сохранены для дальнейшего постбека о регистрации/покупке
Итоговый урл который мы после интерполяции отдадим клиенту должен сохранится в 'renderedTrackerUrl'
### s2s 
```
https://bff.com/event&eventName=purchase&eventType=purchase&eventCurrecny=USD&eventAmmount=4&clickid=984394892349398439&param1={param1}
```
Получив постбек, мы должны по clickid найти его в списке созанных трекинговых урлов, достать данные об устройсвте и отправить в appsflyer