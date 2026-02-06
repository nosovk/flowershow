Input:
- [[2025-08-04-CAT-Mobile-app]]
- [[2025-09-23-asset-links-for-mobile]]
- [[2025-09-23-mobile-app-headers]]
- [[2026-01-28-cat-mobile-analitics]]

## schema
[![](https://mermaid.ink/img/pako:eNptk92OmzAQhV9l5Gs2SrLhzxcrRU0rRWrVSGm7UsWNgQmxAJsakzaN8u41BnahBG6wzzfHZ7B9I4lMkVBS468GRYI7zjLFykiwRkvRlDGqSIB5DgW7onp6eYHt6cSLnFH4WqGAb4oluZYKCi7yDu0BaOGujMIRRdqxOEY72ZK92NvqGToUT9gjuyAkBU9yYGYBprXicaO5FPOiSZTWF7SE2kTHWZZjO9snscQoh9UmhntRa1YUwKpq5rStqt7nTTVTnXTY0y4gSAM40MuH_bts-0vxwhPc7x6UW33W9cRiqzVLzmOo7XvqOQr8ivEPjr8pKMx4jRrarZUZ7617-b23uv2b5rCcAS8otN2Gpkb1MG6f5bvV2xi7aWujg2N5a66w1lDJWsfmVz1o8LOUOZxMTHsOeApcQBr_B1ZV_amwu2U9bdYOiQRxSKZ4SqhWDTqkRFWydkhuLRERfcYSI0LNZ8pUHpFI3E1NxcRPKcuhTMkmOxN6YkVtRk2VMj3cpjfELI3qg2yEJjS0DoTeyB9Cn_zlYu35q2DpB57nPT8b9Wqmw-XCdYONG27CVRhu_PXdIX_toqvFyt8EXui55l0HS3ftkPbOHq8iGdbDlJvD-qW74faiD8E-WqVPe_8HSaFFuw?type=png)](https://mermaid.live/edit#pako:eNptk92OmzAQhV9l5Gs2SrLhzxcrRU0rRWrVSGm7UsWNgQmxAJsakzaN8u41BnahBG6wzzfHZ7B9I4lMkVBS468GRYI7zjLFykiwRkvRlDGqSIB5DgW7onp6eYHt6cSLnFH4WqGAb4oluZYKCi7yDu0BaOGujMIRRdqxOEY72ZK92NvqGToUT9gjuyAkBU9yYGYBprXicaO5FPOiSZTWF7SE2kTHWZZjO9snscQoh9UmhntRa1YUwKpq5rStqt7nTTVTnXTY0y4gSAM40MuH_bts-0vxwhPc7x6UW33W9cRiqzVLzmOo7XvqOQr8ivEPjr8pKMx4jRrarZUZ7617-b23uv2b5rCcAS8otN2Gpkb1MG6f5bvV2xi7aWujg2N5a66w1lDJWsfmVz1o8LOUOZxMTHsOeApcQBr_B1ZV_amwu2U9bdYOiQRxSKZ4SqhWDTqkRFWydkhuLRERfcYSI0LNZ8pUHpFI3E1NxcRPKcuhTMkmOxN6YkVtRk2VMj3cpjfELI3qg2yEJjS0DoTeyB9Cn_zlYu35q2DpB57nPT8b9Wqmw-XCdYONG27CVRhu_PXdIX_toqvFyt8EXui55l0HS3ftkPbOHq8iGdbDlJvD-qW74faiD8E-WqVPe_8HSaFFuw)


В мобильном приложении необходимо добавить appsflyer.

Со стороны бекенда необходимо добавить 3 метода и их нужно вызывать из мобильного приложения:
### /api/open
Этот метод отрабатывает при открытии приложения, в него необходимо передавать:
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
}
```
### /api/attribution
Этот метод мы вызываем при получении данных об атрибуции со стороны appsflyer.
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
}
```
### /api/auth
Этот метод вызывается при авторизации человека внутри веб вью.
```json
{  
  "deviceId": "MTcwMzI1MDM0NTIzOC1lbl9VUy0xMDgwLjAtMjIzNi4wLTIzMTU2Mw==", //custom device id, ubiq for each instalation  
  "fcmid": "e_fAofeISYSkAaPtWODypM:APA91bHHqT9bVz09sZ3PgIehsJBAqATdc2UOBISGX_YSm8iHyOsdqxz6nQvbv4zHqqofX1dUa3FqPNwH6oB1TZdonGpcsiBh_lMRrcND0cwiSe7R5gkntKfT-LUah5RW4su2vJ2inO1L", // firebase messaging token  
  "userId": "3222323",
}
```


## Bridge
Необходимо сделать бриджа для получения данных userid из приложения внутри webview ([пример](https://obs.nodeart.app/Projects/CAT/2025-08-04-CAT-Mobile-app#casino)).