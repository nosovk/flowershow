# Клиент: 7BIT
## Домен: 
[[iGaming]], casino
#iGaming 
## Какие задачи перед нами ставились / Какие проблемы решали:
Создание фронтенда на базе WhiteLabel решения. Необходимо было учесть нюансы с прошлой версии продукта.
- SEO 
- расширяемость новыми роутами без влияния на глобальное состояние
- переход от двух независимых версий (mobile/desktop) к одной адаптивной
- переход на использование встроенной в платформу CMS для управления содержимым продукта

## Как реализовали:
Использовали AngularUniversal для пререндера приложения на сервере, чтобы добиться FCP до 2‑х секунд и индексации поисковой системой.
Использовали NgRx и LazyLoad модули для минимизации взаимодействия частей приложения между собой и уменьшения связности кода.
Использовали адаптивную верстку с применением vw/vh/clamp где необходимо.
Создали несколько конструкторов на базе собственного DSL, для описания поведения некоторых блоков (к примеру показать что-то игрокам с определенными статусам, из определенной страны, в определенное время)

## Какие технологии мы использовали: 
#Angular, #NgRx, #AngularUniversal, #NodeJS, #Docker, #Swarm, #Traefik, #Redis, #cloudflare 

## Сколько людей всего работали на проекте:
7 developers, 1 devops