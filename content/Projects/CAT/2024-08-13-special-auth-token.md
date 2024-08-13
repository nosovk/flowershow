## Goal
Описать основные сценарии, которые мы хотели бы реализовать. Предложить варианты решения.

[TOC]
## User stories
### 1. Returning Visitor Story
Я как пользователь могу перейти по нескольким рекламным оферам одного продукта. К примеру: 
- catland.com/land1 - лендинг
- mirrorcatland.com/land2 - другой лендинг
- cat2go.com/ - редиректор, который определяет пользователя на ближайшее зеркало
- cat.com/ - сам сайт продукта
- catmirror.com/ - зеркало продукта

Я как пользователь прихожу на лендинг catland.com/land1, и совершаю регистраци. После успешной регистрации меня редиректит на редиректор cat2go.com/. 
Редиректор смотрит на меня и принимает решение средиретить меня на сайт продукта cat.com/.
Далее, я как пользователь, перехожу по другой рекламе, и попадаю на mirrorcatland.com/land2.

Поведение сейчас: я могу совершить новую регистрацию.

Желаемое поведение: попав на новый лендинг, увидев что я уже был зарегистрирован на продукте ранее, я ожидаю что я буду автоматически залогинен, после чего отправлен на редиректор, после чего отправлен на активное зеркало в залогиненом состоянии.

### 2. Personal promotion Story
В некоторых случаях на один и тот же промо офер возвращаются игроки на протяжении длительного промежутка времени. 

- Для пользователей имеющих статус VIP я хочу отправлять их на отдельное вип зеркало.
- Для пользователей имеющих более 4х депозитов я хочу показывать вместо регистрационной формы специальный баннер, рекламирующий турнир.
- Для пользователей состоящих в определенной групе я хочу показать специальный бонус.

Это примеры, в целом количество сценариев существенно больше.
## Implementation proposal
### Special landing token
Сейчас мы используем refresh token, получаемый при вызове метода регистрации пользователя. Это создает потенциальные проблемы с безопасностью. Как альтернативное решение мы предлагаем добавить новый, специализированный токен с ограниченными правами и специализрованным сценарием использования.

### Добавить новый токен
В метод `POST /profiles`, отвечающий за регистрациею и в метод `PUT /auth`, отвечающий за логин добавить дополнительное cookies с токеном. 
```javascript
headers.append("Set-Cookie", `__Host-landingauth=${token}; Path=/; HttpOnly; Max-Age=2629800; sameSite=None; secure=true`);
```
> Важно: нам необходим `sameSite=None`, чтобы наш cookies был доступен для всех наших лендингов (по другому называется cross domain cookie).

> Важно: `HttpOnly` обязателен, без него пользователи смогут украсть cookies используя javascript.

> Важно: приставка `__Host-` является частью спецификации, [cookie prefixes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#cookie_prefixes) и должна быть сохранена.

Таким образом, после регистрации или логина у пользователя будет сохраняться cookies, со специальным токеном. При этом, проверить наличие этого cookie в бразуере средствами JS невозможно. 

Почему это безопасно? Установка Cookies не отработает, если мы не разрешим наш домен в списке разрешенных в CORS, поэтому хоть мы и указываем что наш cookies кросс доменный, список разрешенных доменов мы ведем на уровне CORS policy отдельно. То есть сделать запрос на наше апи с любого сайта партнера для получения токена авторизации не выйдет.

### Добавить метод /restore
Мы бы хотели добавить в апи новый метод, назовем его к примеру `/restore`

Этот метод мы будем вызывать, при загрузке лендинга. Если у пользователя нет cookies, то он вернет нам ошибку или пустой объект.

Если у пользователя есть cookie `__Host-landingauth`, то он будет использован для авторизации пользователя.

> Важно: чтобы cookies был передан методу, кроме обычных CORS заголовков так же должен быть возвращен специальный:
> 
```javascript
headers.append("Access-Control-Allow-Credentials", "true");
```
> Без него cookies просто не будет отправлен на сервер для валидации.

Мы хотим, чтобы метод `/restore` возвращал информацию из метода `/userInfo` + новый refresh и jwt токен для пользователя с этим cookie.

Пример ответа: 
```json
{
	"jwtToken": "3cxdfcxsxsxfc",
	"refreshToken": "dxsxdsxsdx",
	"userInfo": {
		"balance": 0,
	    "loyalty": {
	        "IDUser": "3045047524",
	        "Balance": "0.0000",
	        "Login": "4786_1264745",
	        "Country": "deu",
	        "Language": "en",
	        "Currency": "EUR",
	        "Level": "1",
	        "DuplicateLevel": "70",
	        "Points": "0.0000",
	        "NextLevelPoints": "300",
	        "TotalPoints": "0.0000",
	        "TotalBets": "0.00",
	        "CheckDate": "0000-00-00",
	        "ForbidBonuses": "0",
	        "ForbidLoyaltyPoints": "0",
	        "ForbidTournaments": "0",
	        "DepositsCount": "0",
	        "BonusRestrictions": {
	            "RestrictCasinoBonuses": {
	                "State": 0,
	                "IDUser": "3045047524",
	                "Time": "2024-08-13 01:08:58"
	            },
	            "RestrictSportBonuses": {
	                "State": 0,
	                "IDUser": "3045047524",
	                "Time": "2024-08-13 01:08:58"
	            },
	            "RestrictPokerBonuses": ""
	        },
	        "Verificated": "0",
	        "AutoCategoryID": null,
	        "IDCategory": "0",
	        "Wallets": "",
	        "IDShard": "0",
	        "LoyaltyID": "34880000",
	        "Block": 0,
	        "ConfirmPoints": "0",
	        "LevelName": {
	            "en": "Iron",
	            "de": "Iron",
	            "fr": "Iron",
	            "no": "Iron",
	            "fi": "Iron",
	            "ru": "Iron",
	            "uz": "Iron",
	            "kk": "Iron"
	        },
	        "LevelCoef": "1.000",
	        "LevelID": "1",
	        "ActiveBonuses": [],
	        "LevelUp": false,
	        "BonusesBalance": {}
	    },
	    "email": "dsxdsx@fc.om.com",
	    "firstSession": true,
	    "firstName": "",
	    "lastName": "",
	    "status": 1,
	    "idUser": "3045047524",
	    "availableWithdraw": 0,
	    "currency": "EUR",
	    "emailHash": "f4120c7ba1524ce6940b261ed442a1b0",
	    "category": "",
	    "freerounds": [],
	    "affiliateID": "",
	    "LockExpiresAt": "",
	    "nick": "",
	    "NotifyKYCQuestionnaire": false,
	    "pincode": "57761974",
	    "userSessionLimit": false,
	    "Tags": {
	        "49924": "paycos_global_ftd"
	    },
	    "validationLevel": "not-secure",
	    "displayConsolidatedBalanceToStreamer": false,
	    "streamWheelOwner": 0,
	    "streamWheelsParticipant": [],
	    "transfersAllowed": false,
	    "toSVersion": {
	        "AcceptDateTime": "2024-08-13 01:08:10",
	        "ToSVersion": ""
	    },
	    "toSWlcVersion": "",
	    "wallets": {
	        "EUR": {
	            "walletId": 3045047524,
	            "balance": "0.00",
	            "availableWithdraw": "0.00",
	            "currency": "EUR",
	            "freerounds": []
	        }
	    },
	    "notify2FAGoogle": false,
	    "enabled2FAGoogle": false,
	    "enabled2FATrustDevice": true,
	    "allowAccessToAddStreamWheel": false,
	    "isEmailValid": false,
	    "blockByLocation": false
	}
}
```

В этом случае мы могли бы принять решение о показе некоего дополнительного промо на основании информации о пользователе, а так же создать ссылки ведущие на продукт сразу с токенами для авто логина.


## Conclusion
Предложенный механизм минимизирует риски безопасности и пытается свести к минимуму необходимости в доработке платформы. Задчаи для платформы:
- добавить новую сущность, landingToken
- добавить генерацию и возвращение landingToken в методы login/signUp
- добавить один новый метод /refresh, позволяющий получить информацию о пользователе по landingToken, а так же актуальный refresh и JWT токен.
- 