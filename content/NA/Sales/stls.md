## Goal
Описать возможный вариант трансформации.
## Concept
Общий подход который мы хотим предложить называется Headless Commerce.
![[stls.png]]
Этот подход подразумевает что мы отдельно имеем Ecommerce backbone - бекенд реализующий API для управления информацией и запуска workflows. Отдельно от него имеем админ панель (работающую с ним по апи). И совершенно отдельно разрабатываем то что называется storefronts. Это магазины, которые используют наше апи как поставщика информации.

Скорее всего возникнет вопрос, о том, что SPA приложения не индексируются гуглом. Абсолютно верно, но эти storefronts на самом деле не просто фронтенд, а небольшой прослоечный бекенд для рендера страниц, что делает их индексируемыми. Как это выглядит в жизни?

[![](https://mermaid.ink/img/pako:eNqNUk9vgjAU_yrNO20JGisCwmGJU7mZmG2nwQ4VihChNaVkOvWwj7hvtNLKYpZtroem773fv7Q9QMJTCgFkJX9NciIkeprFDKk13-GP92i-k1QwUqIZkQSFlKZ1gHDygnq9u-OjFJQwPTqie5JsOuZPtD8pk2U0SauCoSVhtOygXNAL5E27rTijuqIsvTXctmoJyIhXqNmmRNL6iKYPi2sQPDWIulmtBdnmqG5dM8GZxGai2GGIo7MnyrhAeqwKnROFGhyFXdPQ1Ombdyvzq9nwwmx4zWz4L7OzJFhQUVGRIlXPfGh7McicVjSGQB1TIjYxxOykcKSR_HHPEgikaKgFgjfrHIKMlLWqzJ3NCqKyV1_dLWEQHGAHAfbcPh6NR2Mb-47r2HhkwR4Cv-86vj3AA2x72Pe8kwVvnCuBQd-zHdf2XRt7ozH2fUerPeuhCUDTQt3QwnxR_VO7GHM9Oac4fQLxSN_l?type=png)](https://mermaid.live/edit#pako:eNqNUk9vgjAU_yrNO20JGisCwmGJU7mZmG2nwQ4VihChNaVkOvWwj7hvtNLKYpZtroem773fv7Q9QMJTCgFkJX9NciIkeprFDKk13-GP92i-k1QwUqIZkQSFlKZ1gHDygnq9u-OjFJQwPTqie5JsOuZPtD8pk2U0SauCoSVhtOygXNAL5E27rTijuqIsvTXctmoJyIhXqNmmRNL6iKYPi2sQPDWIulmtBdnmqG5dM8GZxGai2GGIo7MnyrhAeqwKnROFGhyFXdPQ1Ombdyvzq9nwwmx4zWz4L7OzJFhQUVGRIlXPfGh7McicVjSGQB1TIjYxxOykcKSR_HHPEgikaKgFgjfrHIKMlLWqzJ3NCqKyV1_dLWEQHGAHAfbcPh6NR2Mb-47r2HhkwR4Cv-86vj3AA2x72Pe8kwVvnCuBQd-zHdf2XRt7ozH2fUerPeuhCUDTQt3QwnxR_VO7GHM9Oac4fQLxSN_l)
- External Data Feeds: внешние источники информации, это могут быть xml feeds, это может быть база 1с (что угодно на самом деле), это могут быть данные из PIM системы
- Admin Panel: панель для управления настройками workflows внутри (хотя можно и продукты тут править, но если у нас прямо сотни тысяч продуктов то лучше задуматься о PIM)
- Backbone Backend (BB): собственно говоря бекенд магазинов, на самом деле он устроен существенно сложнее (это набор независимых сервисов, очередей и т.п.). Он предоставляет несколько важных функций: апи для доступа к состоянию (база) и апи для управления workflows (к примеру покупка, доставка)
- CRM/1C: это внешние системы, которые могут получать данные используя свое апи.
- Backend for Frontend (BFF): абстракция на уровне конкретного магазина, которая занимается адаптацией API Backbone Backend (BB) к потребностям фронтенда
- Frontend: веб приложение, nextjs, sveltekit, nuxt - может быть что угодно, вариантов на чем сделать storefront огромное множество, главное чтобы команда быстро могла вносить правки (сео, промо и т.п.)

### Workflows
Важное концептуальное отличие нового поколения ecommerce систем от старых — использование workflows. Вместо написания апи, схем повтора и обработки ошибок все долгие или затрагивающие внешние системы оборачиваются в workflow engine, который уже реализует контроль того, что мы не потеряем данные, переживем сбой и так далее.
Пример такого workflow engine: https://temporal.io/ Его можно подружить с любым бекендом, но некоторые современные фреймворки имеют из коробки упрощенные версии workflow engine внутри, к примеру medusa: https://docs.medusajs.com/learn/fundamentals/workflows

Общая идея состоит в том, что вместо написания большого количества служебного кода (для к примеру обработки статусов доставки) необходимо перейти к использованию workflows engine, который гарантирует нам что ничего не потеряется и не будет забыто.

## обзор доступных решений

### MedusaJS
[https://medusajs.com/](https://medusajs.com/)
Решение на nodejs, предоставляет хороший product vision и продуманную архитектуру. Но при этом под капотом есть вещи которые мне в инженерном плане не нравятся (express, mikro-orm). Основные сложности с этим решением — отсутствие готовых build pipelines, что делает дорогим старт в плане необходимости большого обьема работы devops команды для автоматизации.

### Vendure
[https://www.vendure.io/](https://www.vendure.io/)
Решение на nodejs, предоставляет более приятная для работы подкапотная начинка (nestjs), но при этом существенно проигрывает в product vision (нет workflows к примеру).

### Saleor
https://saleor.io/
Решение для headless ecommerce на python. В целом оно единственно живое на python, и живет в списке квк вариант использования текущей python команды.

## Spree
https://spreecommerce.org/
Это эталонное решение, которое умеет все, и вообще обычно референс для понимания как нужно делать. Но оно написано на Ruby. Из-за чего поддерживать его сложно (в плане найма людей, но проще чем Perl конечно)

## Steps
Я предлагаю ознакомиться с концепцией, если она ок, посмотреть на примеры базовых решений. Далее выбрать одно из решений и делать на нем новую основу для бизнеса. Написать импорты и обмен с другими системами. Параллельно делать свой storefront. Отдельно стоит подумать о возможности интеграции с PIM решением.