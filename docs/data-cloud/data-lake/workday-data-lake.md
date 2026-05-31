# Overview

Workday Data Lake (or “Data Lake”) acts as the source for external zero-copy queries from partner systems such as Salesforce and Snowflake, ensuring data remains within Workday's security boundary until requested. Data Lake provides governed, scalable access to the data securely stored within the Workday environment.

Data Lake includes:

* Workday Data Connect: Enables zero-copy data access of curated Workday data between Workday and external systems through Apache Iceberg. 

* Unified Data Catalog: Delivers a simplified and accessible catalog of Workday's most valuable primary business objects, making them available as queryable tables. 

Data Lake is best suited for large-scale enterprise analytics and data warehousing use cases where periodic data refresh is acceptable, serving as an alternative to traditional ETL pipelines.

For this hackathon exercise, there will not be any additional refresh cadence from the Workday tenant into Data Lake (zero-copy reads from Snowflake and Salesforce).

## Using Data Lake from Snowflake

Data Lake has been preconfigured in the Snowflake tenants that are available during the Hackathon. 

The only requirement is the Snowflake account for the Snowflake tenant login. See [Snowflake Tenant Access](/docs/data-cloud/hackathon-tenant-and-external-partner-access.md).

See:

* [Use Data Lake in Snowflake](/docs/data-cloud/data-lake/use-data-lake-in-snowflake.md)  
* [Example: Blend Workday Data with External Datasets through Snowflake](/docs/data-cloud/data-lake/example-blend-workday-data-with-external-datasets-through-snowflake.md)

## Using Data Lake from Salesforce

Data Lake has been preconfigured in the Salesforce tenants that are available during the Hackathon. 

The only requirement is the Salesforce account for the Salesforce tenant login. See [Salesforce Tenant Access](/docs/data-cloud/hackathon-tenant-and-external-partner-access.md). 

To use Data Lake in Salesforce, create a new Data Stream using Apache Iceberg.

<img width="512" height="228" alt="image" src="https://github.com/user-attachments/assets/e5207081-87eb-4f64-9e30-ff1c2732588e" />

<img width="494" height="512" alt="image" src="https://github.com/user-attachments/assets/d7858c34-ad8f-4b7a-9c13-27f3f0d31aca" />
