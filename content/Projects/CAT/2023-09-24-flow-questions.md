В документе [[Projects/CAT/Flow]] был указан важный момент касательно последовательности событий.

> - api used for registration could have another domain rather final casino domain (which is obtained at rotator). Because of that, cookie authorization does not work (because cookies stick to issuing domain – API domain). That's why we have to request some additional JWT functionality. If we can use real casino domain as API domain – we can avoid passing JWT and cut of rotator part.

Если мы на лендинге будем использовать апи конечного продукта для регистрации то нам не нужно будет дорабатывать фронтенд для поддержки JWT.
То есть вопрос про реализацию JWT регистрации возник по причине использования Rotator блока после лединга (шаги 9 и 10 на диаграмме). Если лендинг будет сразу отправлять на правильное зеркало пользователя (используя API URL продукта конечного), то существующий механизм авторизации по cookie будет работать корректно.