## Goal
Создание партнерского продукта для привлечения аудитории игроков и заработка на аффилиатной программе real money casino + возможно доходы от продвижения слотов определенных вендоров.
## Key Points
Основное отличие от обычных партнерских сайтов в механике турниров на бонусные деньги.
Игрок регистрируется на сайте используя телеграм. При регистрации от него требуется добавить в контакты бота. Если бот не добавлен — человека не пустит к играм пока не добавит бота.
Будучи авторизированным человек получает баланс FUN монет. Игрок может играть на FUN монеты с сохранением баланса между играми. В продукте идут турниры на ФАН монеты. При этом для выигрыша могут выдаваться реальные NDB в казино партнере к примеру или иные призы. По расписанию (раз в день) балансы обновляются до исходного состояния (к примеру 5000fun).
Каждый день бот пишет людям информацию о рефиле монет и о текущих турнирах для ретеншена игроков в продукт.
## Project Risks
- АПИ гейм провайдеров должно поддерживать работу с FUN балансом как с реальными деньгами. То есть мы должны иметь возможность проинициализировать игру с указанным стартовым балансом. Из существующих вендоров нашел пока только одного у которого есть такая возможность (и у него левые игры). Необходимо получить соответствующее апи.
- Для иных гео телеграм может быть не оптимальным способом коммуникации. Вся механика продукта заточена на телеграм, проблемы с ним могут поломать основные механики.

## Flow
### 1. Анализ
Необходимо описание основных механик + нарисованные моки всех экранов (бальзамик или аналог). Этот этам может занять до недели времени. На этом этапе необходимо формализовать механизмы турниров, чтобы был понять список параметров для конфигурирования турниров со стороны администратора.
Так же необходимо проработать требования к URLs и к SEO на данном этапе
Люди: бизнес аналитик, частично техлид
Время: до недели
### 2. Дизайн
На этом этапе необходимо из моков отрисовать макеты, в первую очередь мобильной версии приложения.
Отдельно необходимо реализовать все состояния компонент (hover etc)
Отдельно необходимо прорисовать анимации.
Если планируется использовать [Lottie](https://lottiefiles.com/) - на этом этапе его так же необходимо подготовить.
Люди: бизнес аналитик, дизайнер
Время: от недели до двух (может быть более при наличии сложных анимаций)
#### 3. Рельсы
на этом этапе необходимо настроить базовые механизмы проекта.
- создать CI/CD с поддержкой веток
- проект под клиентскую часть
- интегрировать i18n
- авторизация через тг (lucia не умеет авторизацию в [тг](https://github.com/pilcrowOnPaper/lucia/issues/857), могут возникнуть сложности)
- Интеграция кита с телеграмом (в зависимости от требований может быть либо прямая интеграция, либо [Grammy](https://github.com/grammyjs/grammY), пример есть [тут](https://github.com/begoon/sveltekit-bot/tree/main) для другой библиотеки)
- решение по админке (либо отдельный домен, либо роут внутри кита), авторизация админа и поддержка ролей
Люди: 1 или 2 frontend разработчика, проджет менеджер, devops 2/3 дня
Время: от одной до двух недель
#### 4. Написание бизнес логики
на этом этапе необходимо реализовать все основные сценарии проработанные на этапе 1.
Люди: 1 или 2 frontend разработчика, проджект менеджер.
Время: от месяца
#### 5. Верстка
На этом этапе необходимо привести моки в соответствие дизайну. Этап может разнится в зависимости от дизайна, набора анимаций и т.п.
Люди: 1 верстальщик, проджект менеджер
Время: от месяца
#### 6. Тестирование
Это и процесс и этап, тестирование это процесс\этап который необходимо запустить со сторов половины этапов врестки и написания бизнес логики. Проект не подразумевает написания e2e тестов, но ручное тестирование необходимо
Люди: 1 тестировщик, проджект менеджер
Время: от двух недель

## Таймлайн этапов

| stage / week  | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|-------------- | -- | -- | -- |-- | -- | -- | -- | -- | -- | -- |
| 1. Анализ | x |  |  | | | | | | | |
| 2. Дизайн | | x | x | | | | | | | |
| 3. Рельсы | |  |  x | x | | | | | | |
| 4. Написание бизнес логики | | | | | x | x |x |x |x |x |x |x |  | | |
| 5. Верстка | | | | | | x |x | x| x|x | | | | | |
| 6. Тестирование | | | | | | |  | | x| x| |x

| role/week| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10|
| ---      | - | - | - | - | - | - | - | - | - | - |
| anayst   | x | x  | x |   |   |   |   |   |   |   |
| pm       |   | x | x | x | x | x | x | x | x | x |
| devops   |   |   | x |   |   |   |   |   |   |   |
| frontend |   | x | x | x | x | x | x | x | x | x |
| frontend |   |   |   | x | x | x | x | x | x | x |
| designer |   | x | x |
| html     |   |   |   |   |   | x | x | x | x | x |
| qa       |   |   |   |   |   |   |   |   | x | x |

### Disclaimer
Любые сроки не есть коммитмент, в жизни все может пойти не так. Люди болеют, возникают новые сложности которые не были замечены на этапе оценки и т.п. Чаще всего страдает этап тестирования и приемки, в диаграмме на него выделено две недели, но этого может быть мало и т.п.

## Development Risks
Для работы на проекте необходимо два фронтенд разаботчика чтобы нивелировать риск болезни\призыва сразу обоих. В случае если этот риск приемлем можно обойтись одним разработчиком. Для этапа рельсов разработчик может быть один, для этапа написания бизнес логики уже два.
## Tech
фреймоврк: [Svelte](https://svelte.dev/), мета-фреймворк: [SvelteKit](https://kit.svelte.dev/), авторизация: [Lucia Auth](https://github.com/pilcrowOnPaper/), хостинг: [CloudFlare Pages](https://pages.cloudflare.com/), база данных: оптимально [Neon](https://developers.cloudflare.com/d1/), но если будут проблемы со скоростью то можно [CloudFlare D1](https://developers.cloudflare.com/d1/)