## Цель:
Отобразить стандартную схему деплоя приложения, с пояснениями почему именно так.
## Схема:
[![](https://mermaid.ink/img/pako:eNqtVuFKG0EQfpVlQbCQSO42Vj2KUCImQoRQbX606Y_Tu5iQ5C5cLrRWBKOFFhW0Cq1YqRZqf8e00Rgb-wp7r9AXaB-hs7vn5TRXatreD72Z--abmW93Z7OE501NxwpesNRyDiUfZAwEz8AACocRfU9PnHVnizboBW3SDm3RJnLW6BmYl7SBaAvRNrx1nFX4fA6AprPCv4TDGUMwVapzgjqD6Z5AAoYhVzNYQNjzsKJbjzP45-Hrj4geAseFswl5gIvWOTvYGfxEBOiGFkS_y8s6oXUe3IHALeTUWCYW280Vm4RM3_dqP1pbvEUIq7M-UKxoVrVsUbX0P6Sa0LX8vGrrGqIfeD8N9tefI5HOmRWbd_TmmCVKLJZ1K5xGCXBz9i7WI555qlqlWLFasXULQSy3Ed1nBTo1Id29OWscPHV6Sr_wDukOPYCVoF9BJGgVQS1M3Vd8uc7RVMrr5eq5bnnZ09MS5ExPIwkN8qV6AY3xvM4KLHob_q-xpXM2gJbvAliVOz3s7Jkw5wu6JfH2tz-7ZiBy1kVtbsOrqmfzBTQ4ZSxYeqUSTJ0S-HdHTNSUadlq3vgNd1xAd9dR3FKzqqEGopIu6gAlzUK-B-LbAAGKyUIxGRQ7gjU5oZfOSy77bfSRb6WP3J8-98tlEbG_cm1zIvoNXlv8iJy6h_k8kCFmGrag2P3EKBowAM6guQ5f-mbAkXc2-pSNCNnIX8lGbiUb6Vs28u-ykf8iW4_hE9MbzKysJoyApn8019yskMI3giGGHoPEcJIZ5JJFQfQpbbFx0fZPFxEghhcwjF8bSX6-HWhljU0GnvxG-hYfSpJAu7OAs8Fp73WmAnzxAF_S9cWZyeb9W1CXycA6Ayk9UF9Vyv5EsltlgJOdq14vPyvCzQDgC_t9fVVC_PTErSTAybZqr5dvP68S4lVC_HrcuCH5vuxWIXDsJhYCJ2ZnUzOurrFJ8TU26Yp_yG_6NkS3xJ3Ptzq05QZ4S91HhNx3BMEh-OWS17BiW1U9hEu6VVKZiZcYVwbbOb0Et7kCr5pqFdgVvQwxZdV4ZJqlqzDLrC7ksJJVixWwqmUN7vaJvArjqguBQ6hbMbNq2FgZ4wxYWcLPsCJJd4ciI1Fyd1gmEVkaI9EQXsSKLA2NjgxHIsNRMkqikUh0ZDmEn_OkEfZl-RfwyOR5?type=png)](https://mermaid.live/edit#pako:eNqtVuFKG0EQfpVlQbCQSO42Vj2KUCImQoRQbX606Y_Tu5iQ5C5cLrRWBKOFFhW0Cq1YqRZqf8e00Rgb-wp7r9AXaB-hs7vn5TRXatreD72Z--abmW93Z7OE501NxwpesNRyDiUfZAwEz8AACocRfU9PnHVnizboBW3SDm3RJnLW6BmYl7SBaAvRNrx1nFX4fA6AprPCv4TDGUMwVapzgjqD6Z5AAoYhVzNYQNjzsKJbjzP45-Hrj4geAseFswl5gIvWOTvYGfxEBOiGFkS_y8s6oXUe3IHALeTUWCYW280Vm4RM3_dqP1pbvEUIq7M-UKxoVrVsUbX0P6Sa0LX8vGrrGqIfeD8N9tefI5HOmRWbd_TmmCVKLJZ1K5xGCXBz9i7WI555qlqlWLFasXULQSy3Ed1nBTo1Id29OWscPHV6Sr_wDukOPYCVoF9BJGgVQS1M3Vd8uc7RVMrr5eq5bnnZ09MS5ExPIwkN8qV6AY3xvM4KLHob_q-xpXM2gJbvAliVOz3s7Jkw5wu6JfH2tz-7ZiBy1kVtbsOrqmfzBTQ4ZSxYeqUSTJ0S-HdHTNSUadlq3vgNd1xAd9dR3FKzqqEGopIu6gAlzUK-B-LbAAGKyUIxGRQ7gjU5oZfOSy77bfSRb6WP3J8-98tlEbG_cm1zIvoNXlv8iJy6h_k8kCFmGrag2P3EKBowAM6guQ5f-mbAkXc2-pSNCNnIX8lGbiUb6Vs28u-ykf8iW4_hE9MbzKysJoyApn8019yskMI3giGGHoPEcJIZ5JJFQfQpbbFx0fZPFxEghhcwjF8bSX6-HWhljU0GnvxG-hYfSpJAu7OAs8Fp73WmAnzxAF_S9cWZyeb9W1CXycA6Ayk9UF9Vyv5EsltlgJOdq14vPyvCzQDgC_t9fVVC_PTErSTAybZqr5dvP68S4lVC_HrcuCH5vuxWIXDsJhYCJ2ZnUzOurrFJ8TU26Yp_yG_6NkS3xJ3Ptzq05QZ4S91HhNx3BMEh-OWS17BiW1U9hEu6VVKZiZcYVwbbOb0Et7kCr5pqFdgVvQwxZdV4ZJqlqzDLrC7ksJJVixWwqmUN7vaJvArjqguBQ6hbMbNq2FgZ4wxYWcLPsCJJd4ciI1Fyd1gmEVkaI9EQXsSKLA2NjgxHIsNRMkqikUh0ZDmEn_OkEfZl-RfwyOR5)
## Элементы
### CloudFlare
Мы используем CloudFlare чтобы скрыть IP адрес сервера. Не зная IP адрес, злоумышленники не cмогут определить физическое местонахождение сервера. Что защищает нас от двух проблем:
- переполнение входящего канала
  На уровне сети, даже если я настраиваю блокировку какие-то отправителей по IP подсетям, для откидывания пакета на уровне firewall он должен быть по прежнему доставлен на машину. Это создает возможность просто забить входной канал к машине, до применения любых фильтрова на самом сервере.
- abuse атаки на хостера.
  Зная IP я могу писать злые письма, о том что там детское порно лежит. Хостеру проще отключить сервер, чем разбираться с правомочностью жалобы
### Dedicated Server
Мы обычно используем Dedicated сервера, вроде OVH, потому что на физическом оборудовании есть несколько плюсов:
- более современные процессоры чем в публичных облаках (2016й год vs 2024й)
- выделенный L1\L2 кеш. Многие оптимизации современных процессоров отключены в облачных решениях из-за соображений безопасности
- просто значительно более мощное оборудование
Однако есть и минусы:
- отсутствие физического резервирования. В случае возникновения проблем с оборудованием - его замена не происходит моментально, что приводит к даунтаймам.
- физическое резервирование удваивает стоимость
- самому необходим реализовывать бекапы и прорабатывать сценарии восстановления из бекапов
### HyperV host
На физический сервер мы раскатываем Windows хост систему. Это дает несколько преимуществ:
- возможность использовать проприетарные драйвера производителя оборудования (это может давать до 20% прироста к производительности)
- запуск всей полезной нагрузки в vhdx контейнерах, что позволяет их переносить между хостами
- поддержка HA реплицирования хостов, что позволяет сделать физическое резервирование или бекапирование виртуальных машин
- возможность жестко разделить физические ресурсы между виртуальными машинами (наш app сервер никогда не заест все ресурсы, потому что есть физическая привязка ядер процессора к виртуальной машине). Единственный не разделяемый ресурс у нас это входящий канал интернета (который мы защищаем используя CF)
### Swarm кластер
Мы используем Docker Swarm, потому что он просто простой как палка и не ломается. Даже при обновлении не ломается. Даже если его случайно уронить. В целом поддержка проще чем у k8s. Для работы необходимо минимум 3 узла. Поэтому мы создаем 3 виртуальные машины и объединяем в один кластер. Один из узлов назначается мастером (обычно это инфраструктурный узел)
### Инфраструктурный узел
Отдельная VM, на которой располагаются все вспомогательные сервисы для работы сервера.
- Grafana: как интерфейс для мониторинга
- Loki: как хранилище для логов
- AlertManager: для нотификаций о проблемах
- Portainer: для доступа к контейнерам напрямую
- другие сервисы, если они нужны
### Рабочий узел
VM на котрой, собственно говоря крутиться полезная нагрузка.
- traefik: ingress контроллер, на уровне которого происходит управление сертификатами, блокировки и т.п.
- сервер приложений: само приложение, это может быть несколько контейнеров в ротации к примеру (на пример php - мы поднимаем 3 php контейнера и все запросы раскидываем между ними)
- связанные контейнеры: к примеру walg для бекапирования баз данных, odyssey как connection-pooler для базы данных и т.п.
