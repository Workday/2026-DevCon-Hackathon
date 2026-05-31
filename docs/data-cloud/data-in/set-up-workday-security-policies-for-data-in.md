## Prerequisites

Zero Copy Iceberg Data In requires the following access and security configuration: 

*  Administrator-level access.
* Security domains in the Prism Analytics functional area:

   * *External Catalog Create*: Controls who can create a catalog.

   * *External Catalog Manage*: Controls who can edit a catalog, including access to view existing catalogs, edit catalog metadata, and delete catalogs.

   * *External Catalog Owner Manage*: Controls who can perform specific catalog operations, such as sharing the table with other users and synching the catalog.

   * *External Tables Manage*: Controls who can edit a table, including viewing the table and metadata, editing table metadata, and enabling or disabling for analysis.

   * *External Tables Owner Manage*: Controls who can perform specific table operations, such as sharing the table with other users.  

## Context

Domain security policies secure access to configurable items in Workday. Configure the assignable roles for the Data In security domains, and set up the security policy for each of the prerequisite security domains.

## Steps:

1. Sign in to the Workday tenant. 

>   **Note**: Your user account must have administrator access.

2. Access the **Maintain Assignable Roles** task.

3. In the **Maintain Assignable Roles** grid, add rows for each of these role names, with their corresponding role assignment values:

| Role Name | Workday Role | Enabled for | Self-Assign | Is Leader / Is Supporting | Role Assignees Restricted To | Assigned/Reviewed By Security Groups |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| External Table Editor | 02\. External Table Editor \- Prism | Prism External Table | (None) | None of the Above | <ul><li>Accountant (Unconstrained)</li><li>Data Analyst</li></ul> | <ul><li>Data Administrator</li><li>Prism External Table Owner (Workday Owned)</li><li>Prism Table Owner (Workday Owned)</li></ul> |
| External Table Owner | 01\. External Table Owner \- Prism | Prism External Table | Yes | Is Leader | (None) | <ul><li>Data Administrator</li><li>Prism External Table Owner (Workday Owned)</li><li>Prism Table Owner (Workday Owned)</li></ul> |
| External Table View | 03\. External Table Viewer \- Prism | Prism External Table | (None) | None of the Above | <ul><li>Accountant (Unconstrained)</li><li>Data Analyst</li></ul> | <ul><li>Data Administrator</li><li>Prism External Table Owner (Workday Owned)</li><li>Prism Table Owner (Workday Owned)</li></ul> |
4. Repeat these substeps to edit the security policy for each required security domain. 

   a. In the Search box, enter **domain: \<security domain name\>**. Example: *domain:External Catalog Create.*

   b. On the **View Domain \<Security Domain\>** page, click the related actions menu and select **Edit Security Policy Permissions**. For each of the security domains in this table, add the security groups and their corresponding permissions:  
      


      | Security Domain | Report/Task Permissions Security Groups | Report/Task Permissions | Integration Permissions Security Groups | Integration Permissions |
      | :---- | :---- | :---- | :---- | :---- |
      | External Catalog Create | View and Modify for Prism: Tables Owner Manage | View/Modify | Get and Put for Prism: Tables Manage  | Get/Put |
      | External Catalog Manage | <ol><li>View and Modify for Prism: Tables Manage</li><li>View and Modify for Prism: Tables Manage</li>  | <ol><li>View</li><li>View/Modify </li> | Get and Put for Prism: Tables Manage | Get/Put |
      | External Catalog Owner Manage | View and Modify for Prism: Tables Owner Manage | View/Modify | Get and Put for Prism: Tables Manage | Get/Put |
      | External Tables Manage | <ol><li>View Only for Prism: Tables Manage</li><li>View and Modify for Prism: Tables Manage</li>  | <ol><li>View</li><li>View/Modify</li>  | Get and Put for Prism: Tables Manage | Get/Put |
      | External Tables Owner Manage | View and Modify for Prism: Tables Owner Manage | View/Modify |   Get and Put for Prism: Tables Manage| Get/Put |

   c. Click **OK**.  
      The **Edit Domain Security Policy Permissions** page displays the updated configuration.

   d. Click **Done**.
8. Access the **Activate Pending Security Policy Changes** task.

   a. Describe your changes in the **Comment** field.

   b. Click **OK**.

   c. Select the **Confirm** check box to activate your changes.

   d. The **View All Security Timestamps** report displays the confirmation  

   e. Click **OK**.

## Result

Your Data In security domain policies have been updated and activated.

## Next Step
Set up Data In with your selected external partner:
* [Set Up Data In With Snowflake](/docs/data-cloud/data-in/set-up-data-in-with-snowflake.md)
* [Set Up Data In With Salesforce](/docs/data-cloud/data-in/set-up-data-in-with-salesforce.md)