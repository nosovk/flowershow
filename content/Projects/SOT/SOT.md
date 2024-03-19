## Цель
Создания социального казино. Суть продукта: люди приходят, регистрируются на продукте, играют в слоты, люди могут выигрывать реальные деньги выполняя целевые действия (к примеру играя в турнир). Фактически дальний аналог фаусета, или мотива, но при этом у нас есть выраженное отличие - мы предлагаем людям играть в слоты в той или иной форме, и платим им за это.
Заработок на рекламе, мы рекламируем офферы казино, сливая на них трафик. При этом в отличие от обычного обзорника или каталога мы имеем свой собственный ретеншен, построенный на предложении выиграть бесплатно.

## PreConditions
Провайдеры игр готовы давать нам игры бесплатно. При этом мы можем в демо игру провайдера загрузить начальный баланс пользователя и так же получать обновления баланса. Если у нас нет слотов с которых можно получать результаты игры, то мы не сможем организовать соревнования.
## User Stories
Я как пользователь прихожу на сайт, вижу предложение поучаствовать в турнире, с розыгрышем 100usd. Для участия в розыгрыше мне необходимо зарегистрироваться (при регистрации я отдаю свой email для контактов). После регистрации я вижу список турниров для участия. Нажав участвовать в турнире я получаю баланс. В рамках баланса я могу играть на разрешенных в турнире аппаратах. Проиграв все бонусные деньги, я занимаю призовое место в турнирной таблице и баннер с предложением получить бонус на реальный депозит.
На следующий день мне приходит письмо, и пуш с уведомлением о доступности нового турнира.

### Описание механики турнира
Игроки, участвующие в турнире, представляют группу. Каждый участник получает сумму x на счет. Игроки проигрывают деньги на счету. Для каждого кошелька мы считаем total win по счету в рамках турнира. Мы сортируем пользователей по параметру total win в рамках турнира, чтобы определить места в таблице участников. На момент окончания турнира мы фиксируем места людей, и призовые места получают призы. Деньги, которые остались не проигранными в рамках турнира — сгорают.

## Функциональные блоки
### Регистрация (client)
Я могу зарегистрироваться любым способом, который оставит информацию о моем емейле. К примеру:
- регистрация по е-мейл (confirm email, reset password)
- регистрация через oauth (gmail, facebook)
Неясна судьба регистрации через телеграм — телеграм не содержит е-мейла, вероятно для регистрации через телеграм необходимо реализовывать какую-то дополнительную логику (к примеру подписка на канал в телеге).

### Antifraud (client/admin)
Мы хотим помечать меткой suspicious пользователей с неуникальным fingerprint либо иными признаками подозрительности
### Retention mails (client)
Мы хотим интегрировать customer.io, либо аналогичный сервис для построения retention flow
### Retention push (client)
Целесообразность пушей вызывает вопросы, но может все еще, имеет смысл сделать.
### Tournament (admin)
Модуль позволят создавать турниры.
Свойства турнира:
- время начала: время старта турнира
- время конца: время окончания турнира
- время начала показа: время, с которого турнир доступен на экране списка турниров
- auto-assignment: имя группы, которая автоматически будет добавлена в участники турнира при старте турнира, может быть пустым
- self-assignment: разрешить людям самостоятельно добавляться в турнир
- gameList: список игр, которые участвуют в турнире
- prizes: список призов, в порядке убывания места
- repeatable: автоматический повтор турнира (пока не ясно как это должно работать)

### Tournament (client)
### Profile (client)
Я как клиент, хочу видеть историю турниров, в которых я участвовал и какие места я занимал в турнире.
### Profile (admin)
Я как админ, хочу видеть список сессий игрока на сайте, паузы между сессиями, на какие рекламные предложения реагировал пользователь, какие рекламные предложения были ему показаны (часть от DSP).
Важно: для управления игроками необходим механизм тегов. К примеру тег: depositor, more one year LT, somecasinoplayer. 

### DSP
Нужно поискать какое-то готовое DSP решение, т.к. разработка своего дело очень затратное. Суть DSP - если мы знаем что по нашей партнерской ссылке игрок зарегистировался на продукте x -  мы более не должны ему показывать оффер этого продукта. Минимальный DSP состоит из списка оферов, трекера кликов, настройки постбеков для учета регистраций и fd.
## Traffic Sources
Если мы привлекам трафик из сеошки, то нам выгодно сохранять публичные страницы прошедших турниров, сохранять их в sitemap и максимально наращивать количество страниц на сайте.

## Technical decisions
### Основные требования:
Полностью кастомный дизайн. Сайт из ниши развлечений, необходимо максимально кастомные элементы с большим количеством красивых анимаций. Фактически мы должны показывать много анимированных элементов, много мелких деталей создают ощущение глубины проработки.
Максимально быстрая скорость загрузки — мы должны максимально быстро рендерить скелет приложения, после чего уже можно догружать недостающие блоки, информацию, анимацию.

### frontend (client)
Для фронтенда предлагается использовать [svelte](https://svelte.dev/) Он максимально заточен под разработку кастомных дизайнов. Для динамической догрузки данных мы хотим использовать функционал [streaming promises](https://geoffrich.net/posts/conditionally-stream-data/), публикуя приложение на [CloudFlare Pages](https://pages.cloudflare.com/) (у них есть нативная интеграция с SvelteKit для работы с догрузкой промисов). [SvelteKit](https://kit.svelte.dev/) предоставляет нам сразу и BFF (backend for frontend), легковесный бекенд для обеспечения [кеширования](https://developers.cloudflare.com/kv/) данных API и быстрых ответ клиентам. Сайт должен поддерживать многоязычность, для чего можно использовать [paraglideJS](https://inlang.com/m/gerre34r/library-inlang-paraglideJs). Для поддержки авторизации удобно использовать [lucia](https://lucia-auth.com/).
### Backoffice (admin)
Для админ панели и управления бизнес сущностями мы предлагаем отдельно поднять дополнительное приложение, админ панель. За базу такого приложения можно взять OSS CMS [Strapi](https://strapi.io/). Ее можно использовать для хранения информации о турнирах, игроках и т.п.
Для хранения информации об истории ставок, strapi не подходит, поэтому понадобится отдельно сделать модели данных и API для интеграции игр и расчета статистики по турнирам. Можно использовать тот же сервер strapi для разработки бекенда, но эти сущности не будет иметь репрезентации в виде сущностей страпи.


# Команда

| role/weeks  | 1-2 | 3-4 | 5-6 | 7-8 | 9-10 | 11-12 |
| ----------- | --- | --- | --- | --- | ---- | ----- |
| fullstack 1 | x   | x   | x   | x   | x    | x     |
| fullstack 2 |     |     | x   | x   | x    | x     |
| backend     | x   | x   | x   |     |      |       |
| devops      | x   |     |     |     |      | x     |
| fe          |     |     | x   | x   | x    | x     |
| qa          |     |     |     | x   | x    | x     |
| pm          | x   | x   | x   | x   | x    | x     |

### Спринты
#### 1-2
Создание рельс приложения, настройка i18n, настройкакешированияя запросов к api, настройка typegen для api. 
Создание флоу регистации, сброса пароля. Создание механизма гардов для ограничения доступа.
Настройка CI/CD для стейджинга и продакшена. Поднятие двух Strapi (stage/prod), реализация механизма бекапов.
#### 3-4
Бекенд - интеграция с апи гейм вендора, интерфейс кошелька (балансы).
Фронтенд - создание мапинга роутинга на сущьности страпи

#### 5-6

#### 7-8

#### 9-10

#### 11-12