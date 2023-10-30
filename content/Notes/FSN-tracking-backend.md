# Проблема:
Для покупки рекламы в Facebook критически важно отдавать в фейсбук данные по эффективности (регистрации, депозиты, они же покупки). Без этого фейсбук не может оптимизировать аудиторию для показа рекламы и закупка медийки становится бесполезной.
### Решение
В таблице [clicks](https://mb.nodeart.app/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjo1OSwicXVlcnkiOnsic291cmNlLXRhYmxlIjo0OTMzLCJvcmRlci1ieSI6W1siZGVzYyIsWyJmaWVsZCIsODM5MjYsbnVsbF1dXX0sInR5cGUiOiJxdWVyeSJ9LCJkaXNwbGF5IjoidGFibGUiLCJ2aXN1YWxpemF0aW9uX3NldHRpbmdzIjp7fX0=) мы должны добавить поле appsflyer-installation-id. Наличие этого поля позволит нам интегриоваться с S2S событиями в appsflyer.
[Документация](https://dev.appsflyer.com/hc/reference/post_s2s_inappevent)
Мы должны добавить энпоинт для получения конверсии от треккера.
Пример аналогичного функционала у конкурентов [keitaro](https://telegra.ph/Integraciya-s-TD-APPS-Keitaro-08-23), [binom](https://telegra.ph/Integraciya-s-TD-APPS-Binom-08-23)

Наш бекенд должен принимать постбек от треккера (keitao, binom etc) и по данным из постбека находить этого же пользователя в таблице clicks, забирать из нее данные по усторойству необходимые для s2s вызова и отправлять в appsflyer с типом события (рег\депозит) и его суммой из постбека в аппсфлаер


#### Data
#### Clicks
- date: datetime
- Data: json
- AdsUrl: presaved url in app
- AppsflyerID: app id in appsflyer
- FCMID[](): Firebase Cloud Messaging Token
- Device: record with device info
- AppBundleID: application bundle id (to split different apps)
- AppVersion: build version
- ClienToken: app uniqu install id
- NetworkConnetcionType: 
- Advertisement: IOS/Android tracking id
- Battery Level:
- IP: client IP
- User Agent:
- Longitude:
- Latitude:
- Continent:
- ASN:
- Country:
- City:
- TimeZone:
- TrackerError
- TrackerUrl:

##### AppsFlyer
