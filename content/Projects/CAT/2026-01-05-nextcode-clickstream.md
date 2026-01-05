Goal: получить более надежные данные веб аналитики, нежели данные из GTM/GA

# Реализация
в каждом проекте добавлен скрипт '/event-stream/script/weba.min.js', к примеру
- https://wagibet.asia/event-stream/script/weba.min.js
- https://motorcasino563.com/event-stream/script/weba.min.js
- https://atom-casino.com/event-stream/script/weba.min.js


скрипт размещен всегда по относительному пути внутри продукта. Скрипт есть на всех зеркалах всех продуктов.

Подключение скрипта осуществляется в html
```html
<script async src="/event-stream/script/weba.min.js" type="module"></script>
```

## применение скрипта

### 3 типа события размечаются дата-аттрибутами.

1. Click `data-track-event-click`
2. Form submit `data-track-event-form-submit`
3. Text input `data-track-event-text-input` (работает по событию `focusout` в текстовом поле)

Пример:

```html
<button data-track-event-click="event_name"></button>
```

Значение аттрибута передаётся на бекенд в виде названия события `click:event_name`.
Названия префиксируются по типам `click`|`input`|`submit`.

### Передача данных в событии.

`data-track-data-value` — абстрактное значение.
`data-track-data-currency` — валюта.
Любые другие данные в атрибутах с префиксом `data-track-data-`.

Пример

```html
<button
	data-track-event-click="event_name"
	data-track-data-value="10"
	data-track-data-currency="USD"
	data-track-data-bonus="ABC"
></button>
```

На бекенд уйдёт

```js
{
	type: "click:event_name",
	data: {
		value: "10",
		currency: "USD",
		bonus: "ABC"
	},
	metadata: {/**/}
}
```

### Логирование

Если выполнить `localStorage.setItem('tracker_debug', 'true')`, то в консоли браузер появится лог отправки событий на бекенд.
