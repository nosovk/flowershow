We need `pg_stat_statements` extension first.
```sql
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;
```

Stats will start collection just after it has been enabled.

If you want to reset stats use:
```sql
SELECT pg_stat_statements_reset();
```

### Check exec time and plan time
```sql
SELECT
    pss.userid,
    pss.dbid,
    pd.datname as db_name,
    round((pss.total_exec_time + pss.total_plan_time)::numeric, 2) as total_time,
    pss.calls,
    round((pss.mean_exec_time+pss.mean_plan_time)::numeric, 2) as mean,
    round((100 * (pss.total_exec_time + pss.total_plan_time) / sum((pss.total_exec_time + pss.total_plan_time)::numeric) OVER ())::numeric, 2) as cpu_portion_pctg,
    substr(pss.query, 1, 200) short_query
FROM pg_stat_statements pss, pg_database pd
WHERE pd.oid=pss.dbid
ORDER BY (pss.total_exec_time + pss.total_plan_time)
        DESC LIMIT 30;
```
Be aware, that query is truncated `substr(pss.query, 1, 200)`, if you need full query - do not limit size of responce. 
### Active queries longer than 5 sec
```sql
SELECT
  pid,
  now() - pg_stat_activity.query_start AS duration,
  query,
  state
FROM pg_stat_activity
WHERE (now() - pg_stat_activity.query_start) > interval '5 seconds';
```
### Top space consuming tables
```sql
select schemaname                                    as table_schema,
       relname                                       as table_name,
       pg_size_pretty(pg_total_relation_size(relid)) as total_size,
       pg_size_pretty(pg_relation_size(relid))       as data_size,
       pg_size_pretty(pg_total_relation_size(relid) - pg_relation_size(relid))
                                                     as external_size
from pg_catalog.pg_statio_user_tables
order by pg_total_relation_size(relid) desc,
         pg_relation_size(relid) desc limit 20;
```
### Most troublesome queries
```sql
SELECT
  (total_exec_time / 1000 / 60) as total_min,
  mean_exec_time as avg_ms,
  calls,
  query
FROM pg_stat_statements
ORDER BY 1 DESC
  LIMIT 20;
```