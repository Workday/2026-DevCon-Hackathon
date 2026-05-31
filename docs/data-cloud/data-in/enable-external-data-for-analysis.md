# Data In (Optional): Enable External Data for Analysis

## Context

You can make an external table available as a report data source inside Workday. When the schema is synced, activate the tables you wish to use for reporting.

This procedure applies to these external partners: Snowflake and Salesforce.

## Steps

1. Access the **Data Catalog** report. 

2. In the left navigation menu of the Data Catalog, click **External Tables**.

3. Locate an external table associated with your External Catalog.

   Example: A table you inspected in the Explore and Validate External Tables task ([Snowflake](/docs/data-cloud/data-in/set-up-data-in-with-snowflake.md#task-4-explore-and-validate-external-tables-12)) ([Salesforce](/docs/data-cloud/data-in/#task-5-explore-and-validate-external-tables-12)).

4. Right-click the table and select **Edit**.

5. Check **Enable for Analysis**.

6. Select **Save**.

## Result

Enabling the external table for analysis transforms the external table into a reporting-ready data source. 

## Next Steps

You can use the external table as a data source in Prism, Discovery Boards, or other analytical experiences according to your own hackathon scenario.