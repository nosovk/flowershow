## Задача
[Схема проекта](https://drive.google.com/file/d/1JWB6UI-SjNlcygzgDUjkbiOLOpOOeJF8/view)
Необходимо построить платформу для сбора данных с клиентских устройств типа счетчики электричества

## User stories
Я как компания хочу вести онлайн учет показаний своих счетчиков. Я регистрируюсь на сайте. Я добавляю ответственных сотрудников. Я вношу данные счетчика (модель, серийный номер). Я получаю данные для подключения счетчика к системе. Я оплачиваю счет согласно количеству счетчиков. Я могу посмотреть статистику показаний счетчиков за оговоренное время. Я могу подать отчетность в регулирующие органы согласно данным со счетчиков. Каждый месяц у меня списываются с карты деньги согласно подключенному тарифу.

## Компоненты системы

[![](https://mermaid.ink/img/pako:eNqNVG2PmkAQ_iub_axGARVI06TKeW3SS2zuEpuCH1YYZCOw3rL4UuN_78D6wmlNuwnLzjwzs8-zA3ugoYiAujROxTZMmFTkzQtygqMoF0vJ1gnZwoL4OM21vxrjiT9ORRlNUiZhTtrtz2QqxW7vv0kGMV9p60NCHTSDxZTlkN4BX6KM5zdQXUOj028-PuQV5Abk_D6k5shKlZBYyEznoOVXE3lmCrasyeZMg7Q7JEwgXJFULHlOOqe8a-SV179jT1I8ViQLwWTke8_fy93VbhC4-P6DAeomn1BioYQEEjHFCOQRgex9RwpQiufLot535HsjMhWFWmJPGgWqM8ACOkb7sUCQ37SZC0V8nBqpIylWIH39Ik8vP37qXnsz30MeM2x-IsoCyDjl4eprtWyKnFWdeS9B7jXtWIoMKXw8pUZ8hZC1FBsege7mWV-dcqLRULaEXPUa2A1kPIbMO-hvRxLBhodQED9MOSad7ZNG7Wz32pqCZnMDGY8h8wHUSDG0H2luC1Re-ceTC13aohnIjPEI_99D5Q6oSiCDgLq4jCBmZaoCGuRHDMXzFK_7PKSukiW0aLnGnoDHGUrNqBuztEDvmuXUPdAddXvDTtfpG8NuzzJtx-rbdovuqWtbnUF3ODQsx3EsczC0ji36Wwis0O3YtmF1zX7f6JtGzxmc93iKOH66ly2gNl_0tVPfPvW2v-oiFbfjH8K8XII?type=png)](https://mermaid.live/edit#pako:eNqNVG2PmkAQ_iub_axGARVI06TKeW3SS2zuEpuCH1YYZCOw3rL4UuN_78D6wmlNuwnLzjwzs8-zA3ugoYiAujROxTZMmFTkzQtygqMoF0vJ1gnZwoL4OM21vxrjiT9ORRlNUiZhTtrtz2QqxW7vv0kGMV9p60NCHTSDxZTlkN4BX6KM5zdQXUOj028-PuQV5Abk_D6k5shKlZBYyEznoOVXE3lmCrasyeZMg7Q7JEwgXJFULHlOOqe8a-SV179jT1I8ViQLwWTke8_fy93VbhC4-P6DAeomn1BioYQEEjHFCOQRgex9RwpQiufLot535HsjMhWFWmJPGgWqM8ACOkb7sUCQ37SZC0V8nBqpIylWIH39Ik8vP37qXnsz30MeM2x-IsoCyDjl4eprtWyKnFWdeS9B7jXtWIoMKXw8pUZ8hZC1FBsege7mWV-dcqLRULaEXPUa2A1kPIbMO-hvRxLBhodQED9MOSad7ZNG7Wz32pqCZnMDGY8h8wHUSDG0H2luC1Re-ceTC13aohnIjPEI_99D5Q6oSiCDgLq4jCBmZaoCGuRHDMXzFK_7PKSukiW0aLnGnoDHGUrNqBuztEDvmuXUPdAddXvDTtfpG8NuzzJtx-rbdovuqWtbnUF3ODQsx3EsczC0ji36Wwis0O3YtmF1zX7f6JtGzxmc93iKOH66ly2gNl_0tVPfPvW2v-oiFbfjH8K8XII)

Важно: схема имеет некоторые упрощения, к примеру Traefik проксирует и доступ к Broker/DB или к примеру API Server должен проверять токены используя Auth Gateway. На схеме подобные связи не отражены, чтобы она была читаемой.

### CloudFlare
url: https://cloudflare.com

Функции:
- CloudFlare Pages: для хостинга веб панели [Price](https://pages.cloudflare.com/)
- CloudFlare CDN: для ускорения загрузки
- CloudFlare PubSub: на данный момент нет, но возможно когда-то заменит брокер. [Документация](https://developers.cloudflare.com/pub-sub/)

Для реализации необходима работа devops
### Proxy Server
url: https://traefik.io/traefik/

Функции:
- ssl termination
- интеграция с CloudFlare для acme v2: генерация wildcart
- rate limits/access lists: контроль доступа
- service discovery для swarm: умеет создавать endpoints по labels сервисов, грубо говоря DI на уровне контейнеров

Для реализации необходима работа devops
### Auth Gateway
url: https://github.com/ory/
либо
url: https://github.com/keycloak/keycloak

Функции:
- веб форма авторизации: логин и регистрация пользователей [kratos](https://github.com/ory/kratos) 
- oauth provider: [hydra](https://github.com/ory/hydra)
- важно: до сих пор нет веб интерфейса для администрирования (управление группами), вероятно нужно писать свой, [пример](https://github.com/dfoxg/kratos-admin-ui/). Из-за этого возможно стоит посмотреть на keycloak из-за потенциальной экономии ресурсов - у него есть готовая админка

[Oathkeeper](https://github.com/ory/oathkeeper) - в хлам, при использовании JWT можно спокойно без него жить

[keto](https://github.com/ory/keto) - в хлам, предлагается обойтись простым RBAC

Keycloak хуже по ТТХ, но есть готовый контейнер который мы используем для mattermost.

Этот блок необходимо создать и настроить на стороне DevOps, после чего необходимо будет интегрировать в [#WebPanel] и [#AdminPanel] и [#API Server]

Для реализации необходима работа devops, nodejs, fullstack

### WebPanel
url: https://app.sviteco.com

Функции:
- управление подписками
- управление платежами
- управление пользователями в организации

Этот блок мы предлагаем разработать используя SvelteKit + CloudFlare Pages.

Для разработки необходим frontend или fullstack

### AdminPanel
url: https://app.sviteco.com/admin

Функции:
- управление пользователями
- коррекция биллинга и доступов
- impersonate
- коррекция настроек устройств

Этот блок мы предлагаем разработать используя SvelteKit + CloudFlare Pages.

Для разработки необходим frontend или fullstack
### Broker
url: https://www.emqx.io/
Функции:
- прием сообщений по MQTT
- ACL для упарвления доступом к топикам
- [коннектор для ClickHouse](https://www.emqx.io/docs/en/v5.2/data-integration/data-bridge-clickhouse.html): часть enterprise подписки, предлагаю купить, чтобы не писать самим

### DataBase
url: https://www.postgresql.org/
Функции:
- OLTP база данных: хранение настроек, данных для интеграций с внешними сервисами и т.п.
- connection pooler: [odyssey](https://github.com/yandex/odyssey)
- backups: [wal-g](https://github.com/wal-g/wal-g) бекапирование WAL в облако с возможностью восстановления на заданную точку

PostgeSQL является базой для всех операционных данных — пользователи, авторизации, платежи, настройки и т.п. Для JS есть [kisley.js](https://kysely.dev/), который позволяет весьма удобно работать с теми же миграциями. 

### DataWarehouse
url: https://github.com/ClickHouse/ClickHouse
Функции:
- OLAP база данных: хранение данных устройств, агрегация данных
- кластеризация и шардирование: поддержка кластерного режима работы для масштабирования + гибки настройки шардирования (возможность положить исторические данные в медленный сторадж)
Есть вопрос кастательно возможности DGLUX забирать из него данные.

### Dashboard
url: https://wiki.dglogik.com/dglux5_wiki:home
Функции:
- интерфейс пользователя для визуализации данных счетчиков
На данный момент это переходное решение, которое позволяет быстро запустить рабочий MVP. В целом оно подлежит замене на собственную реализацию.
### Agents
Функции:
- прием данных не в MQTT формате, конвертация и отправка в broker.

### API Server
Сервер реализующий вспомогательные функции.

Функции:
- интеграция с платежным шлюзом
- отправка писем
- отправка смс

## Infra
Мы предлагаем использовать:
- CloudFlare
- CloudFlare Pages
- Baremetal servers at OVH.ie
Такой выбор характеризуется тем что большая часть инфраструктуры вроде брокера, и ClickHouse умеют в кластеризацию и мы сможем их горизонтально масштабировать. В то же время, используя Swarm, мы можем существенно упростить локальную разработку для разработчиков. 

Кроме этого цена на baremetal существенно ниже.
## FrontEnd development
WebPanel и AdminPanel это фактически одно приложение, мы хотели бы предложить писать его используя [sveltekit](https://kit.svelte.dev/) ([пример 1 ory интеграции](https://github.com/micleyman/sveltekit-ory-starter/blob/main/infra/kratos/kratos.yml), [пример 2](https://github.com/emrahcom/kratos-selfservice-svelte-node)) и готовую дизайн систему [IBM Carbon](https://carbon-components-svelte.onrender.com/)

Для разработки рекомендуются использовать команду из 2х frontend/fullstack разработчиков, по мере необходимости подключая верстальщика для наведения красоты. Используя kit можно так же на этом уровне реализовать интеграции с платежками и иные API, которые не завязаны на длительное выполнения
## BackEnd development
[API Server] фактически наш бекенд для реализации всего что связанно с долгими задачами. К примеру мы поставили задачу на рассылку писем всем клиентам — подобную задачу нельзя выполнять в cloudflare pages, но вполне можно выполнить на собственном сервере. 
Для управления бекендом удобно использовать либо rest api, либо mqtt топик. 

Для самого сервера предлагается использовать [fastify](https://fastify.dev/)

Мы частично функционал можем перекладывать на KIT используя функционал pages, таким образом ускоряя разработку (та же интеграция Stripe под Kit есть готовая)

## Узкие места
Агенты на данный момент являются малость черным ящиком, важно что они должны все выдавать результат в форме MQTT. Качественная реализация агентов критически важна, т.к. это единственная не кластеризуемая часть системы.

## Предлагаемый процесс
Необходимо проговорить узкие места

Необходимо проговорить то, что я мы изменили схему работы относительно изначальной

Необходимо получить Price Quotations от EMQX

Оптимально отрисовать моки экранов которые должны быть на сайте.

На этом этапе можно будет выдать оценку проекта.

После этого мы можем собрать прототип всех компонентов системы в виде группы контейнеров в docker-app.

Параллельно с этим мы можем создать рельсы web части проекта и сервера на nodejs.