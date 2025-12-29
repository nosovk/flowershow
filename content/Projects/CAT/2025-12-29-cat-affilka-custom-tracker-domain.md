# Реализация кастомного домена трекера Affilka

Входные данные: [2025-12-10-affilka-proxy (тз для аффилки)](./2025-12-10-affilka-proxy)

## Реализация

Для работы с Affilka используется [базовый Cloudflare Worker](https://nodeart.app/git/projects/CAT/repos/cat.workers/browse/src/affilka-inline-base). Он реализует логику проксирования запросов, содержит встроенное логирование и переиспользуется между различными продуктами.

Для каждого конкретного продукта создается отдельный экземпляр воркера с соответствующими настройками ([пример конфигурации](https://nodeart.app/git/projects/CAT/repos/cat.workers/browse/src/affilka-inline-atom/wrangler.toml)).

### Конфигурация (wrangler.toml)

Основные параметры конфигурации:

*   `account_id`: ID аккаунта Cloudflare, в который будет развернут воркер.
*   `name`: Имя воркера в Cloudflare.
*   `AFFILKA_DOMAIN`: Базовый домен трекера Affilka, на который необходимо перенаправлять запросы.
*   `PRODUCT`: Идентификатор продукта, к которому относится трекер.
*   `ADC_ENCODED`: Ключ для записи данных в BigQuery.
*   `BIGQUERY_DATASET_NAME`: Имя датасета в BigQuery.
*   `BIGQUERY_TABLE_NAME`: Имя таблицы в BigQuery.

## Диаграмма последовательности (Sequence Diagram)

[![](https://mermaid.ink/img/pako:eNp9kk2PmzAQhv_KyNcSAiEJwVK3Qkm12sOqSMmpQqpGZgIWYFNj0qZR_nsN2TRtD-uTP-Z95p3xXJjQBTHOevo-kBK0k1gabHOFwmoDGWAPWYNnMrnq0FgpZIfKwmF82A691S0cDIpaqhJ2ukWp_g1Mx8D0eJRNje9Hbick9lLpXIFb2ezp6cOBw5eO1EPaSFVzqKztej6ft4RNSwob4QvdzvuqPdVhdQxuhMNISDm8Yk2A0JM5kQEz1trbB6Rr5Ekb9KX-H5DOHGHmPOxJFdBogVZqBRVh4UA_pK0crZCGhIXCMaWaAjiIqY7J06feYvlxrFSRedl9E40U9cvu7nBMkHF4lieCKFg8eFbD35x3MRmMHNhyKPUovMluBveH9BmkmhoHzGOlkQXj1gzksZaM-wh3ZJcRlDNbUUs5425boKlzlqur07gP-qp1e5cZPZQV40dsencaugLtfXL-3BrXMTJbPSjLeBItJgjjF_aT8VXsrxfJOorCVRwGcbD02JnxcBX5cRi622W4idbJ4uqxX1PWwN_Ey8StOFmFQRIEG4_hYPX-rMTdk2ubm9jX2zhPU3139nl6eTN2_Q15lfjq?type=png)](https://mermaid.live/edit#pako:eNp9kk2PmzAQhv_KyNcSAiEJwVK3Qkm12sOqSMmpQqpGZgIWYFNj0qZR_nsN2TRtD-uTP-Z95p3xXJjQBTHOevo-kBK0k1gabHOFwmoDGWAPWYNnMrnq0FgpZIfKwmF82A691S0cDIpaqhJ2ukWp_g1Mx8D0eJRNje9Hbick9lLpXIFb2ezp6cOBw5eO1EPaSFVzqKztej6ft4RNSwob4QvdzvuqPdVhdQxuhMNISDm8Yk2A0JM5kQEz1trbB6Rr5Ekb9KX-H5DOHGHmPOxJFdBogVZqBRVh4UA_pK0crZCGhIXCMaWaAjiIqY7J06feYvlxrFSRedl9E40U9cvu7nBMkHF4lieCKFg8eFbD35x3MRmMHNhyKPUovMluBveH9BmkmhoHzGOlkQXj1gzksZaM-wh3ZJcRlDNbUUs5425boKlzlqur07gP-qp1e5cZPZQV40dsencaugLtfXL-3BrXMTJbPSjLeBItJgjjF_aT8VXsrxfJOorCVRwGcbD02JnxcBX5cRi622W4idbJ4uqxX1PWwN_Ey8StOFmFQRIEG4_hYPX-rMTdk2ubm9jX2zhPU3139nl6eTN2_Q15lfjq)

## Архитектурные особенности

Важно понимать, что данное решение является оберткой (wrapper) над оригинальным трекером Affilka. Это означает прямую зависимость: если оригинальный трекер Affilka недоступен, наш сервис также не будет работать.

### Решаемые задачи

Наш трекер решает две основные проблемы (подробнее в [оригинальном документе](./2025-12-10-affilka-proxy)):

1. BYIP (Bring Your Own IP): Позволяет использовать трекер с нашими собственными IP-адресами, что улучшает доставляемость и контроль репутации.
2. Embedded Mode (Встраиваемый режим): Позволяет встраивать трекер непосредственно в структуру продукта (на том же домене). Пример: `https://ganamax.mx/aff/s085cda74`. Либо внутрь лендинга или к примеру редиректора (или к примеру возможность делать отложенной инициализации).
3. Analytics: у нас появляется дополнительный датасет с информацией обо всех кликах (ранее мы выгружали эту информацию из аффилки, в реализации с прокси мы можем напрямую собирать эту информацию на уровне прокси) 

#### Минусы подхода
- дополнительные задержки, при использовании трекера напрямую нет возникает задержки от враппера.
