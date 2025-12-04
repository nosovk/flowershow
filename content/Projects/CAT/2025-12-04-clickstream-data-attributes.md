# DATA ATTRS PRODUCT


Для логирования взаимодействия с элментом необходимо довесить ему `data-track="имя элмента"`

## 1. Кнопка Google-логина

Добавить `data-track="google_login"` в кнопку

## 2. Кнопка копирования кода бонуса

Добавить `data-track="copy_bonus" data-track-value="%code%` на каждую кнопку со значением кода.

## 3. Форма активации бонуса

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


