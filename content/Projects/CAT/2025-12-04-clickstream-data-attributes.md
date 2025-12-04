# DATA ATTRS PRODUCT

## 1. Кнопка Google-логина

Добавить `data-track="google_login"` в кнопку

## 2. Смена валюты

Добавить `data-track="currency_listbox"` на родительский элемент контрола, в идеале на элемент с `role="listbox"`
На сайте я видел несколько вариантов компонентом, нужно на все.

## 3. Смена метода оплаты в форме депозита

Добавить `data-track="payment_method" data-track-value="%method%"` на каждый айтем.
`data-track-value` — значение метода, можно скопировать то же, что в `data-method-id` или сократить.

## 4. Кнопки пресета депозита (20, 30, 50, 100, 200)

Добавить `data-track="deposit_preset" data-track-value="%value%"` на каждую кнопку
со значением сумма+валюта, пример `50 USD`.

## 5. Текстовое поле ввода суммы депозита

Добавить `data-track-input="deposit_amount_input"` на `input`.

## 6. Кнопка копирования кода бонуса

Добавить `data-track="copy_bonus" data-track-value="%code%` на каждую кнопку со значением кода.

## 7. Кнопка переключения крипто-протокола (USDT)

Добавить `data-track="crypto_protocol"` на кнопку.

## 8. Кнопка копирования крипто-адреса

Добавить `data-track="copy_crypto_address"` на кнопку, или на 1 из родителей кнопки.

## 9. Форма активации бонуса

Добавить `data-track-form="deposit_bonus_activate"` на элемент `form` в режиме активации (когда текст на кнопке "Activate").
Добавить `data-track-form="deposit_bonus_remove"` на элемент `form` в режиме удаления (когда текст на кнопке "Remove").
Добавить `data-track-input="deposit_bonus_input"` на текстовый инпут.

# DATA ATTRS LANDINGS

## 1. Form submit in form

Добавить `data-track-form="landing_registration"` на `form`

## 2. Form submit wo form

Добавить `data-track="landing_registration_button"` на кнопку сабмита.
Внимание: только если нет формы! Для варианта с формой не нужно!

## Tip

Если выполнить `localStorage.setItem('tracker_debug', 'true')`, то в консоли браузер появится лог отправки событий.


