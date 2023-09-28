User flow for apps and tracker integration
[![](https://mermaid.ink/img/pako:eNp1U7mO2zAQ_RWCbWRHliXrKBYIsClTpQvUjKmRTaxMKjx24xj-l3xLviwj6oBXwQIqqOG7OBzeuNAN8opb_OlRCXyWcDJwqVWtQDhtmLdoatWDcVLIHpRj0PfvC8e2fV9wBsTLQKvV3z-MvkFk8_RETEuaWDHdo2KgBi3Wwwln4IwgcCBVTCrroOtG1_9Bw7rtrgPSomoYOGfk0TupF3OlHbIOW8d0-4jX5gRKis_QMKu9EfiYlpH4pwG9JLCrCAti1gP7wlrqGCwJWAMOHlMH5EDcjNJT5I_wowW1d0I2-CoFBhQ170MmEUbmdA8TW_YBFa1pDJ2YqRNjjBiMQdlXKrxJdyZ_QVes1WjeMG-6lefjuaSiblwg-MBRe7fii7DjNOvRDMjVwTfTANizfmOypaHEMcWXZx7xC5K0bGhyb7VirObujBeseUXLBlvwnat5re4EBe_096sSvHLGY8R9T6eeB51XLXSWqjS4vLrxX7za5du4zJI83qX7okyzooj4lVdFuj3EeZ6kZVmm-0Oe3iP-W2tSiLdFkaTxPsuSbJ_sysPs8bWRNKeLBYbfb-ODC-8u2P4IIkO2-z90Fkcd?type=png)](https://mermaid.live/edit#pako:eNp1U7mO2zAQ_RWCbWRHliXrKBYIsClTpQvUjKmRTaxMKjx24xj-l3xLviwj6oBXwQIqqOG7OBzeuNAN8opb_OlRCXyWcDJwqVWtQDhtmLdoatWDcVLIHpRj0PfvC8e2fV9wBsTLQKvV3z-MvkFk8_RETEuaWDHdo2KgBi3Wwwln4IwgcCBVTCrroOtG1_9Bw7rtrgPSomoYOGfk0TupF3OlHbIOW8d0-4jX5gRKis_QMKu9EfiYlpH4pwG9JLCrCAti1gP7wlrqGCwJWAMOHlMH5EDcjNJT5I_wowW1d0I2-CoFBhQ170MmEUbmdA8TW_YBFa1pDJ2YqRNjjBiMQdlXKrxJdyZ_QVes1WjeMG-6lefjuaSiblwg-MBRe7fii7DjNOvRDMjVwTfTANizfmOypaHEMcWXZx7xC5K0bGhyb7VirObujBeseUXLBlvwnat5re4EBe_096sSvHLGY8R9T6eeB51XLXSWqjS4vLrxX7za5du4zJI83qX7okyzooj4lVdFuj3EeZ6kZVmm-0Oe3iP-W2tSiLdFkaTxPsuSbJ_sysPs8bWRNKeLBYbfb-ODC-8u2P4IIkO2-z90Fkcd)

Where:
- bff: is a light proxy on app domain, which can store logs and responsible to integration with tracker (at this point we can change implementation without making changes to app in store)
- tracker: is a decision-making app, Binom or something else

What should we keep in mind:
- organic traffic should be excluded from AD at first step to avoid app block
- bff should be published at app domain with https/v3 enabled
- we show loading screen until we receive answer from BFF, if there is timeout (like 10 seconds or something like that) we go to normal game


## OnAttribution event
При получении данных по атрибуции из appsflyer мы хотим вызвать API

> Важно: url api метода должен быть вынесен в конфиг.

POST `/sendAttribution`
```json
{
  data: {... attribution data ...},
  advertisement: "", // idfa id
  appsflyerid: "1690...", // отдельно appsflyer id
  fcmid: "dwreerd", // token to send firebase notifications
  device: {
    device_dpr: 23, //
    device_width: 2323, //
    device_height: 2332, //
    model: "a10s", // phone model
    os_name: "android", // ios
    version: "SM-A107M",
    manufacture: "samsung",
  },
  app_bundle_id: "com.my.app", // id приложения в сторе для идентификации приложения
  app_version: "1.0.1", // версия нашего билда
  clienToken: "uuid", // у нас нет ид устройств, поэтоу это наш ид инсталяции, просто uuid сохраненный в appdata, чтобы он не менялся при перезапуске
}
```

В ответ мы получаем информацию о целевом действии:
```json
{"action": "none"}
```
В этом случаем мы запускаем игру как обычно.
либо:
```json
{
"action": "redirect",
"payload": "https://google.com"
}

https://docs.adspect.ai/ru/latest/