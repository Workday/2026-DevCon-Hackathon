## Overview

Live Data Query (LDQ) enables Agents to access live instance data from OMS. It is a Trino-backed SQL engine that executes SQL queries on behalf of the user. Workday exposes a relational representation of curated Business Objects and fields callable by any agent, on any platform, that has been granted access through ASOR.

Live Data Query is a read-only tool.

## Curated Catalog

Live Data Query queries against the Workday Data Cloud Catalog - a curated, relational schema where:

* Tables = Business Objects (BOs) selected for customer relevance, technical support, and security domain alignment.
  * The catalog uses two approaches to map the Workday class hierarchy into tables:
Table Per Hierarchy (TPH) - a superclass BO is used as the base table, grouping multiple concrete subclasses into a single table (e.g. staffing_event covers hire, transfer, termination, and other event subtypes). Benefits: fewer tables to understand, simpler queries. Watch-fors: tables may be sparsely populated for subclass-specific columns, and all subclasses must share the same security domain.
Table Per Concrete Class (TPCC) - each concrete subclass gets its own table. More tables to navigate but no sparsity or security mixing concerns. When in doubt, prefer TPCC.

  * The catalog's cardinal rule: a primary key may exist in only one base table. This prevents ambiguity when joining across the schema. Views can be layered on top of base tables to recreate parts of the hierarchy not directly exposed (e.g. unioning several TPCC tables to reconstitute an abstract superclass).

* Columns = Calculated Report Fields (CRFs) for those BOs (special note on Relationships = typed reference columns that encode foreign keys as integers).

## Schema

### Tables

|Schema|Count|Description|
| --- | --- | --- |
|public|293|Core HCM, talent, recruiting, compensation, and learning objects|
|fts_internal|222|Financial objects: journals, invoices, payroll, purchasing; 39 tables overlap with public (183 unique)|

Total: 476 unique Business Objects across both schemas.

### Column Types

|Type|Physical Trino Type|Description|
| --- | --- | --- |
|VARCHAR|varchar|String value|
|BOOLEAN|boolean|True/false|
|DATE|date|Calendar date|
|DECIMAL|decimal|Numeric with precision|
|BIGINT|bigint|Integer|
|TIMESTAMP|timestamp|Date + time|
|INSTANCE_SINGULAR|int|Foreign key to a single related record; join directly on this column|
|INSTANCE_NON_SINGULAR|array(int)|Foreign keys to multiple related records; requires CROSS JOIN UNNEST before joining|

## Supported Grammar

* SELECT
* DESCRIBE

## Unsupported Grammar

DO NOT USE SHOW STATS, EXPLAIN PLAN, UPDATE, DELETE, etc

## Usage Recommendation (compared to MCP - Agent Ready Rest APIs)

* Bulk reads requiring SQL - especially queries involving aggregations (GROUP BY, COUNT, AVG), multi-table joins, window functions, or filtering across high-cardinality data
* Mid - High-volume object classes where paging through MCP responses would be impractical
* Data is available in the catalog and the query reflects current effective time

## Do Not Use for:

* High-volume full extracts - avoid queries like SELECT * FROM worker or SELECT * FROM journal_line without meaningful filters. Keep target cell count (columns rows) under 1,000,000
* A specific effective date is required - LDQ doesn't currently support setting a custom effective date; it always reflects current effective time. If the use case requires point-in-time queries as of a specific date (including Loop Effective Dating scenarios), use MCP if those APIs support effective dating for that domain
* Write operations of any kind - creates, updates, deletes, submits, and process actions

LDQ does include event and log-style tables (e.g. staffing_event, job_history) that capture records of what occurred over time. These are useful for understanding historical activity, but they still reflect the current state of those records - LDQ doesn't support querying the full dataset as it existed at an arbitrary past point in time.

Decision Rule

* Bulk read, rich-SQL grammar needed, current effective time -> LDQ (if data exists in catalog)
* All other cases -> MCP

## Other Notable Best Practices

Performance: Filters

* Prefer indexed fields when filtering, especially on high-volume objects (>100k rows); indexed columns are marked in the catalog schema (functionally this often equates to adding a date range filter on large tables, including within CTES)
* Filter on UUID, not display ID when traversing relationships
* Filter zero/null amounts early in base CTEs do not carry nulls through joins
* Filters are especially important in join scenarios; apply them before joining, not after

Performance: SELECT/Projections

* Avoid SELECT * - project only the columns you need. This is particularly important on large tables like worker and journal_line

Performance: Joins

* Add selective match conditions directly in the JOIN ON clause not in a downstream WHERE or CASE
* Remove redundant conditions (e.g. join-enforced checks duplicated in CASE expressions)
* Never self-join without pre-filtering the base dataset first
* Traversing multi-value relationships (INSTANCE_NON_SINGULAR columns are array(int)): use CROSS JOIN UNNEST to expand the array before joining to the referenced table. Apply filters before the unnest to reduce the rows expanded

Performance: Window Functions

* Always pair window functions with ORDER BY and PARTITION BY on large datasets
* Move ranking functions (e.g. NTILE) late in the query - apply them over aggregated results rather than over raw join output rows in CTES

Performance: Sorting and Limiting

* Always include a LIMIT
* Avoid ORDER BY on large result sets without a LIMIT
* LIMIT inside a CTE doesn't reduce upstream table scans - use date filters at the source to control scan volume

Functional

* Confirm the relevant tables exist in the catalog before routing a use case to LDQ - coverage varies by domain (financial data lives in fts_internal; some domains like revenue, expense, and helpCase have no LDQ equivalent and require MCP)
* LDQ reflects current effective time only - If the question is "what was true on date X," that's not an LDQ use case today

## Example Usage

LDQ is currently used by the 1P Fin Test Suite (FTS) Agent (84713$32). LDQ Fin Operation (109003$1, f4a1b840005610000ec5f0aaa9220000)

## A few technical Details

### Required Agent Tools

To enable an Agent to access LDQ, the following Agent Tools must be added to the Agent Definition Skill:
```
tool-wids:
- name: LDQ-Workday Core Public
  wid: dafc7b8890fd10002a9cb873ddc40000
- name: LDQ-Financials Agent
  wid: f4a1b840005610000ec5f0aaa9220000
- name: wql/data/view (GET) (v1)
  wid: 9749ba9ac47e1000073fdc1bc8a5001a
- name: data
  wid: 95dd8596f7e8100014b18d3ad68c0732
- name: wqlInternal/data/view (GET) (v1)
  wid: 1559fe9e0a491000157e39494d5400fd
- name: wqlInternal/data/view (POST) (v1 - ) +TG
  wid: 1f8011547126100005e8cfb7658d01f7
- name: wql/data/view (POST) (v1) +TG
  wid: 3e15b2a1676d10000c9572ea9cc264b3
```

### Why the above Tools are required

LDQ routes queries through the BI Connector, which in turn calls WQL over HTTP using the caller's ASU token. The token's scope claim must include both the LDQ skill and the WQL skills.

At the platform level, every WQL request is subject to a scope check (Security Policy Evaluator -> OAuth2Utils.isScopeValidForSecured). This check validates that the Agent Definition's allowed skills include the skill required by the secured WQL resource being accessed. If the WQL Agent Tools (Post WQL Internal+TG, Get WQL Internal+TG) are absent from the Agent Definition, the scope check fails and all queries are rejected with a 403 - permission denied error, regardless of whether the LDQ skill itself is present.