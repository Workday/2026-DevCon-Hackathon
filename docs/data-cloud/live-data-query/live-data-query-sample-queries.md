## Running the Queries

These sample queries access Workday data. Execute them through the driver connector. All queries in this section use workday_core.public as the catalog and schema. Your environment may differ. Always verify first by running these discovery queries before running any data queries:

Discover available catalogs and schemas
```
SHOW CATALOGS
SHOW SCHEMAS IN workday_core
SHOW TABLES IN workday_core.public
```
Replace workday_core.public in the sample queries with your actual catalog/schema, if different.

## Count all workers
```
SELECT COUNT(*) AS total_workers
FROM workday_core.public.worker
```
## List active workers with their job titles
```
SELECT
w.worker_id,
w.full_name,
w.employee_type,
jp.job_title
FROM workday_core.public.worker w
JOIN workday_core.public.job_profile jp
ON w.job_profile_id = jp.job_profile_id
WHERE w.active = TRUE
LIMIT 50
```
## Workers by management level
```
SELECT
management_level,
COUNT(*)
AS headcount
FROM workday_core.public.worker
WHERE active = TRUE
GROUP BY management_level
ORDER BY headcount DESC
```
## Workers hired in the last 90 days
```
SELECT
worker_id,
full_name, hire_date,
business_title
FROM
workday_core.public.worker
WHERE
hire_date >= CURRENT_DATE - INTERVAL '90' DAY
ORDER BY
hire_date DESC;
```