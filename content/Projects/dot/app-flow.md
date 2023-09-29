User flow for apps and tracker integration
[![](https://mermaid.ink/img/pako:eNp1U82OmzAQfhXL15KUEJIAh5UqbY899VZxmZghsRZs6p_dplHepc_SJ-vYQJSlWonDMHx_Hg9XLnSDvOIWf3pUAp8lnAz0taoVeKeV749oqBZOG-ZtqAcwTgo5gHIMhuF949i27xvOgHgJtFr9_cPoCSKrpydiWtLEiukBFQMVtNgAJ5yBM4LAkVQxqayDrhtd_weFuu0uAWlRNQycM_LondR3c6Udsg5bx3T7iNfmBEqKz9Awq70R-JiWkfingL4nsIsId8SsB_aFtTQxuCdgDTh4TB2RgbgapafIH-FHCxrvhGzwVQqMKBreh0wijMzpHia2HCIqWdIYOjFTJ8YYMRqDsq_UeJPuTP6Crlir0bxh3nQLz8dzSUXT6CH6wFF7t-CL-MVpNqAJyMXBV9MC2LN-Y7KlBcUxxZdnnvAeSVo2tMXXWjFWc3fGHmteUdlgC75zNa_VjaBhp79flOCVMx4T7gc69bz0vGqhs9SlxeXVlf_i1eawTstddkg3-bYo811RJPzCqyJf79PDIcvLssy3-0N-S_hvrUkhXRdFlqfb3S7bbbNNuZ89vjaS9vRugfH12_jzxX8w2v6IIiHb7R964Uwp?type=png)](https://mermaid.live/edit#pako:eNp1U82OmzAQfhXL15KUEJIAh5UqbY899VZxmZghsRZs6p_dplHepc_SJ-vYQJSlWonDMHx_Hg9XLnSDvOIWf3pUAp8lnAz0taoVeKeV749oqBZOG-ZtqAcwTgo5gHIMhuF949i27xvOgHgJtFr9_cPoCSKrpydiWtLEiukBFQMVtNgAJ5yBM4LAkVQxqayDrhtd_weFuu0uAWlRNQycM_LondR3c6Udsg5bx3T7iNfmBEqKz9Awq70R-JiWkfingL4nsIsId8SsB_aFtTQxuCdgDTh4TB2RgbgapafIH-FHCxrvhGzwVQqMKBreh0wijMzpHia2HCIqWdIYOjFTJ8YYMRqDsq_UeJPuTP6Crlir0bxh3nQLz8dzSUXT6CH6wFF7t-CL-MVpNqAJyMXBV9MC2LN-Y7KlBcUxxZdnnvAeSVo2tMXXWjFWc3fGHmteUdlgC75zNa_VjaBhp79flOCVMx4T7gc69bz0vGqhs9SlxeXVlf_i1eawTstddkg3-bYo811RJPzCqyJf79PDIcvLssy3-0N-S_hvrUkhXRdFlqfb3S7bbbNNuZ89vjaS9vRugfH12_jzxX8w2v6IIiHb7R964Uwp)
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