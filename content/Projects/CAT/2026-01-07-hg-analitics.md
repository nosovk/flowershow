# HG Analytics

Оригинальные URL, используемые для раздачи скриптов и сбора данных:
- GET https://cat-analytics.cat-casino.workers.dev/event-stream/script/webhg.min.js
- POST https://tracker-batch-hg.cat-casino.workers.dev/event-stream/push

Для корректной работы системы необходимо настроить NGinx следующим образом:

- Все GET-запросы на `/event-stream/script/*` проксировать на `https://cat-analytics.cat-casino.workers.dev/event-stream/script/`
- Все POST-запросы на `/event-stream/push*` проксировать на `https://tracker-batch-hg.cat-casino.workers.dev/event-stream/push`

### Дополнительные заголовки
Для POST-запросов (метод push) необходимо добавлять заголовок `X-CAT-IP` с IP-адресом пользователя и `X-CAT-Original-URL` с оригинальным URL запроса.

Пример конфигурации Nginx:
```nginx
location /event-stream/script/ {
        limit_except GET {
            deny all;
        }

        set $upstream_script "https://cat-analytics.cat-casino.workers.dev";

        proxy_pass $upstream_script;
        
        proxy_ssl_server_name on;
        proxy_ssl_name cat-analytics.cat-casino.workers.dev;
        proxy_set_header Host cat-analytics.cat-casino.workers.dev;
        
        proxy_set_header Connection "";
    }

    # --- Rule 2: POST requests to /event-stream/push* ---
    location /event-stream/push {
        limit_except POST {
            deny all;
        }

        set $upstream_push "https://tracker-batch-hg.cat-casino.workers.dev";

        proxy_pass $upstream_push;

        proxy_ssl_server_name on;
        proxy_ssl_name tracker-batch-hg.cat-casino.workers.dev;
        proxy_set_header Host tracker-batch-hg.cat-casino.workers.dev;

        # Forwarding user IP info
        proxy_set_header X-CAT-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        # Forwarding origin URL
        proxy_set_header X-CAT-Original-URL $scheme://$host$request_uri;
    }
```

Подключение скрипта на фронтенде выглядит так:
```html
<script async src="/event-stream/script/weba.min.js" type="module"></script>
```
[Документация по разметке сайта тегами аналитики](./2026-01-05-nextcode-clickstream)

## Настройка для конкретных продуктов
В будущем для каждого продукта будет создан отдельный скрипт для загрузки специфичных настроек. Пример URL:
`https://cat-analytics.cat-casino.workers.dev/event-stream/script/hg-atom.min.js`

Хотя эти скрипты пока не созданы, приведенная выше конфигурация Nginx уже поддерживает их корректную работу.

Необходимость использования двух разных скриптов обусловлена уменьшением риска ошибок: модуль сбора аналитики должен быть изолирован от частых изменений, чтобы избежать ошибок при обновлениях. Второй скрипт будет содержать кастомные настройки для конкретного продукта, которые можно изменять без риска нарушить работу основного функционала.
