## Задача
Мы делаем большое количество лендингов наподобие [такого](https://cat-cas-lands.pages.dev/de). Отличие этих лендингов от обычных — наличие на них формы регистрации. Нам важно, чтобы пользователи на лендингах, не послужили причиной отказа основного проекта (случайно спровоцировав DDOS к примеру). На бекенде реализован функционал Rate Limit, который запрашивает ReCapthca при достижении лимита запросов. Мы бы хотели его использовать и на лендингах.

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
Есть форма опубликованная на лендинге: [ссылка](https://cat-cas-lands.pages.dev/en)

При достижении лимита запросов на `POST /profile` приходит код ответа `429`, но при этом отсутствует приведенный в документации заголовок `Retry-After`. Заголовки связанные с ReCapthca так же отсутствуют.

#### Пример запроса/ответа
```bash
curl 'https://test-catcasino.egamings.com/api/v1/profiles' \
  -H 'Accept: application/json' \
  -H 'Accept-Language: ru-RU,ru;q=0.9,en-US;q=0.8,en;q=0.7' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: text/plain;charset=UTF-8' \
  -H 'Origin: https://cat-cas-lands.pages.dev' \
  -H 'Pragma: no-cache' \
  -H 'Referer: https://cat-cas-lands.pages.dev/de' \
  -H 'Sec-Fetch-Dest: empty' \
  -H 'Sec-Fetch-Mode: cors' \
  -H 'Sec-Fetch-Site: cross-site' \
  -H 'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36' \
  -H 'sec-ch-ua: "Chromium";v="116", "Not)A;Brand";v="24", "Google Chrome";v="116"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-platform: "Windows"' \
  --data-raw $'{"email":"nosov+2023-09-24@nodeart.io","phoneCode":"355","phoneNumber":"80505535424","password":"NodeArt9\u0021","currency":"EUR","registrationPromoCode":"","countryCode":"ukr","agreedWithTermsAndConditions":true,"ageConfirmed":true,"passwordRepeat":"NodeArt9\u0021"}' \
  --compressed
```
![[responce-429.png]]


На иные API методы ограничения также не обнаружены.

## Что хотелось бы:

Запрос на необходимость показа каптчи определялась бекендом для Google Captcha V2. Получив запрос на отображение ReCapthca мы, используя к [примеру](https://github.com/basaran/svelte-recaptcha-v2) ([демо реализации](https://basaran.github.io/svelte-recaptcha-v2/demo)), подгрузим библиотеку Google ReCapthca2 и покажем галочку для прохождения валидации. Пройдя валидации мы приложим токен к следующему запросу (или, скорее всего, повторим тот запрос по которому у нас запросили каптчу сначала).

Для того чтобы это сделать нам необходимо чтобы на тестовом окружении была активирована поддержка Captcha со cтороны бекенда.

Нобходимо документация как работать с запросом на Captcha.

Желательно иметь наглядный пример релизованной механики работы с captcha (грубо говоря сайт где можно посмотреть как это работает в живую)