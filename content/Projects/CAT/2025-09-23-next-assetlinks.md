## Goal: показать где и как должны быть сложены assetlinks

Мы сделали мобильные приложения, которые, в том числе должны перехватывать попытку открыть сайт казино на телефоне, и открывать только мобильное приложение (уже с открытым path внутри). Сценарий перехвата ссылки необходим для того, чтобы работала авторизация по активационной ссылке в мобильном приложении. То есть человек ставит мобильное приложение, человек регистрируется в мобильном приложении, в почту приходит ссылка на активацию, при открытии ссылки на активацию человека пересылает в мобильное приложение, внутри которого будет открыта ссылка на активацию (которая сразу его и залогинит)

### Мобильные APK
- https://shadowroute.co/wagibet
- https://shadowroute.co/motor

По ссылкам можно скачать актуальную версию apk для установки.

Для активации на сайтах в специальном роуте должен лежать файл ассоциации с мобильным приложением. Этот файл имеет фиксированный путь - `/.well-known/assetlinks.json` и должен быть статичным.

Примеры путей к файлам:

- **wagibet:** https://wagibet.com/.well-known/assetlinks.json, https://wg8bt.app/.well-known/assetlinks.json

- **motor:** https://casibra.com/.well-known/assetlinks.json, https://motor.app/.well-known/assetlinks.json

На сайтах, которые лежат на CloudFlare эти файлы уже лежат, но в связи с переездом на fastly, все места где у нас зеркала обслуживаются на faslty или через free CF, не имеют необходимых файлов, поэтому мы просим положить их как статику, и тогда мы отключим подмену файлов со стороны CF.

## Файлы assetlinks
Все файлы это корректный json, и они должны иметь корректный content-type заголовок, сам json стоит минифицировать, убрав лишние пробелы и т.п.
### wagibet
```json
[  
  {  
    "relation": ["delegate_permission/common.handle_all_urls"],  
    "target": {  
      "namespace": "android_app",  
      "package_name": "wagibet.mobile",  
      "sha256_cert_fingerprints":  
        ["F2:7D:EF:DD:04:AC:35:AC:3F:20:8E:CD:CB:A8:BE:09:FC:A8:85:AB:38:48:AA:C5:5D:23:7A:F9:D1:FC:44:0D"]  
    }  
  }  
]
```

### motor
```json
[  
  {  
    "relation": ["delegate_permission/common.handle_all_urls"],  
    "target": {  
      "namespace": "android_app",  
      "package_name": "motor.mobile",  
      "sha256_cert_fingerprints":  
        ["F2:7D:EF:DD:04:AC:35:AC:3F:20:8E:CD:CB:A8:BE:09:FC:A8:85:AB:38:48:AA:C5:5D:23:7A:F9:D1:FC:44:0D"]  
    }  
  }  
]
```

### atom
```json
[  
  {  
    "relation": ["delegate_permission/common.handle_all_urls"],  
    "target": {  
      "namespace": "android_app",  
      "package_name": "atom.mobile",  
      "sha256_cert_fingerprints":  
        ["F2:7D:EF:DD:04:AC:35:AC:3F:20:8E:CD:CB:A8:BE:09:FC:A8:85:AB:38:48:AA:C5:5D:23:7A:F9:D1:FC:44:0D"]  
    }  
  }  
]
```