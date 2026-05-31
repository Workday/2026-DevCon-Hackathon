**Data In** leverages Workday Prism data management tools for zero-copy external data into Workday. Unlike traditional Prism ingestion which copies data into Workday, Data In enables customers to query external data where it resides, without physical movement and copying of data. 

For the Hackathon, hackers have the opportunity to use Zero-Copy Iceberg Data In with Snowflake and Salesforce.

## Data In Setup

| External System | Setup Instructions |
| :---- | :---- |
| Snowflake | <ul><li> [Set Up Workday Security Policies for Data In](/docs/data-cloud/data-in/set-up-workday-security-policies-for-data-in.md)</li><li>[Set Up Data In with Snowflake](/docs/data-cloud/data-in/set-up-data-in-with-snowflake.md)</li></ul> |
| Salesforce | <ul><li> [Set Up Workday Security Policies for Data In](/docs/data-cloud/data-in/set-up-workday-security-policies-for-data-in.md)</li><li>[Set Up Data In With Salesforce](/docs/data-cloud/data-in/set-up-data-in-with-salesforce.md)</li></ul> |

## Example Use Case

**Total Cost of Labor Analysis with Zero-Copy Data Blending**: Leadership requires a detailed analysis of the total cost of labor across three dimensions: Base Pay, Benefits (Workday data), and Stock Compensation (external data stored in Snowflake). The traditional method of extracting sensitive Workday data for analysis in third-party applications presents a significant security risk. Workday Data Cloud and Prism Analytics solve this by enabling the secure blending of Workday data with external data (Stocks from Snowflake) to create a comprehensive visualization, eliminating the need to move sensitive labor data out of Workday.

## Prism AI Tools

You can use Prism Copilot Expression Builder, a generative AI assistant that enables you to generate complex transformation logic and sophisticated calculations using natural language. See [Prism Copilot Expression Builder](/docs/prism/prism-copilot-expression-builder.md).

Prism NLQ on Table Data is another AI assistant within Workday Prism Analytics, designed to streamline data validation and preparation by allowing users to query tables using plain conversational language. See [Prism NLQ on Table Data](/docs/prism/prism-natural-language-query-on-table-data.md).

# Related Information

* Administration Guide: [Steps: Create a Derived Dataset](https://doc.workday.com/admin-guide/en-us/reporting-and-analytics/prism-analytics/creating-tables-and-datasets/vnl1493767258306.html)  
* Administration Guide:  [Create Custom Reports](https://doc.workday.com/admin-guide/en-us/vndly-documentation/reporting/create-reports-from-templates-nrb.html)