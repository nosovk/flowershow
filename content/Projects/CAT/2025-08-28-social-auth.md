# Goal: описать проблему с использованием промокодов при соц регистрации

## Flow соц регистрации

1. Клиент делает запрос на апи метода
GET `https://kometa.casino/api/v1/socialNetworks?lang=en`
```json
{"code": 200, "status": "success", "data": [{
"id": "vk","name": "Vkontakte"},{
"id": "gp","name": "Google+"}]}
```
И получаю список доступных методов авторизации

2. После этого делают запрос на получение ссылки для авторизации

PUT `https://kometa.casino/api/v1/auth/socialLink?lang=en`
```
{"provider":"gp"}
```

В ответ получаю редирект:
GET `https://accounts.google.com/o/oauth2/auth?redirect_uri=https%3A%2F%2Farkada.myshadowcasino.com%2Fapi%2Fv1%2Fauth%2Fsocial%2Foauth_cb%2Fgp&client_id=363438536936-2nlu376j4knbco961v3h4jdvuic18d0r.apps.googleusercontent.com&scope=https://www.googleapis.com/auth/userinfo.email&response_type=code&state=eyJsYW5nIjoiZW4ifQ==`

Важно: редирект содержит в себе
redirect_uri: `https://arkada.myshadowcasino.com/api/v1/auth/social/oauth_cb/gp`

> этот домен не связан с доменом, с которого идет регистрация. Фактически это редирект на редиректор. Но проблема сейчас в том, что он заблокирован в ру, то есть людей с зеркала бросает на заблокированный домен.

4. По этому урл человек должен залогиниться в гугле, и человека перебрасывает на

GET 302 редирект `https://arkada.myshadowcasino.com/api/v1/auth/social/oauth_cb/gp?state=eyJsYW5nIjoiZW4ifQ%3D%3D&code=4%2F0AVMBsJiiVYHIsi8e3zJ2jkZUSKLPmuBEeI0V3wYsti4Q_xseQM1t909VQzPoI7Fy-UZK9Q&scope=email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+openid&authuser=0&prompt=consent`

5. этот урл редиректит далее на редиректор уже:

GET 302 `https://land.move2arkada.com/api/v1/auth/social/oauth_cb/gp?code=4%2F0AVMBsJiiVYHIsi8e3zJ2jkZUSKLPmuBEeI0V3wYsti4Q_xseQM1t909VQzPoI7Fy-UZK9Q&scope=email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+openid&authuser=0&prompt=consent&stag=eyJsYW5nIjoiZW4ifQ%3D%3D`

6. редиректор уже отправляет человека на корректное зеркало
GET 302   
`https://arkadacasino560.com/api/v1/auth/social/oauth_cb/gp?authuser=0&code=4%2F0AVMBsJiiVYHIsi8e3zJ2jkZUSKLPmuBEeI0V3wYsti4Q_xseQM1t909VQzPoI7Fy-UZK9Q&prompt=consent&scope=email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+openid&stag=eyJsYW5nIjoiZW4ifQ%3D%3D`

7. Где происходит редирект вида:
GET 302
`https://arkadacasino560.com/en/user-social-register?message=FINALIZE_SOCIAL_REGISTRATION?message=SOCIAL_LOGIN_SUCCESSFUL`


8. После этого происходит.
PUT `https://arkadacasino560.com/api/v1/auth/social?lang=en`

```json
{"social":"gp","firstName":null,"lastName":null,"social_uid":"107522447538859521369","photo":"https://lh3.googleusercontent.com/a-/ALV-UjXp2iHbMlSMFNrMkdx_145_KJTpUzs-ur2Vliy8PbglReWGNQ=s96-c","email":"tqa7971@gmail.com","i_agree":1,"registrationBonus":null,"currency":"RUB","countryCode":"arm","ageConfirmed":true,"agreedWithTermsAndConditions":true}
```

### input от SG

Ранее мы вам описывали PUT `/api/v1/auth/social`, а вы смотрите в PUT `/api/v1/auth/socialLink`.
PUT socialLink - это инит привязки
PUT social - это завершение соцреги, в которой отправляются основные поля профиля, обязательны: social, social_uid, currency, agreedWithTermsAndConditions, один из емейл\логин\телефон(или все вместе), пароль, остальное - опционально

Полный список полей 
```json
{
"currency": "string",
"email": "string",
"firstName": "string",
"lastName": "string",
"password": "string",
"passwordRepeat": "string",
"registrationPromoCode": "string",
"registrationBonus": "string",
"social": "vk",
"social_uid": "string",
"ageConfirmed": true,
"agreedWithTermsAndConditions": true,
"i_agree": 0
}
```
обязательные `required={"currency", "email", "firstName", "lastName", "password", "social", "social_uid"}`


### Почему невозможно реализовать выше указанный метод:
Выше прописан флоу вызовов в браузере, из которых вы можете видеть что метод `/api/v1/auth/social` вызывается уже после флоу авторизации пользователя (не на лендингах, а после редиректора, и после `/api/v1/auth/social/oauth_cb/gp`, которое устанавливает cookies для вызова `/api/v1/auth/social`). Поэтому мы не можем воспользоваться указанным методом для передачи вам данных о бонусе.

Но мы можем воспользоваться методом `/api/v1/auth/socialLink?lang=en`, для передачи в него дополнительных параметров (к примеру stag, промокод и другие поля заполненные пользователем). Тогда вы сможемте в url для редиректа добавить их в параметр state, и таким образом пробросить себе на сайт эти данные (минуя редиректор)