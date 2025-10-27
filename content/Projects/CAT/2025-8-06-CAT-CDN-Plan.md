Goal: зафиксировать этапы проекта и definition of done на каждый из этапов

Предпосылки:
 - [CDN Doc](./2025-06-26-cat-cdn)
 - [Traffic-capacity](./2025-07-22-traffic-capacity)
## Этапы
### EDGE прототип
Разработка EDGE контейнера, соответствующего требованиям (zram, caddy, shared tls)
0. R&D решения для кеширующего WS (Caddy, Traefik, Varnish, Nginx, OpenResty, Souin etc.)
	1. Гибкость настройки самого кеша, взаимодействие с origin, кастомные cache keys (важен ключ с гео)
	2. Сертификаты. Возможность использования ACME, shared storage для сертов
	3. Поддержка hot reload и in-place replacement для конфигурации (возможность включать/выключать те же логи по запросу)
	4. Бенчмарк решения
		1. TTFB
		2. Активные сессии (в т.ч. на плохих каналах)
		3. Скорость отдачи контента под нашей текущей нагрузкой
		4. Поведение при различных cache hit/miss ratio
		5. Утилизация ресурсов под в наших тест-кейсах
		6. Проверка работоспособности WebSocket (у NextCode они используются в проде)
1. Описать конфигурацию (sysctl, etc.) ядра и tcp-стека, согласно рекомендациям для подобных кейсов (ссылки добавлю)
2. Определить метрики производительности и дееспособности кеширующего сервера
	1. Решить, как мы собираем/отдаем эти метрики
3. Описать Dockerfile для провижна нашего кеширующего сервера (используем compose для удобства работы с сайдкарами?)
	1. compose ок, но по возможности я бы сделал one image for all
4. Реализация CI для билда готового имейджа на Github/Gitlab/Bitbucket
5. Playbook/installer (bash, python, etc.) с нашим кеширующим сервером
	1. Рассмотреть вариант `curl <link> | sh -c -` для максимально простого развертывания решения
### Control Plane прототип
1. docker-compose, для запуска loki или аналога и фетча метрик с edge серверов.
### MVP тест
Покупка реальных мощьностей в ру, реализация теста на одном из зеркал. 
### Реализация бизнес правил
Правила балансировки нагрузки, мониторинга.

### PostMVP: DNS



#### notes
To check: apache traffic server + apache traffic control (dns balancer included with dns sec) [example](https://github.com/apache/trafficcontrol/tree/master/infrastructure/cdn-in-a-box)

## Timelines:
2025.08.10 - 2025.08.17: RD, test and measure different solutions
2025.08.18 - 2025.08.25: milestone 1, some working edge prototype

