## Goal: показать где и как должны быть сложены assetlinks

Мы сделали мобильные приложения, которые, в том числе должны перехватывать попытку открыть сайт казино на телефоне, и открывать только мобильное приложение (уже с открытым path внутри). Сценарий перехвата ссылки необходим для того, чтобы работала авторизация по активационной ссылке в мобильном приложении. То есть человек ставит мобильное приложение, человек регистрируется в мобильном приложении, в почту приходит ссылка на активацию, при открытии ссылки на активацию человека пересылает в мобильное приложение, внутри которого будет открыта ссылка на активацию (которая сразу его и залогинит)

### Мобильные APK
- https://shadowroute.co/kent
- https://shadowroute.co/cat
- https://shadowroute.co/arkada
- https://shadowroute.co/daddy
- https://shadowroute.co/kometa
- https://shadowroute.co/gamma
- https://shadowroute.co/r7

По ссылкам можно скачать актуальную версию apk для установки.

Для активации на сайтах в специальном роуте должен лежать файл ассоциации с мобильным приложением. Этот файл имеет фиксированный путь - `/.well-known/assetlinks.json` и должен быть статичным.

Примеры путей к файлам:

- **arkada** https://arkada.casino/.well-known/assetlinks.json https://b7qmw.app/.well-known/assetlinks.json

- **cat** https://catcasino.com/.well-known/assetlinks.json https://cf9du.app/.well-known/assetlinks.json

- **daddy** https://daddy.casino/.well-known/assetlinks.json https://h2lmv.app/.well-known/assetlinks.json

- **gama** https://gama.casino/.well-known/assetlinks.json https://n4xet.app/.well-known/assetlinks.json

- **kent** https://kent.casino/.well-known/assetlinks.json https://yw7pz.app/.well-known/assetlinks.json

- **kometa** https://kometa.casino/.well-known/assetlinks.json https://xq8rk.app/.well-known/assetlinks.json

- **r7** https://r7.casino/.well-known/assetlinks.json https://vz0ul.app/.well-known/assetlinks.json

На сайтах, которые лежат на CloudFlare эти файлы уже лежат, но в связи с переездом на fastly, все места где у нас зеркала обслуживаются на faslty не имеют необходимых файлов, поэтому мы просим положить их как статику, и тогда мы отключим подмену файлов со стороны CF

## Файлы assetlinks
Все файла это корректный json, и они должны иметь корректный content-type заголовок, сам json стоит минифицировать, убрав лишние пробелы и т.п.
### arkada
```json
[  
  {  
    "relation": ["delegate_permission/common.handle_all_urls"],  
    "target": {  
      "namespace": "android_app",  
      "package_name": "arkada.mobile",  
      "sha256_cert_fingerprints":  
        ["F2:7D:EF:DD:04:AC:35:AC:3F:20:8E:CD:CB:A8:BE:09:FC:A8:85:AB:38:48:AA:C5:5D:23:7A:F9:D1:FC:44:0D"]  
    }  
  }  
]
```

### catcasino
```json
[  
  {  
    "relation": ["delegate_permission/common.handle_all_urls"],  
    "target": {  
      "namespace": "android_app",  
      "package_name": "cat.mobile",  
      "sha256_cert_fingerprints":  
        ["F2:7D:EF:DD:04:AC:35:AC:3F:20:8E:CD:CB:A8:BE:09:FC:A8:85:AB:38:48:AA:C5:5D:23:7A:F9:D1:FC:44:0D"]  
    }  
  }  
]
```

### daddy
```json
[  
  {  
    "relation": ["delegate_permission/common.handle_all_urls"],  
    "target": {  
      "namespace": "android_app",  
      "package_name": "daddy.mobile",  
      "sha256_cert_fingerprints":  
        ["F2:7D:EF:DD:04:AC:35:AC:3F:20:8E:CD:CB:A8:BE:09:FC:A8:85:AB:38:48:AA:C5:5D:23:7A:F9:D1:FC:44:0D"]  
    }  
  }  
]
```

### gama
```json
[  
  {  
    "relation": ["delegate_permission/common.handle_all_urls"],  
    "target": {  
      "namespace": "android_app",  
      "package_name": "gama.mobile",  
      "sha256_cert_fingerprints":  
        ["F2:7D:EF:DD:04:AC:35:AC:3F:20:8E:CD:CB:A8:BE:09:FC:A8:85:AB:38:48:AA:C5:5D:23:7A:F9:D1:FC:44:0D"]  
    }  
  }  
]
```

### Kent
```json
[  
  {      "relation": ["delegate_permission/common.handle_all_urls"],  
    "target": {  
      "namespace": "android_app",  
      "package_name": "kent.mobile",  
      "sha256_cert_fingerprints":    ["F2:7D:EF:DD:04:AC:35:AC:3F:20:8E:CD:CB:A8:BE:09:FC:A8:85:AB:38:48:AA:C5:5D:23:7A:F9:D1:FC:44:0D"]  
    }  
  }  
]
```


### Kometa
```json
[  
  {  
    "relation": ["delegate_permission/common.handle_all_urls"],
    "target": {  
      "namespace": "android_app", 
      "package_name": "kometa.mobile",  
      "sha256_cert_fingerprints": ["F2:7D:EF:DD:04:AC:35:AC:3F:20:8E:CD:CB:A8:BE:09:FC:A8:85:AB:38:48:AA:C5:5D:23:7A:F9:D1:FC:44:0D"]  
    }  
  }  
]
```

### R7
```json
[
  {
    "relation": ["delegate_permission/common.handle_all_urls"], 
    "target": {  
      "namespace": "android_app",  
      "package_name": "r7.mobile",  
      "sha256_cert_fingerprints":["F2:7D:EF:DD:04:AC:35:AC:3F:20:8E:CD:CB:A8:BE:09:FC:A8:85:AB:38:48:AA:C5:5D:23:7A:F9:D1:FC:44:0D"]  
    }  
  }  
]
```