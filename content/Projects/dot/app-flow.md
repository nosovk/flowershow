User flow for apps and tracker integration
[![](https://mermaid.ink/img/pako:eNrVVbFu2zAQ_RWCa53UcuTY1hCgaDpk6BR0KbTQ5MkmLJEqeUziBvn3HklZlWMURcduFO_de8d3R-qVS6uAV9zDjwBGwr0WOye62tRGBLQmdFtwtJZoHQs-rnvhUEvdC4NM9P35xrZpzjfQCXmIabWJ6Vd3d5TjiQ0qZnswTJjIwnqxA9IZYgRLahXTxqNo26w0Dcd10x4jxoNRTCA6vQ2oLUkZi8BaaJDZZoq0bieMlh-FYt4GJyFXxYjwQ8SNep7qUqlAP0qPqBOb8AfWkC9iVGZKoMh1JkxMucrEQ5GXyExLxg0YBU9aQoqnIi5yCJpzBm-HPN2n-Ox9AgOUtRmwuaAkJox_oo1njXvSlNQwa7KgYsG1o05G78CAEwiKfW61PDzcJ2jqxoWggzYh0TKN75qRyGSk0Cp7jHuqgkZGdJ7cR3C9zen0YamYb5Napl5qQ953IsmKrQ347hQyRYiiBxeRo9lXw2j5vX1muiFhyC58yodK1T2o2tge6Sw77cm8PFeMncaF9c6qILFinTiAZwaeL7ADJMHPe_W4eGS99bilzaxNouRIzJp0ajoWMQWewGDEnFozGcYd4PQO5E5sj_k0TKsZgxdMvmFuHa0j1Z-uyu8ZjkiftVnqyHh3EgWRtNbSDaa9vfDwN5emuH93aMZkcI6eqiMNetfZkA35v0zjM94Bza5W9PS-RsKa0zXooOYVLRU0IrRY89q8ETQ-xI9HI3mFLsCMh57KPL3UvGpE62mXXltevfIXXi2K62J5c1OUm1VRlIvVupzxI6-W5XWxWpW382I5p_Byfvs24z-tJYr59Wa9Xm3WxWZ5e1POF2V5UvmiNF3xUQTS59f8z0i_jiT8PbHE6t5-AWY2N7Q?type=png)](https://mermaid.live/edit#pako:eNrVVbFu2zAQ_RWCa53UcuTY1hCgaDpk6BR0KbTQ5MkmLJEqeUziBvn3HklZlWMURcduFO_de8d3R-qVS6uAV9zDjwBGwr0WOye62tRGBLQmdFtwtJZoHQs-rnvhUEvdC4NM9P35xrZpzjfQCXmIabWJ6Vd3d5TjiQ0qZnswTJjIwnqxA9IZYgRLahXTxqNo26w0Dcd10x4jxoNRTCA6vQ2oLUkZi8BaaJDZZoq0bieMlh-FYt4GJyFXxYjwQ8SNep7qUqlAP0qPqBOb8AfWkC9iVGZKoMh1JkxMucrEQ5GXyExLxg0YBU9aQoqnIi5yCJpzBm-HPN2n-Ox9AgOUtRmwuaAkJox_oo1njXvSlNQwa7KgYsG1o05G78CAEwiKfW61PDzcJ2jqxoWggzYh0TKN75qRyGSk0Cp7jHuqgkZGdJ7cR3C9zen0YamYb5Napl5qQ953IsmKrQ347hQyRYiiBxeRo9lXw2j5vX1muiFhyC58yodK1T2o2tge6Sw77cm8PFeMncaF9c6qILFinTiAZwaeL7ADJMHPe_W4eGS99bilzaxNouRIzJp0ajoWMQWewGDEnFozGcYd4PQO5E5sj_k0TKsZgxdMvmFuHa0j1Z-uyu8ZjkiftVnqyHh3EgWRtNbSDaa9vfDwN5emuH93aMZkcI6eqiMNetfZkA35v0zjM94Bza5W9PS-RsKa0zXooOYVLRU0IrRY89q8ETQ-xI9HI3mFLsCMh57KPL3UvGpE62mXXltevfIXXi2K62J5c1OUm1VRlIvVupzxI6-W5XWxWpW382I5p_Byfvs24z-tJYr59Wa9Xm3WxWZ5e1POF2V5UvmiNF3xUQTS59f8z0i_jiT8PbHE6t5-AWY2N7Q)
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