# Wrapper
## Цель
Сделать враппер, который бы осуществлял доставку контента в приложения с рекламой, и был бы переиспользован в большом количестве проектов.
## Критерии
- модифицируемость, мы не можем делать copy/paste из проекта в проект. Мы каждый раз должны в него вносить изменения, помимо названий функций. К примеру менять контракт вызова бекенда или порядок вызовов.
- размер, для нас важен маленький размер приложения. Чем он меньше, тем лучше.

## Flows
- [[ip-track]]
- [[attribution-track]]
## User stories
### New user, organic, pass check
Пользователь находит приложение в Маркете минуя рекламную ссылку, устанавливает приложение. Внутри приложения получает статус organic. Проверка IP разрешает показ рекламы, человек видит рекламу.
### New user, ad-link, pass check
Пользователь находит приложение в Маркете через рекламную ссылку, устанавливает приложение.  Его ip матчится с базой рекламных ссылок. Проверка IP разрешает показ рекламы, человек видит рекламу согласно данным полученных от трека рекламной ссылки.
### New user, ad-link, pass check
Пользователь находит приложение в Маркете через рекламную ссылку, устанавливает приложение.  Его ip НЕ матчится с базой рекламных переходов. Проверка IP разрешает показ рекламы. Мы ждем первого события — получение данных об атрибуции либо 5 секунд. Делаем запрос на получение реклманой ссылки и показываем рекламу (согласно атрибуции или фолбек)
### New user, ad-link, not pass check
Пользователь находит приложение в Маркете через рекламную ссылку, устанавливает приложение.  Его ip матчится с базой рекламных ссылок. Проверка IP НЕ разрешает показ рекламы, человек видит игру.
### New user, organic, not pass check
Пользователь находит приложение в Маркете минуя рекламную ссылку, устанавливает приложение. Внутри приложения получает статус organic. Проверка IP НЕ разрешает показ рекламы, человек видит игру.
### Returning user (activated), ad-link, not pass check
Если у нас уже ранее был получен урл, то независимо от ответа сервера мы показываем сохраненный урл, и уже после можем отправлять атрибуцию на бекенд.
### Returning user (not activated), ad-link, not pass check
Если у нас ранее не был получен урл рекламы, то мы выполняем запрос на проверку атрибуции, и если запрос не успешен то по прежнему показываем игру.
### Returning user (not activated), ad-link, pass check
Если у нас ранее не был получен урл рекламы, то мы выполняем запрос на проверку атрибуции, и если запрос успешен то по показываем полученную ссылку с рекламой.


## SDK которые необходимо интегрировать
Необходимо интегрировать
- [appsflyer SDK](https://pub.dev/packages/appsflyer_sdk)
- [firebase cloud messaging sdk](https://pub.dev/packages/firebase_messaging)
- [IDFA плагин](https://pub.dev/packages/app_tracking_transparency)

При этом appsflyer должен обрабатывать deeplinks (корректно настроить перехват и permission для андроида)

Настраивать интеграцию appsflyer и FCM (треккинг удаления приложения) необязательно.

> Важно: [все методы получения атрибуции](https://github.com/AppsFlyerSDK/appsflyer-flutter-plugin/blob/master/doc/Guides.md) (и старые и новые) должны быть реализованны

Запрос permissions на отправку пушей нужно делать сразу при старте приложения. Отказ от выдачи разрешений не должен блокировать запуск приложения.

> Важно: мы не хотим подключать никакие части Firebase кроме CloudMessaging (экономим место)

Запрос на IDFA необходимо делать сразу при старте приложения.

## Проверка на запуск рекламы
У нас есть три шага проверки на запуск рекламы.
### Проверка по ip при запуске
Мы делаем запрос на /sendAttribution со значением параметра `{"action": "launch"}`. Если в ответ приходит указание показать рекламу, показываем. Если приходит `{"action":"none"}`, то мы переходим к следующей проверке. В ответ мы получаем параметр timeout, который определяет сколько ждать таймаута для получения атрибуции.

Если это первый запуск приложения мы добавляем специальный параметр: launchNo: 1. Далее мы увеличиваем его на 1 при каждом запуске приложения.
## Проверка при получении атрибуции
Мы выполняем первое из двух условий: либо нам пришло два события об атрибуции от appsflyer, либо таймаут с прошлого этапа. 

Если отработал timeout, мы делаем запрос на получение рекламы с `{"action": "timeout"}`

Если ранее отработало получение атрибуции, мы делаем запрос на получение рекламы с `{"action": "attribution"}`
### Проверка при получении запоздавшей атрибуции
Если атрибуция пришла после события timeout, то мы отправляем еще один запрос, с action: "late_attribution". При отработке этого метода по-прежнему может быть запущенно рекламное приложение.

> Важно: url api метода должен быть вынесен в конфиг.

POST `/sendAttribution`
```json
{
  "data": {
	installConversionData: {...}, // appsflyer
	appOpenAttribution: {...}, // appsflyer
	deepLinking: {...} // appsflyer
	},
  "appsflyerid": "1690...", // отдельно appsflyer id
  "appsflyer_app_id": "com.bigfishbonansa.app", // appsflyer app id
  "imei": "IMEI not available due to new Android/iOS policies", // imei, может быть любая строка
  "idfa": "354AFFB6-5943-409D-8033-45FEF24C51AD", // "not iOS 14+", // idfa
  "idfv": "343ea5f8-ee87-525d-cbe3-2b7bcbe45b88", //
  "att": -1,
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
{"action": "none", "timeout": 5000}
```
В этом случаем мы запускаем игру, как обычно, и делаем запрос с передачей атрибуции максимум через 5 секунд. Поле "timeout" не обязательное, если оно отсутствует — по умолчанию 5000.
либо:
```json
{
  "action": "redirect",
  "payload": "https://offer.com", // куда отправить пользователя
  "preservSettings": true, // сохранять урл
  "resetSettings": true, // переписать уже существующие настройки
  "timeout": 5000 // этот параметр вообще тут не обязателен, но может быть
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

## flow
[![](https://mermaid.ink/img/pako:eNp9UsGOmzAU_BXrnbNREgcSOFRC2h572p6a5ODYJqAaGxlb2y3w733GoaWJWiSQPZ55M-_hHrgREnIolXnnFbOOfH09a4JP0bYnxbzm1YW8vHwa3qQWpHDODuRalj2-YyR2_nqzrK1IZEeQBFLQkaFuSWm8FsOdUIgucrBgXCyp2riZfmONPHWVeZ9Wl8iN3wBECWdKXRn_Towlrm6k8W4gDGOeuFFKcjdt6qt3tdGXh8SLozl2gKbKGCp02_NKYvUFc3xOX0yi9RplDz3-fRhiPw3Nydl70VeAiz-e_Q3HErD_BXnQ3D2HztVK4VwJE8RbNSxS_Esxz_zpl8EKGmkbVgu8M33AzuAqifUgx6WQJfPKneGsR6Qy78zbh-aQO-vlCnwr0PC1Zth6A3nJVIdoyzTkPfyAPN2sD3uaJmm2PdJdkmUr-IB8u07obkP39IhH9HDc7MYV_DQGKyCfJinNUro97I_bLEtmk8-idsb-9pDT9ku869OVn3y_TVViOGv8rboLxl_m-wE9?type=png)](https://mermaid.live/edit#pako:eNp9UsGOmzAU_BXrnbNREgcSOFRC2h572p6a5ODYJqAaGxlb2y3w733GoaWJWiSQPZ55M-_hHrgREnIolXnnFbOOfH09a4JP0bYnxbzm1YW8vHwa3qQWpHDODuRalj2-YyR2_nqzrK1IZEeQBFLQkaFuSWm8FsOdUIgucrBgXCyp2riZfmONPHWVeZ9Wl8iN3wBECWdKXRn_Towlrm6k8W4gDGOeuFFKcjdt6qt3tdGXh8SLozl2gKbKGCp02_NKYvUFc3xOX0yi9RplDz3-fRhiPw3Nydl70VeAiz-e_Q3HErD_BXnQ3D2HztVK4VwJE8RbNSxS_Esxz_zpl8EKGmkbVgu8M33AzuAqifUgx6WQJfPKneGsR6Qy78zbh-aQO-vlCnwr0PC1Zth6A3nJVIdoyzTkPfyAPN2sD3uaJmm2PdJdkmUr-IB8u07obkP39IhH9HDc7MYV_DQGKyCfJinNUro97I_bLEtmk8-idsb-9pDT9ku869OVn3y_TVViOGv8rboLxl_m-wE9)

## Cases
### first launch
- "ads_url": "",
- "action": "launch",
- "last_url": ""
- "launchNo": 1
### first launch, attribution
- "ads_url": "", // will hav url, if we got ads on first launch
- "action": "attribution",
- "last_url": ""
- "launchNo": 1
### second launch, not activated
- "ads_url": "",
- "action": "launch",
- "last_url": ""
- "launchNo": 3
### second launch, attribution, not activated
- "ads_url": "",
-  "action": "attribution",
- "last_url": ""
- "launchNo": 4
### second launch, activated
- "ads_url": "https://offer.com", // url which was used for activation
-  "action": "launch", 
-  "last_url": "https://offer.com/deposit" // url where user closed an app
- "launchNo": 2
### second launch, attribution, activated
- "ads_url": "https://offer.com", // url which was used for activation
- "action": "attribution",
- "last_url": "https://offer.com/deposit" // url where user closed an app
- "launchNo": 3
