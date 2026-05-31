# Summary

* [Task 1: Access Workday Zero-Copy Tables in Snowflake](#task-1-access-workday-zero-copy-tables-in-snowflake-2)
* [Task 2: Explore Tables in Workday Catalog](#task-2-explore-tables-in-workday-catalog-6)  
* [Task 3: Use Cortex Code to Query Workday Data](#task-3-use-cortex-code-to-query-workday-data-10)

# Task 1: Access Workday Zero-Copy Tables in Snowflake

## Prerequisite

* Snowflake account with access to Snowflake instance. See [Snowflake Tenant Access](/docs/data-cloud/hackathon-tenant-and-external-partner-access.md).


## Context 

Use Database Explorer in Snowflake to review the Workday zero-copy tables in the catalog. With Zero-Copy connector, your sensitive Workday data does not leave Workday and is available to be analyzed LIVE from any tools in Snowflake.

> **Note**: This is a synthetic data-set for a hypothetical GMS organization. Data may not be available in all tables.

 

## Steps

1. Sign in Snowflake using your registered Snowflake account.
2. On the left-hand navigation, in the Horizon Catalog section, select **Catalog > Database Explorer**.
The **Databases** list includes **WORKDAY_ZERO_COPY_DB**, which is the Workday Data Cloud catalog linked as a zero-copy database.

3. Select the **WORKDAY_ZERO_COPY_DB** to display its schema.
**Note**: The database icon is unique with a Connector symbol signifying a linked zero-copy database connection.

4. Select the **workday_core** schema and then select **Tables**.
5. In the **Tables** list, find and select the **campaign** table.
6. On the **Table Details** panel, select the **Data Preview** tab.
7. Explore other tables per your interest.

   This example displays the campaign table data:
<img width="998" height="731" alt="image" src="https://github.com/user-attachments/assets/ccc3d653-9bb5-4fe8-98c5-9c4d84018755" />



# Task 2: Explore Tables in Workday Catalog

## Prerequisite

* Snowflake account with access to Snowflake instance. See [Snowflake Tenant Access](/docs/data-cloud/hackathon-tenant-and-external-partner-access.md).


## Context 

In Snowflake, you can run a pre-provisioned SQL file which includes both simple and complex queries on the zero-copy tables. The SQL query enables you to understand how the Workday Catalog is structured and how to join tables using Array field types, also enabling you to write your own SQL’s, or use CoCo to create SQL’s for your questions. 

## Steps

1. On the left-hand navigation panel, select **Projects \> Legacy Notebooks.**

<img width="585" height="387" alt="image" src="https://github.com/user-attachments/assets/f0d58047-64ce-42b7-bf26-08296acc8f76" />



2. Select the **WORKDAY_EXPLORATION_SCRIPTS** file. This action opens the SQL examples in a python notebook in a separate Window. Review & execute the scripts to understand how data is organized in Workday’s Catalog and relationships between tables are established based on **Scalar FK (SQL’s 3,4)** and **Array FK (SQL’s 5, 10)** join types.
   
<img width="424" height="324" alt="image" src="https://github.com/user-attachments/assets/210a3b23-7af1-4f7c-adf3-950c374710fb" />
<br>
<img width="998" height="588" alt="image" src="https://github.com/user-attachments/assets/cd410fec-3569-4fae-acce-94a5173da098" />




# Task 3: Use Cortex Code to Query Workday Data

## Prerequisite

* Snowflake account with access to Snowflake instance. See [Snowflake Tenant Access](/docs/data-cloud/hackathon-tenant-and-external-partner-access.md).


## Context 

You can use Cortex Code AI in Snowflake to create a SQL script that will get exactly what you want. **Cortex Code** is Snowflake’s native AI-driven "intelligent agent" designed to assist with the entire lifecycle of data development—from writing SQL to building complex ML pipelines. 

## Steps

1. In the left-hand navigation panel, select **Projects \> Workspaces**.

<img width="464" height="396" alt="image" src="https://github.com/user-attachments/assets/322ec51d-d6a2-4274-9b10-09fc4815caf7" />


2. Select **\+ Add new \> SQL File**, and name the file MyScript-\<your first name and last name initial\>. 

   Example: MyScript-OscarB.

<img width="496" height="353" alt="image" src="https://github.com/user-attachments/assets/38782946-2f41-466e-afac-6d4fad4b8474" />


3. Write a natural language prompt that will create a SQL script that will join two tables. For your convenience, copy the prompt below.

   `Using the WORKDAY_ZERO_COPY_DB.WORKDAY_CORE schema and the COMPUTE_WH warehouse, write a SQL script that joins the JOB_PROFILE table to the MANAGEMENT_LEVEL table. Return the job profile name, job profile summary, and the management level display name. Sort results by management level, then by job profile name.`

4. In the lower-right corner of the page, select the blue **Cortex Code** icon and paste the prompt into the chat window. After a moment, Cortex Code will generate a SQL script. 

<img width="354" height="394" alt="image" src="https://github.com/user-attachments/assets/42928eb1-939f-4c69-821b-f18e281ad37f" />
<br>
<img width="999" height="707" alt="image" src="https://github.com/user-attachments/assets/35443d7c-ece2-4bd8-9763-afd53639fd43" />


5. Accept / Modify the recommended SQL from AI (click on **Keep** in SQL editor, or **Keep all** on cortex code 

<img width="1024" height="842" alt="image" src="https://github.com/user-attachments/assets/95d6b856-88ac-4403-ad03-855a66758323" />



6. Run the script by clicking on the Play button.

7. View the results.
<img width="1127" height="1390" alt="image" src="https://github.com/user-attachments/assets/d478d76f-ca90-45e3-80fb-3787cbc5aaaa" />
