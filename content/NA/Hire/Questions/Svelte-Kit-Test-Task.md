## Цель: 
знакомство с экосистемой sveltekit и сопутствующим инструментарием. Знакомство с cloudlflare pages и экосистемой workers.

## Задача
Реализовать сокращатель ссылок. Сервис представляет собой форму, содержащую два поля. В поле `url` вводится длинная ссылка, в поле `shortUrl` вводится желаемый короткий урл.
При переходе по ранее созданной `shortUrl` человека редиректит на url введенный при создании урл. При переходе по ссылке вида `/shortUrl/stats` открывается история переходов по ссылке.

### Этапы:
- публичный репозиторий на github
- создать пустой проект [sveltekit](https://kit.svelte.dev/docs/creating-a-project)
- изменить тип адаптера на [cloudflare-pages](https://kit.svelte.dev/docs/adapter-cloudflare)
- подвязать [cloudflare pages к репозиторию в github](https://developers.cloudflare.com/pages/get-started/git-integration/) и активировать ci/cd
- создать ветку с названием task, дальнейшая работа будет вестись в этой ветке. С нее должен быть сделать PR на мастер, в котром будет вестись ревью. ВАЖНО: активированный ci/cd на прошлом этапе позволит собирать превью ветку для просмотра результата работы.
- создать [layout](https://kit.svelte.dev/docs/routing#layout) для оформления страниц шапкой, футером. Чтобы не возится с оформлением рекомендую подключить [picocss](https://picocss.com/docs)
- создать форму ввода для url и shortUrl с кнопкой submit. Форма должна релизовывать:
	- необходимо использовать [form enhancement](https://kit.svelte.dev/docs/form-actions#progressive-enhancement) для создание качественного ux формы.
	- валидацию url, с установкой [invalid](https://picocss.com/docs/forms/input#validation-states) для полей содержащих не корректный ulr
	- при сабмите формы кнопка submin должна переходить disabled state (можно с [loading](https://picocss.com/docs/loading)). Цель: предотвратить дабл клик. Показать человеку, что форма работает
	- в случае успеха — показать ссылку на `/shortUrl/stats` вместо кнопки success
	- в случае ошибки — показать текст ошибки. Пример кейса для проверки ошибки - такой `shortUrl` уже есть.
- подключить [cloudflare kv](https://developers.cloudflare.com/kv/) к проекту
- подключить miniflare для локальной разработки. [Инструкция](https://community.cloudflare.com/t/how-do-you-properly-access-cloudflare-resources-from-within-a-sveltekit-app/518591/9)
- необходимо создать [+server.ts](https://kit.svelte.dev/docs/routing#server) роут для обработки `/[shortUrl]`. При переходе по урл:
	- необходимо отдавать 302 редирект на `url` связанный с этим `shortUrl`
	- сохранять в kv время перехода
	- сохранять в kv useragent браузера, который совершал переход
	- сохранять в kv [geoip данные](https://developers.cloudflare.com/fundamentals/reference/http-request-headers/#cf-ipcountry) которые предоставляет нам cloudflare
	- сохранять в kv [ip адрес](https://developers.cloudflare.com/fundamentals/reference/http-request-headers/#cf-connecting-ip) с которого был осуществлен переход
- необходимо создать роут +svelte.page для `/[shortUrl]/stats`. Внтутри роута:
	- показать табличку с историей открыти ссылки, вместе с сохраненной информацией (время, ip, useragent, гео данные)
	- показать сумму кликов по ссылке - totals
- bonus: сделать роут /list, в котормо можно было бы увидеть список всех созданных ссылок.

### Критерии оценки
- минимальное использование внешних библиотек
- корректное использование TS (использование strict mode)
- минимальное количество кода для реализации задачи
- максимальное использование встроенного в sveltekit функицонала (лейауты, разные типы роутов, form progressive enhancement и иные функции kit)
- способ структурирования информации в kv. KV предоставляем максимально простой механизм, а хранить нам нужно историю переходов под каждый созданный урл. Сделать это можно разными путями.
- приложение не сломается если по одной ссылке кто-то перейдет 2000 раз. 4000 раз. Не сломается экран stats тоже и kv тоже.

### Артефакт
Должен быть PR, который не включает в себя скелет sveltekit и интеграцию адаптера для запуска CI/CD. Внутри PR должен быть рабочий preview branch для просмотра запущенного на pages проекта.
