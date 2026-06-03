# Set Up Data In With Snowflake

## Prerequisites

* Workday tenant and credentials provided by the Hackathon facilitators.  
* Snowflake account with access to a Snowflake tenant.  See [Snowflake Tenant Access](/docs/data-cloud/hackathon-tenant-snowflake-and-salesforce-access.md).  
* [Set Up Workday Security Policies for Data In](/docs/data-cloud/data-in/set-up-workday-security-policies-for-data-in.md)

# Setup Summary

* [Task 1: Create a Zero Copy Connection to Snowflake](#task-1-create-a-zero-copy-connection-to-snowflake-4)  
* [Task 2: Create an External Catalog](#task-2-create-an-external-catalog-7)  
* [Task 3: Explore and Validate External Tables](#task-3-explore-and-validate-external-tables-11)  
* Optional: [Enable External Data for Analysis](/docs/data-cloud/data-in/enable-external-data-for-analysis.md)  
* Optional: [Blend External Data with Workday Data](/docs/data-cloud/data-in/blend-external-data-with-workday-data.md)  
* Optional: [Create Visualizations using Snowflake Data](/docs/data-cloud/data-in/create-visualizations.md)




# Task 1: Create a Zero-Copy Connection to Snowflake

## Context 

This procedure enables Workday access to Snowflake table metadata without copying data by creating a Zero-Copy connection from Workday to your Snowflake Iceberg catalog. 

## Steps

1. Sign in the Workday tenant.  
2. Access the **Data Catalog** report.   
3. Select **Create \> Connection**.  
4. In the **Create Connection** wizard, under **Zero Copy Connectors**, select the **Snowflake** tile.

5. Enter the connection details from the external system:

| Field | Description |
| :---- | :---- |
| **Name** | The connection name must be unique in the Data Catalog.  <p>Example: Snowflake\_ZeroCopy\_\<first name\>\<last initial\>. |
| **Description** | (Optional) A brief description of the connection’s purpose. |
| **Catalog URL** | The URL of the Snowflake data catalog. The URL must use only lowercase characters. Format: https://\<account\_locater\>.snowflakecomputing.com/polaris/api/catalog |
| **Warehouse Location (Database Name in Snowflake)** | Use the preconfigured Snowflake Database: **HR\_ANALYTICS**  <br>This field is the name of the Snowflake catalog that contains your Iceberg tables. |
| **Authentication Type** | Select **Oauth 2.0 with Key Pair**.  |
| **Issuer** | Initial format (before you know the key fingerprint): <br>Format: **\<ACCOUNT\_LOCATOR\>.\<SERVICE\_USER\>** <br>Example: XTQ04827.ICEFLOW\_PRISM\_USER. |
| **Principal** | The Service Principal from Snowflake. This user will be used to authenticate the Snowflake connection. <br>Format: **\<ACCOUNT\_LOCATOR\>.\<SERVICE\_USER\>** <br>Example: XTQ04827.ICEFLOW\_PRISM\_USER |
| **Token Endpoint** | The OAuth2 token endpoint from Snowflake.<br> Format: \<Catalog\_URL\>/v1/oauth/tokens <br>Example: https://\<account\_locater\>.snowflakecomputing.com/polaris/api/catalog/v1/oauth/tokens |
| **Scopes** | Use the preconfigured Scopes value \- **session:role:ICEFLOW\_PRISM\_ROLE** <br>This field is the OAuth2 scopes from Snowflake. The scopes define what the Principal has access to when connected to the external system |

These sample screenshots illustrate how to get the \<ACCOUNT\_LOCATOR\> value. 

<img width="518" height="391" alt="image" src="https://github.com/user-attachments/assets/78fa016c-0069-44b2-b22b-c3c79b4b6df7" />


In the **Account Details** window, get the **Account locator** value.
  
<img width="514" height="364" alt="image" src="https://github.com/user-attachments/assets/6d219800-a75f-4305-82d3-15bd82860844" />


6. **Save** the connection but do **not** test it yet. 

   Before you test the connection, you must first complete the certificate/key setup in the next steps. 

   ​​​​​​After the save is complete, the ​​​​​​**Authentication Certificate**​​​​ window displays.​​​​

7. From the ​​​​​​**Authentication Certificate**​​​​ window, download the certificate in ​​​​​​PKCS\#8​​​​ format.   
   If you skipped downloading the certificate from the ​​​​​​Authentication Certificate​​​​ window, follow these steps:  
   a. Access the **Data Catalog** report.   
   b. From the **Connections** list, open the connection you just created.  
      Example: Snowflake\_ZeroCopy\_\<first name\>\<last initial\>  
   c. On the **View Connection Details** page, locate the **Authentication Certificate** section.  
   d. Click **Certificate \> Download**.  
   e. Select **PKCS\#8**.

   This file contains the RSA public key that you’ll register with the Snowflake service user.

8. **Set the RSA public key on the Snowflake service user.**  
   a. Sign in your **Snowflake** account.  
   b. Create a **SQL Worksheet** with sufficient privileges to alter the service user. 

   To create a SQL Worksheet, select **Project \> Workspace** \> Add new SQL file. 

   c. Type and run an ALTER USER command to set the RSA public key for your service user.

   Example (replace placeholders and insert the key body from the downloaded PKCS\#8 file):

   ALTER USER ICEFLOW\_PRISM\_USER   SET RSA\_PUBLIC\_KEY \= '\<Content of Downloaded PKCS\#8\>'; 

   * If your environment uses a **second RSA key**, use RSA\_PUBLIC\_KEY\_2 instead:

   ALTER USER ICEFLOW\_PRISM\_USER   SET RSA\_PUBLIC\_KEY\_2 \= '\<Content of Downloaded PKCS\#8\>';

   d. Run the following command to inspect the user and retrieve the public key fingerprint:

   DESCRIBE USER ICEFLOW\_PRISM\_USER;

   e. In the result set, locate the value of either:

   * RSA\_PUBLIC\_KEY\_FP  
   * RSA\_PUBLIC\_KEY\_2\_FP (if you used the second key)

   The value will look similar to:

   SHA256:kbbJ7tq0a4yusN9SbuS8GCeoW55/fnrm7hogC6/fo0Y=

   f. Copy this fingerprint value.

9. **Update the Workday Connection Issuer with the key fingerprint.**  
   a. Return to Workday and open your Snowflake Zero Copy connection.  
   b. Use the related action on the connection and choose **Prism Connection → Edit Connection**.  
   c. Update the **Issuer** field to append the RSA public key fingerprint.

     Use this final Issuer format:

   \<ACCOUNT\_LOCATOR\>.\<SERVICE\_USER\>.\<PUBLIC\_KEY\_FP\>

   Where:

   * \<ACCOUNT\_LOCATOR\> is your Snowflake account locator.

   * \<SERVICE\_USER\> is the service user you altered above.

   * \<PUBLIC\_KEY\_FP\> is the value copied from RSA\_PUBLIC\_KEY\_FP (or RSA\_PUBLIC\_KEY\_2\_FP).

	Example: `PSB02139.ICEFLOW_PRISM_USER.SHA256:kbbJ7tq0a4yusN9SbuS8GCeoW55/fnrm7hogC6/fo0Y=`

   d. Save the updated connection.

10. **Test the connection.**  
   a. On the connection details page, select **Test Connection**.  
   b. Confirm that you see **“Test connection succeeded”**.

   This message confirms Workday can authenticate to Snowflake using Zero Copy and access Iceberg catalog metadata.

   If the test fails, double-check:

   * Catalog URL format.

   * Token endpoint URL.

   * Service user and account locator values.

   * RSA public key correctly set on the user.

   * Issuer format including the public key fingerprint.

# Task 2: Create an External Catalog
## Context

In this next procedure, create an External Catalog in Workday that points to an external partner’s Iceberg namespace. 

## Steps

1. Access the **Data Catalog** report.   
2. On the **Data Catalog** page, select **Create \> External Catalog**.  
3. Enter the external catalog details:

| Field | Description |
| :---- | :---- |
| **External Catalog Name** | Enter a name for your external catalog. |
| **Connection** | Select the external partner Zero Copy connection you just created. |
| **Namespace** | `DEVCON2026` (exactly, all caps, no extra spaces), or another namespace provided for the hackathon. <br> This must be a namespace under the Snowflake database selected from the connection setup. |
| **Description** | (Optional) The description of the external catalog. |

5. Click **Save**. 

6. Wait for the **Success** notification that the External Catalog was created.

7. Select **Back to Data Catalog**.

8. Select External Catalog, and validate that schema sync is successful on your External Catalog

## Result

After you save the External Catalog, Workday automatically runs a **Schema Sync** to:

* Read the metadata of Iceberg tables in the specified external partner namespace.

* Register those tables as **External Tables** in Workday.

* Make table structures visible in the Data Catalog **without moving or copying data**.

The duration of this sync depends on the number and complexity of tables in the namespace.

# Task 3: Explore and Validate External Tables

## Context

Explore the external tables registered from the external partner and perform basic schema validation inside Workday.

This procedure applies to these external partners: Snowflake and Salesforce.

## Steps

1. **Locate the external partner’s external tables.**  
   a. Access the Data Catalog report.  
   b. Select **External Tables** from the left navigation.  
   c. ​In the ​​​​​​External Tables​​​​ view, confirm that all Iceberg tables from Snowflake are listed under the schema/namespace you selected during external catalog creation.

2. **Inspect an external table.** 
a. Choose any external table that looks relevant to your hackathon project.  
   b. Right-click (or double-click) the table name to open **View External Table Details**.  
   c. On the details page, review:  
      * Column names  
      * Data types  
      * Any available column statistics  
      * Sample data preview (if present)  

   d. Verify that:  
      * Data types (for example, numeric vs. string) align with the definitions in the external system.  
      * Example values look reasonable and aligned with your expectations.

# Next Steps

* Optional: [Enable External Data for Analysis](/docs/data-cloud/data-in/enable-external-data-for-analysis.md)  
* Optional: [Blend External Data with Workday Data](/docs/data-cloud/data-in/blend-external-data-with-workday-data.md)  
* Optional: [Create Visualizations using Snowflake Data](/docs/data-cloud/data-in/create-visualizations.md)
