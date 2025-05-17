### BigQuery, explore expenses

[youtube video](https://youtu.be/cMDC_ugIgK4)

Problem: if you use BQ, certainly, one day you will have a bill, that will be to big just to ignore it. And reasonable action is to find out an answer on a question... WTF? Why so much?

Actually Google stores info about all executed queries, they just named this info as "Jobs". There are a special datasets filled with data about BQ usage, for our case the most promising one is `INFORMATION_SCHEMA.JOBS_BY_PROJECT`

You can check docs on it by visiting that link:
https://cloud.google.com/bigquery/docs/information-schema-jobs

We are interested in "total_bytes_billed" column, there is our costs for each query. We can convert it to real dollars within simple math.

Let's make a few example queries.
We want to get day by day costs for last 30 days:
```sql
SELECT
  DATE_TRUNC(jobs.creation_time, DAY) AS job_date,
  SUM(jobs.total_bytes_billed/1024/1024/1024/1024 * 5) AS job_cost_usd_total
FROM
  region-eu.INFORMATION_SCHEMA.JOBS_BY_PROJECT jobs
WHERE
  jobs.creation_time >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 30 DAY)
GROUP BY
  job_date
ORDER BY
  job_cost_usd_total DESC;
```

Most expensive queries:
```sql
SELECT
  job_id,
  (jobs.total_bytes_billed) AS job_cost_usd
FROM
  region-eu.INFORMATION_SCHEMA.JOBS_BY_PROJECT jobs
WHERE
  jobs.creation_time >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 7 DAY)
ORDER BY
  job_cost_usd DESC;
```

