У нас есть temporal.io для работы с джобами. https://nodeart.app/git/projects/CAT/repos/cat.workflows/browse

Мы хотим в нем добавить новый источник данных для фетчинга. Называется HG.

Документация по апи методу:
[712020d95759a9120b466097afda31e0fea4d1_Transaction_Filter_API_for.pdf](./attachments/712020d95759a9120b466097afda31e0fea4d1_Transaction_Filter_API_for.pdf)

Доступ осуществляется по Bearer токену, для его получения необходимо выполнить запрос вида:

```bash
curl --request POST \ --url https://kc.prod.movepay.online/realms/master/protocol/openid-connect/token \ --header 'content-type: application/x-www-form-urlencoded' \ --data grant_type=password \ --data client_id=psp-core \ --data username=analytics@cat-infra.pp.ua \ --data 'password=xxx'
```
В ответ прийдет access_token. Время жизни токена - 30 минут, необходимо реализовать механизм обновления токена по его истечении.

Внутри апи лежат транзакции. Есть несколько неясностей с транзакциями, к примеру:
есть фильтр по updatedAtTo и updatedAtFrom, который поидее должен передавать обновления уже существующих транзакций, но нет уверенности что в него будут приходить обновления статусов (необходимо проверить).

Важно: транзакция может иметь разные этапы жизненного пути, то есть с одним id у нас может прийти несколько объектов. Поэтому уникальный ключ по бд это updatedAt + ID скорее всего. Вместе с RAW загруженными данными мы должны иметь так же отдельный view, который будет показывать конечные статусы транзакций.

Важно: в документе не отражено как работают статусы по chargeback и rollback транзакций, возможно их нужно из другого места получать.




