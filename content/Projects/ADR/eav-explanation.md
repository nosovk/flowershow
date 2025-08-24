# Goal: показат пример использования EAV в BQ 
Для иллюстрации возьмем таблицу:
## user_update Table
```json
{
  "fields": [
    {
      "name": "id",
      "mode": "REQUIRED",
      "type": "STRING"
    },
    {
      "name": "created_at",
      "mode": "REQUIRED",
      "type": "TIMESTAMP"
    },
    {
      "name": "user_id",
      "mode": "REQUIRED",
      "type": "STRING"
    },
    {
      "name": "property",
      "mode": "REQUIRED",
      "type": "STRING"
    },
    {
      "name": "value_string",
      "mode": "REQUIRED",
      "type": "STRING"
    },
    {
      "name": "value_type",
      "mode": "NULLABLE",
      "type": "STRING"
    },
    {
      "name": "issuer_id",
      "mode": "NULLABLE",
      "type": "STRING"
    }
  ]
}
```

## user_snapshot_view 
```sql
WITH last_values AS (
SELECT DISTINCT
user_id,
property,
created_at,
value_string,
first_value(value_string) over (partition by user_id, property order by created_at desc) last_value_string,
FROM `analytics.user_update`
QUALIFY last_value_string = value_string AND property != 'comment' OR property = 'comment' AND trim(value_string) != ''
)
  SELECT
  user_id,
  MAX(CASE WHEN property = 'login' THEN value_string END) AS login,
  MAX(CASE WHEN property = 'first_name' THEN value_string END) AS first_name,
  MAX(CASE WHEN property = 'last_name' THEN value_string END) AS last_name,
  MAX(CASE WHEN property = 'status' THEN value_string END) AS status,
  MAX(CASE WHEN property = 'last_login' THEN value_string END) AS last_login,
  MAX(CASE WHEN property = 'registered' THEN value_string END) AS registered,
  STRING_AGG(CASE WHEN property = 'comment' THEN format_timestamp('%Y-%m-%d %H:%M', created_at) || ': ' || value_string END, '\n') AS comment
  FROM
    last_values
  GROUP BY
    user_id
```


Как работает модель EAV на примере с таблицей `user_update` и представлением (view) `user_snapshot`.
### Как EAV реализована в таблице `user_update`

Таблица `user_update` — это классический пример EAV, дополненный метаданными для отслеживания истории изменений.

- **Сущность (Entity):** `user_id` — это идентификатор пользователя.
- **Атрибут (Attribute):** `property` — это название свойства, которое изменилось (например, 'status', 'last_name').
- **Значение (Value):** `value_string` — это новое значение для данного свойства.

Дополнительные поля (`id`, `created_at`, `issuer_id`) — это **метаданные**, которые отвечают на вопросы "когда?", "кто?" и "какая именно это была транзакция?". Это делает EAV-таблицу еще и **журналом изменений**.

**Пример данных в `user_update`:**

Представим, что пользователь с `user_id = 'u-555'` сначала сменил имя, а потом статус. В таблице это будет выглядеть так:

| id  | created_at       | user_id | property   | value_string |
| --- | ---------------- | ------- | ---------- | ------------ |
| 1   | 2025-08-20 10:00 | u-555   | first_name | 'Peter'      |
| 2   | 2025-08-21 15:30 | u-555   | status     | 'inactive'   |
| 3   | 2025-08-22 11:00 | u-555   | first_name | 'Pete'       |

### Проблема EAV и как её решает вьюха `user_snapshot`

Как видно выше, чтобы получить полную и актуальную информацию о пользователе 'u-555', нужно просмотреть несколько строк и найти последние значения для каждого свойства. Это неудобно и неэффективно для аналитики.

Вьюха `user_snapshot` решает эту проблему: она **преобразует (разворачивает)** данные из "длинного" формата EAV обратно в привычный "широкий" формат, показывая только **самые последние (актуальные) значения** для каждого пользователя. Этот процесс называется **пивот (pivot)**.

### Как работает SQL-запрос во вьюхе
Разберём запрос по шагам:
#### Шаг 1: Поиск последних актуальных значений (блок `WITH last_values AS ...`)

Эта часть запроса находит для каждого пользователя и каждого его свойства самое последнее значение.


```sql
-- Для каждой комбинации (user_id, property)
-- отсортировать все её изменения по дате от новой к старой
-- и взять самое первое значение (т.е. самое свежее)
first_value(value_string) over (partition by user_id, property order by created_at desc) last_value_string
```

- `partition by user_id, property`: Эта команда создаёт "окна" или группы строк. Одно окно — это все изменения одного свойства ('first_name') для одного пользователя ('u-555').
- `order by created_at desc`: Внутри каждого окна строки сортируются по дате в обратном порядке. Самая свежая запись оказывается наверху.
- `first_value(...)`: Берёт значение из самой первой (т.е. самой свежей) строки в окне.
- `QUALIFY ...`: Это фильтр, который применяется _после_ оконной функции. Он оставляет только те строки, где значение (`value_string`) совпадает с последним найденным значением (`last_value_string`). По сути, он оставляет по одной, самой актуальной, строке для каждой пары `(user_id, property)`.


После этого шага мы получим временную таблицу, где для пользователя 'u-555' останутся только строки:

- `('u-555', 'status', 'inactive')`
- `('u-555', 'first_name', 'Pete')`

#### Шаг 2: Преобразование (пивот) данных в "широкий" формат

Это финальный `SELECT`, который превращает строки в колонки.

```sql
SELECT
  user_id,
  -- Эта конструкция превращает строку со свойством 'login' в колонку login
  MAX(CASE WHEN property = 'login' THEN value_string END) AS login,
  MAX(CASE WHEN property = 'first_name' THEN value_string END) AS first_name,
  -- и так далее для всех нужных полей...
FROM
  last_values
GROUP BY
  user_id
```

- `GROUP BY user_id`: Все строки, относящиеся к одному пользователю, "схлопываются" в одну.
    
- `MAX(CASE WHEN property = '...' THEN value_string END)`: Это и есть трюк для пивота.
    
    - `CASE WHEN property = 'login' THEN value_string END`: Эта конструкция вернёт значение логина, если в текущей строке `property` равен 'login', и `NULL` во всех остальных случаях.
    - `MAX(...)`: Так как после `GROUP BY` мы работаем с группой строк, `MAX` просто выберет единственное не-`NULL` значение (то самое значение логина) и поместит его в новую колонку `login`.

А для поля `comment` используется `STRING_AGG`, чтобы собрать все комментарии в одну строку, а не только последний. Это показывает гибкость такого подхода.

### Итого

1. **Таблица `user_update` (EAV):** Гибко хранит **историю всех изменений** в "длинном" формате. Отлично подходит для записи данных.
2. **Вьюха `user_snapshot` (Пивот):** Преобразует исторические данные в удобный для чтения и анализа **"широкий" срез актуальных данных**, показывая текущее состояние каждого пользователя.