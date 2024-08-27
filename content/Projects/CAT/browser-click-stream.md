Click Stream from browser events
Goal: сохранить историю поведения человека в рамках сессии взаимодействия с сайтом.
## Схема взаимодействия
[![](https://mermaid.ink/img/pako:eNplUTtzwjAM_is6rQ29EALJeWDo0aXXTmy9LCYW4IPYqR9QyvHfKyfQpZpkfQ_J0hVbqwgFevqKZFpaablzsmuMjMGa2G3INQY4Ns6ePTmYLJdPXgcS8G6lgpSOhJQl9MEUsNMngrc1aBPItdQH68C3TvfhnyXTBHTyQB4ktEfdHkYKqxPKrFNy9GTUCLPp1oLlKuxD6IGtz_4-yEAedKsX1nBb-hOBkkFupCfMsCPXSa3499ekbDDsqaMGBaeKtjIeQ4ONuTE1bWN9MS2K4CJlGHv2eSwLxVYePVd7aVBc8RvFJH_OOYqqnOV1WS6KeV5XeVlleEFRTBmdz-qyropZvpgWi1uGP9ay0fRh_ao0z_3nTMPzY7zVcLKh2-cgGkdyNu72d8HtFwJFmHs?type=png)](https://mermaid.live/edit#pako:eNplUTtzwjAM_is6rQ29EALJeWDo0aXXTmy9LCYW4IPYqR9QyvHfKyfQpZpkfQ_J0hVbqwgFevqKZFpaablzsmuMjMGa2G3INQY4Ns6ePTmYLJdPXgcS8G6lgpSOhJQl9MEUsNMngrc1aBPItdQH68C3TvfhnyXTBHTyQB4ktEfdHkYKqxPKrFNy9GTUCLPp1oLlKuxD6IGtz_4-yEAedKsX1nBb-hOBkkFupCfMsCPXSa3499ekbDDsqaMGBaeKtjIeQ4ONuTE1bWN9MS2K4CJlGHv2eSwLxVYePVd7aVBc8RvFJH_OOYqqnOV1WS6KeV5XeVlleEFRTBmdz-qyropZvpgWi1uGP9ay0fRh_ao0z_3nTMPzY7zVcLKh2-cgGkdyNu72d8HtFwJFmHs)
1. После загрузки сайта
2. Загружается подготовленный JS скрипт (он должен быть частью сайта, воз избежание блокировки adblock).
   > Важно: скрипт устанавливает глобальный обработчик кликов в бразуере
3. Когда пользователь делает клик, JS разбирает информацию о том, на каком элементе был сделан клик и ищет специальные data-attributes для формирования сообщения о логе
4. Собрав пакет, он снабжается дополнительными элементами вроде userid, url etc. и отправляется на сервер (мы использовали [EMQX](https://github.com/emqx/emqx) + [u8-mqtt](https://www.npmjs.com/package/u8-mqtt), но это может быть и просто http ендпоинт в который бы батчем слались события)
5. Сервер обогащает данные (к примеру по userid берет данные профиля на момент записи) и складывает в плоскую таблицу в DB (у нас это обычно BigQuery, но ClickHouse тоже решает эту задачу)

## JS интеграция
Важные для нас поля, которые должны быть в каждом пакете:
type: "click"|"load"|"scroll", etc.
domain: "https://catacsino.com"
path: "/en/deposit", текущая страница
urlParams: "?showModal", текущий query string
payload: {constainer: "deposit-block", "selector": "button"}, это JSON со схемой данных привязанной к типу события. К примеру для клика это селектор, для загрузки параметры скорости загрузки и т.п.

### Определение селектора
Большой проблемой сохранения селекторов в лоб является отсутствие позже возможности понять что именно было нажато, т.к. сайт регулярно обновляется. Для решения этой проблемы предлагается использовать дополнительные дата атрибуты.
```html
<div data-container="deposit-block">
<button data-click="deposit-button-click" data-click-value={depositSumm}>
</div>
```
Пример выше малость надуманный, но сам принцип должен стать понятным.
Мы размечаем сайт специальными тегами 'data-container' и при получении клика отдельно складываем ближайший родительский контейнер и селектор относительно него. Такой подход достаточно удобен, т.к. при изменении верстки сайта контейнеры чаще всего неизменны. Кроме того аналитику лего в браузере найти контейнер по имени и понять что в нем лежит.

Однако часто нам нужно дополнительный контекст знать. В этом случае мы можем использовать дополнительные дата атрибуты. К примеру в примере у нас есть "data-click", этот дата атрибут позволяет при клике на этот элемент приложить в посылку в аналитику дополнительно значение аттрибута data-click-value, к примеру с суммой депозита.

Подход с кастомными дата селекторами был удобен в angular и svelte, т.к. в обоих фреймворках есть нативные инструменты для работы с ними, не факт что такой подход будет удобен в реакте.

