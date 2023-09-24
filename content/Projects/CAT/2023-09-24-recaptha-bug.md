## Задача
Мы делаем большое количество лендингов наподобие [такого](https://cat-cas-lands.pages.dev/de). Отличие этих лендингов от обычных - наличие на них формы регистрации. Нам важно чтобы пользователи на лендингах не послужили причиной отказа основного проекта (не за DDOS или его). На бекенде реализован функционал RateLimit, который запрашивает Recapthca при достижении лимита. Мы бы хотели его использовать и на лендингах.

## Документация
Доступная нам информация:
[api url: test-catcasino.egamings.com](https://wlc:wlcdemo@test-catcasino.egamings.com/)
[документация](https://docs.egamings.com/wlc_core/)
Список использованных методов api:
- GET /countries
- GET /currencies
- PUT /profiles/phone
- PUT /profiles/email
- PUT /profiles/promocode
- POST /validate/user-register
- POST /profiles
Предполагается что GET методы не имеют rate limit (статичны)
PUT методы имеют Rate Limit
POST методы имеют жесткий Rate Limit

## Документация по ReCaptha
![[Capthca-common-settings.pdf]]
![[Captcha-google-settings.pdf]]
Вместе с документацией получены ключи для Google ReCaptcha
[Ключи](https://bit.nodeart.app/#/send/pXoEfqSCTsynqqi3atVF4Q/UvmumN99Rq0Y3Dy5XQRmjg) (пароль для доступа CAT)

## Что есть:
Есть форма опубликованная на лендинге [ссылка](https://cat-cas-lands.pages.dev/en)
При достижении лимита запросов на `POST /profile` приходит код ответа `429`, но при этом отсутствует приведенный в документации заголовок `Retry-After`
На иные апи методы ограничения также не обнаружены