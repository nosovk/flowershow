# Клиент: RLS
## Домен: 
iGaming, video streaming
#iGaming #VideoStreaming 
## Какие задачи перед нами ставились / Какие проблемы решали:
Разработка нового приложения с реалтайм стримингом с аппаратного комплекса для удаленного управления пользователями
## Как реализовали:
Изначально прототип использовал Janus, но в ходе дальнейшей разработки было решено искать ему замену, поскольку фронтенд библиотеку Janus необходимо было переписать целиком. Мы пришли к решению mediasoup, которое имеет очень удобную интеграцию с nodejs. Мы смогли перевести приложение на новый SFU сервер и новую фронтенд библиотеку, что существенно уменьшило лаги. Дальнейшее уменьшение лагов уже упиралось в сеть, поэтому мы пересмотрели модель работы приложения, и от many to many схемы перешли к p2p схеме взаимодействия. Такое решение позволило исключить сервер из взаимодействия и достичь еще более маленьких задержек. Переход к p2p модели однако потребовал организации собственного транспортного уровня. Приложение web ориентированное и расчитанно на большое количество не самых доброжелательных пользователей. Поэтому мы решили использовать EMQX брокер для обеспечения возможности горизонтального масштабирования как gateway для взаимодействия веб пользователей с устройствами. Использование MQTT протокола позволило использовать один протокол и на бекенде и на клиенте с возможностью балансировки нагрузки на брокере.

## Какие технологии мы использовали: 
NodeJS, Fastify, MQTT, EMQX, Docker, mediasoup, webrtc, svelte

## Сколько людей всего работали на проекте:
3-6 developers, designer, qa, PO, PM