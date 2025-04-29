Goal: описать поведение платформы при авторизации на лендингах. Продемонстрировать место возникновения проблемы.

Тестовая партнерская ссылка: https://luminous-smolderpass.com/s118c5ce4

Ниже описана последовательность запросов в браузере, приводящая к ошибке. Для удобства восприятия из ссылок убрана часть get параметров.

1. GET https://luminous-smolderpass.com/s118c5ce4 Status 302 => https://442.alovlanding.com/az/win/alov-long-social?stag=660614_68108b4ea2e0e8822f56081c
2.  GET https://442.alovlanding.com/az/win/alov-long-social?stag=660614_68108b4ea2e0e8822f56081c Status: 200
3. GET https://accounts.google.com/o/oauth2/v2/auth?client_id=645476980438-n8eedl06tpneck63b3b7ub1am8rv8sns.apps.googleusercontent.com&redirect_uri=https%3A%2F%2Fland.move2alov.com%2Fwebsitewebapi%2Fapi%2Fv3%2FGoogleAccount%2FCallback&response_type=code&scope=openid+profile+email&state= Status: 302 => https://land.move2alov.com/websitewebapi/api/v3/GoogleAccount/Callback?state=&code=&scope=email+profile+openid+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.profile&authuser=0&prompt=none
Обратите внимание, происходит редирект на редиректор, как callbackurl после авторизации, это связанно с настройками которые мы передаем в апи в виде загловка `website-origin` со значением домена редиректора.
4.   https://land.move2alov.com/websitewebapi/api/v3/GoogleAccount/Callback?state=&code=&scope=email+profile+openid+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.profile&authuser=0&prompt=none&completion_time=964.3000000119209 Status: 302 => https://alov.casino/websitewebapi/api/v3/GoogleAccount/Callback?state=&code=&scope=email+profile+openid+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.profile&authuser=0&prompt=none
Редиректор отрабатывает корректно, и перебрасывает человека на активное зеркало в соответсвующий урл колбека.
5.   https://alov.casino/websitewebapi/api/v3/GoogleAccount/Callback?state=&code=&scope=email+profile+openid+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.profile&authuser=0&prompt=none Status: 302 => https://land.move2alov.com/az/promotions?modal=payment-cashier?playerToken=860a87abc1674090a41e04e5966305ff
::: Error :::
Тут происходит ошибка!!!!
:::
по какой-то причине вместо авторизации на сайте происходит возврат пользователя на редиректор с авторизационными параметрами.

Ожидаемое поведение:  человека отредиректит с параметрами авторизации внутри зеркала, тем самым авторизовав его на продукте

6. https://land.move2alov.com/az/promotions?modal=payment-cashier?playerToken=860a87abc1674090a Status: 302 => https://alov.casino/az/promotions?modal=payment-cashier%3FplayerToken%3D860a87abc1674090a41e04e5966305ff
повторный редирект экранирует и ломает ссылку для авторизации.


### Проблема:
на данный момент поведение платформы при авторизации опирается на заголовок `website-origin` при восстановлении сессии, что приводит к дополнительным шагам редиректа и некорректной работе авторизации. В этом заголовке содержится корректное значение для редиректа в механизме гугл авторизации, но сам сайт при восстановлении сессии не должен опираться на этот параметр при формировании ссылки для редиректа.