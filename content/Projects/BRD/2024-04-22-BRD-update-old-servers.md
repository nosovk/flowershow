### Обновление старых серверов

Старые сервера объединены в один кластер, с единой точкой входа в виде `traefikOld`. Перемигрировать один из них на новое окружение, не сломав кластер, не представляется возможным из-за особенностей сети. Поэтому предлагается поднять рядом новый BRD infra stage, внутри которого поднять базовую инфраструктуру, после чего на него перемигрировать сами окружения.

Важно: при миграции будет гарантированный даунтайм. После миграции старые сервера можно будет вывести из эксплуатации и освободить привязанные к ним ip.
