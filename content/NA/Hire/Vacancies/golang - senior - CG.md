# Описание вакансии: GoLang Senior
## Проект: 
Проект состоит из двух глобальных доменов: 
- Разработка агрегатора игр (слоты других студий, интеграция игры в другие агрегаторы и казино)
- Разработка АПИ для собственно игр сделанных на нашей платформе.


## Задачи: 
На данный момент есть реализация части функционала на python, и начат процесс переписывания на python.
Необходимо провести дезинтеграцию бекенда агрегатора игр от бекенда АПИ для разработки игр (это два разных бизнес домена, они не должны жить вместе).
Необходимо уменьшить задержки на уровне АПИ агрегатора (чем меньше задержка, тем меньше людей отваливается).
Необходимо переработать АПИ, разделив его на внутренее и публичные адаптеры к разным клиентам.

Практики принятые при разработке проекта:
- Сейчас этой практики нет, но ее планируется ввести: типизация контрактов коммуникации между сервисами. Звучит банально, но сейчас в python реализации этого нет.
- Минимум внешних зависимостей в проекте (если что-то можно написать самому или завендорить из-за малого размера — так и делаем)

#### Пример задачи:
## Тех стек: 
golang, gin, pgsql, redis, gin (сейчас есть gorm, хотим избавиться)
## Требования к кандидатам:
Опыт работы: 4+ года коммерческого опыта разработки на GoLang
## Инструменты: 
git, k8s, gitlab ci, metabase
## Технические навыки: 
Английский: intermediate +
Умение писать переиспользуемый код.
Знание стандартов (REST api, http cache etc)
Умение продумывать взаимосвязь разных элементов (api, cache, db, settings).
## Формат работы: 
удаленка, возможна частичная удаленка (Киев).

#vacancy #backend