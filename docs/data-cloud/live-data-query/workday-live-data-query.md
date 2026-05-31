Workday Live Data Query (LDQ) enables real-time SQL access from external systems to Workday's core business objects (Workforce and Talent objects) without using ETL or data replication. External clients query data within Workday on demand through a Python or JDBC driver.

## Live Data Query Setup

Live Data Query offers these combinations of external clients and drivers:

|External Client|Driver|Setup Instructions|
| --- | --- | --- |
|Snowflake|Python|<ul><li>[Register API Client in Workday for ISU](/docs/data-cloud/live-data-query/register-api-client-in-workday-for-isu.md)</li><li> [Set Up Live Data Query in Snowflake with Python Driver](/docs/data-cloud/live-data-query/set-up-live-data-query-in-snowflake-with-python-driver.md)</li></ul>|
|Tableau|JDBC|<ul><li>[Register API Client in Workday for ISU](/docs/data-cloud/live-data-query/register-api-client-in-workday-for-isu.md)</li><li> [Set Up Live Data Query in Tableau with JDBC Driver](/docs/data-cloud/live-data-query/set-up-live-data-query-in-tableau-with-jdbc-driver.md)</li></ul>|
|Python|Python|<ul><li>[Register API Client in Workday for ISU](/docs/data-cloud/live-data-query/register-api-client-in-workday-for-isu.md)</li><li> [Set Up Live Data Query Using Python Client and Driver](/docs/data-cloud/live-data-query/set-up-live-data-query-using-python-client-and-driver.md)</li></ul>|
|DBeaver|JDBC (JWT)|<ul><li> [Register API Client in Workday for ISU](/docs/data-cloud/live-data-query/register-api-client-in-workday-for-isu.md)</li><li> [Set Up Live Data Query in Dbeaver for ISU](/docs/data-cloud/live-data-query/set-up-live-data-query-in-dbeaver-for-isu.md)|
|DBeaver|JDBC (AuthCode Grant)|<ul><li> [Register API Client in Workday for user.user](/docs/data-cloud/live-data-query/register-api-client-in-workday-for-user-user.md)</li><li> [Set Up Live Data Query in Dbeaver for user.user](/docs/data-cloud/live-data-query/set-up-live-data-query-in-dbeaver-for-user-user.md)</li></ul>|

For the required Live Data Query downloads during Hackathon 2026, see [Live Data Query: Downloads for Hackathon 2026](/docs/data-cloud/live-data-query/live-data-query-downloads-for-hackathon-2026.md).

> Note: While we don't have instructions for exactly how to use our 2 drivers in other clients, it's completely feasible and hackers are welcome to explore any client of choice. There is nothing guarding the usage of our drivers and the LDQ usage as long as the connection is set up properly.



## Using Live Data Query

After you’ve established a data connection from the external client with the driver, you can run SQL statements to query Workday data.

* [Live Data Query Sample Queries](/docs/data-cloud/live-data-query/live-data-query-sample-queries.md)
* [Live Data Query via SQL from Agentic Tooling](/docs/data-cloud/live-data-query/live-data-query-via-sql-from-agentic-tooling.md)

## Guardrails and Limits

These guardrails and limits are subject to change:

||Maximum Value|
| --- | --- |
|Query Runtime (Execution)|30 minutes|
|Query Runtime (Queueing, Execution, Data Out)|60 minutes|
|Query Concurrency (Per Tenant)|90 queries|
|Query Concurrency (Per Environment)|100 queries|
|Query Queue Length (Per Tenant)|800 queries|
|Query Queue Length (Per Environment)|1000 queries|
|Memory per Query|20 GB|
|Query Request Size (SQL Length)|1,000,000 characters|
|Query Response Size (Per Chunk)|16 MB|
|Query Response Size (Total)|No limit|
|Query Response Cells (Rows, Columns)|No limit|

Available objects are limited to Workforce and Talent. Row-level security controls are planned for GA; table and column-level access is controlled by the ISU's security group.