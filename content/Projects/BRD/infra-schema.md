## Infrastracture schema
На данный момент инфраструктура Брокарда упрощённо может быть представлена в виде следующей упрощенной схемы:

[![](https://mermaid.ink/img/pako:eNqVVMluwjAQ_RVrzoAaIBhy6KECTt3U9lTlYuIJWE3syDhAi_j3Os5GIVVVH4g98_wWJ_gIkeIIAcSJ2kcbpg25fwklITuxFUZp0u_fEqMZxuLjKeG_dB5xX3S2-WqtWbYhdy9zImSsGVHlHkK40BgZoSR5uysr5W-zxyLJFvUONVESy2YxWnGnKNdCHl4NW_8BWZq232i4fWQhd23vp7cyeztaNcecbbIL6WLUVQdJmRZsvrqAoeQddpbITK7_b2hpajfnKSsrVbPycQ5oTDSTrsM3e9VhtTjbZ634f7x2vRa7KGg6QlWdOlknsK2fZ7yCXgZ1j6sPVOKeZFcS7RfdmraLay8NV81jZS6OpslWEdTZOvmqfOfQKl8nvMlUTaAHKeqUCW7_zMeiEYLZYIohBHbKMWZ5YkII5clCWW7U66eMIDA6xx7kGWcG54LZSGldzJiE4AgHCDzPH3je0KfjyXTk-9PJtAefEAzpYEapP70ZTSaUUo-eevCllCW4Gcw8W_Q96o9HdDakw1pjwYvLA4KYJVurgW75UN5B7ipyuu-OpbBx-gYHPl9x?type=png)](https://mermaid.live/edit#pako:eNqVVMluwjAQ_RVrzoAaIBhy6KECTt3U9lTlYuIJWE3syDhAi_j3Os5GIVVVH4g98_wWJ_gIkeIIAcSJ2kcbpg25fwklITuxFUZp0u_fEqMZxuLjKeG_dB5xX3S2-WqtWbYhdy9zImSsGVHlHkK40BgZoSR5uysr5W-zxyLJFvUONVESy2YxWnGnKNdCHl4NW_8BWZq232i4fWQhd23vp7cyeztaNcecbbIL6WLUVQdJmRZsvrqAoeQddpbITK7_b2hpajfnKSsrVbPycQ5oTDSTrsM3e9VhtTjbZ634f7x2vRa7KGg6QlWdOlknsK2fZ7yCXgZ1j6sPVOKeZFcS7RfdmraLay8NV81jZS6OpslWEdTZOvmqfOfQKl8nvMlUTaAHKeqUCW7_zMeiEYLZYIohBHbKMWZ5YkII5clCWW7U66eMIDA6xx7kGWcG54LZSGldzJiE4AgHCDzPH3je0KfjyXTk-9PJtAefEAzpYEapP70ZTSaUUo-eevCllCW4Gcw8W_Q96o9HdDakw1pjwYvLA4KYJVurgW75UN5B7ipyuu-OpbBx-gYHPl9x)
Где у нас есть 2 кластера, 3 сервера, 4 окружения.

BRD new prod: это кластер для нового продакшена, в нем находится одно окружение - new prod env на одном сервере.

BRD infra old: это старый кластер, в котором находится два сервера и три окружения. На этих серверах очень медленная дисковая подсистема, и как раз их мы и хотели обновить.

### Вспомогательные инструменты кластера
В каждом кластере мы поднимаем определенное количество вспомогательной инфраструктуры, для обслуживания размещенных в кластере окружений. Элементы инфраструктуры:
- Portainer: консоль управления кластером
- Loki: система хранения логов
- Grafana: просмотр логов
- Traefik: точка входа для служб внутри кластера и окружений

### Состав окружения
Окружение состоит из группы основных и вспомогательных служб.
Основные службы:
- nginx: веб сервер
- php-fpm: запуск php
- mariadb: база данных
- redis: кеш и управление очередями
Вспомогательные службы:
- cron: запуск задач по таймеру
- walg: бекапы базы
- restic: бекапы файлов

Так же к каждому окружению подключен один из доступных s3 бакетов, для загрузки в него ассетов.

### Обновление старых серверов
Старые сервера объединены в один кластер, с единой точкой входа в виде `traefikOld`. Перемигрировать один из них на новое окружение, не сломав кластер, не представляется возможным из-за особенностей сети. Поэтому предлагается поднять рядом новый BRD infra stage, внутри которого поднять базовую инфраструктуру, после чего на него перемигрировать сами окружения.

Важно: при миграции будет гарантированный даунтайм. После миграции старые сервера можно будет вывести из эксплуатации и освободить привязанные к ним ip.




