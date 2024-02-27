# Описание вакансии: Svelte FullStack
## Проект: 
Разработка бекофиса для управления онлайн играми для оффлайн казино. 

## Задачи: 
Написание и поддержка фронтенда приложения на SvelteKit.
Необходима поддержка i18n.
Интеграция с апи гейм провайдеров (предполагается наличие адаптеров к разным провайдерам)
Реализация системы прав для доступа к разным частям функционала.

Практики принятые при разработке проекта:
- TypeScript для всего, [пример типизации sveltekit](https://github.com/ivanhofer/sveltekit-typescript-showcase), можно посмотреть как для load функций sveltekit сам генерирует типы.
- Минимум внешних зависимостей в проекте (если что-то можно написать самому или завендорить из-за малого размера — так и делаем)

#### Пример задачи:
Для авторизированных пользователей ([lucia](https://lucia-auth.com/) + [D1](https://lucia-auth.com/database-adapters/pg/)) необходимо добавить метод загрузки ассетов в [CloudFlare R2](https://developers.cloudflare.com/r2/).
## Тех стек: 
[SvelteKit](https://learn.svelte.dev/tutorial/introducing-sveltekit), [Svelte](https://learn.svelte.dev/tutorial/welcome-to-svelte), [CloudFlare Pages](https://kit.svelte.dev/docs/adapter-cloudflare), [CloudFlare KV](https://developers.cloudflare.com/kv/api/), [Lucia Auth](https://www.youtube.com/watch?v=UMpKaZy0Rpc),[CloudFlare R2](https://developers.cloudflare.com/r2/get-started/), [CloudFlare D1](https://developers.cloudflare.com/d1/).

## Требования к кандидатам:
Опыт работы: 4+ года коммерческого опыта разработки на ReactJS/Svelte/Solid/Svelte или аналоге (желание работать со Svelte must have). Опыт разработки бекенда (необязательно на nodejs).
## Инструменты: 
git, browserstack, figma, jira
## Технические навыки: 
Английский: intermediate +
Умение писать переиспользуемый код.
Знание стандартов (использование fetch вместо axios, максимально использовать нативыне возможности браузера/среды исполнения)
Необходимо уметь продумывать взаимосвязь разных элементов (layout/route/kv-cache).
## Формат работы: 
удаленка, возможна частичная удаленка (Киев).

#vacancy #frontend