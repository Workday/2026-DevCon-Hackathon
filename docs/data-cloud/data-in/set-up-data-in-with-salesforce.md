# Set Up Data In With Salesforce

# Setup Summary

* [Task 1: Prepare Salesforce Data](#task-1-prepare-salesforce-data-3)  
* [Task 2: Prepare the Sharing Layer](#task-2-prepare-the-sharing-layer-4)  
* [Task 3: Create Zero Copy Connection to Salesforce](#task-3-create-zero-copy-connection-to-salesforce-5)  
* [Task 4: Create an External Catalog](#task-4-create-an-external-catalog-8)  
* [Task 5: Explore and Validate External Tables](#task-5-explore-and-validate-external-tables-12)  
* Optional: [Enable External Data for Analysis](/docs/data-cloud/data-in/enable-external-data-for-analysis.md)
* Optional: [Blend External Data with Workday Data](/docs/data-cloud/data-in/blend-external-data-with-workday-data.md)
* Optional: [Create Visualizations using Salesforce Data](/docs/data-cloud/data-in/create-visualizations.md)

# Task 1: Prepare Salesforce Data

The source data must be formatted to meet Salesforce Data Cloud’s strict schema requirements.

1. Ingest Data into Salesforce:   
   a. **Navigate:** Click on the **Data Streams** tab \> Click **New**.  
   b. **Source:** Select **Uploaded File** .

   
   ![image|690x238](upload://1HkgZBi3XwWPiG2qvEj9xtnS1e7.png)


   c. **Upload:** Drag and drop your prepared CSV.  
   d. Select the **Primary Key** .  
   e. Select **Next**.  
   f. **Data Stream Deployment Settings:**

| UI Field | Value to Enter | Notes |
| :---- | :---- | :---- |
| **Data Stream Name** |  \<data stream name\> | Custom identifier for the stream. |
| **Data Space** | default | Standard segregation layer. |

  g. **Action:** Click **Deploy**.  
  h. Make sure the Data Stream Status is **Active**.

	
![image|690x186](upload://3PBLd6EZpJhjVibMcbKPFWEcY1G.png)


# Task 2: Prepare the Sharing Layer

1. Create the Data Share:

   a. Navigate to **Data Shares** tab \> Click **New**.

   b. Specify the **Data Share Name**

   c. Select the **checkbox**. (*I acknowledge that admins or privileged users in the receiving organizations other than Salesforce Data Cloud can view and query all records of the data lake objects (DLOs) mapped to data model objects (DMOs) in the data share.*)

   d. Click **Next**.

   ![image|690x180](upload://tUvR2X8aSVj3AujCHws6298sMzw.png)


   e. Select the **Data Lake Object** created in **Step 1** to link it to your Data Share and click **Save**.

	
   ![image|690x388](upload://tMVFMVtWkd2eqasR5qS0OJ472Y1.png)


# Task 3: Create Zero Copy Connection to Salesforce

## Context 

Before ingestion, the source data must be formatted to meet Salesforce Data Cloud’s strict schema requirements. 

## Steps

1. Create Data Share Target:  
   a. In Data Cloud, go to **Data Share Targets** \> **New**.  
   b. Select **Other Targets**.

 
![datain1|690x302](upload://ceT8gwtNJtcmqvy91PZ7hbktNjU.png)



   c. Enter a Data Share Target Name (Example: HR_Analytics).  
   d. **STAY ON THIS PAGE:** Keep this window open. You will need to enter the **Issuer URL** and **Service Account ID** after you create a connection in Workday  
   e. You will also need to copy the **Tenant Endpoint** and **Core Tenant ID** from this page when you create your connection win Workday.  
        
2. Configure Workday Connection:  
   a. **In Workday:** Search for **“Data Catalog”** and open the report.  
   b. **Navigate:** Connections (left bar) \> **Create** \> **Connection**.  
   c. **Select:** **Salesforce** tile under **Zero Copy Connectors** and select **Next**.  
   d. **Enter Details:**  
      * **Name:** Example: Hackathon\_Salesforce  
      * **Catalog URL: Add https://** to the **Tenant Endpoint** from Step 1 (Data Share Target screen in Salesforce) and paste that in Catalog URL.  

![datain2|690x299](upload://9KsY7jgbbqyTXXSecPoXpDG100Q.png)

![datain4|690x487](upload://6l9DmuBWZYVSXxeLpMzIo5kLsV2.png)


 
   * **Cloud Region:** Example: Canada (Central) - ca-central-1 (This must match the region of the Salesforce instance).  
   * **Core Tenant ID:** Paste **Core Tenant ID** from Step 1 (Data Share Target screen in Salesforce).  

![datain9|690x295](upload://59xvUZur74wrjgogvW64vDKL6rK.png)


   * **Action:** Click **Save**. **Do NOT click Test yet.**  
   
   e. **Gather Keys:** On the **View Connection Details** page, locate and keep open the **Service Principal**, **Issuer**, and download the **PKCS\#8 certificate**.  
      
   ![image|359x500](upload://kwXHyOv27PGy421fap053YZnQHH.png)


     
3. Finalize Data Share Target (Salesforce):  
   a. Return to the Salesforce **Data Share Target** page.  
   b. **Service Account ID:** Paste the **Service Principal** from Workday.  
   c. **Issuer URL:** Paste the **Issuer** from Workday.  
   d. **Public Key:**   
      * Copy the contents of the **PKCS\#8 certificate** that you downloaded from Workday.  
      * **Crucial:** Remove the \-----BEGIN PUBLIC KEY----- and \-----END PUBLIC KEY----- headers/footers before saving.  

   e. Click **Save**.

![datain6|690x336](upload://iGcwhWjvIaXvbTEi85ZT6smBKQS.png)


4. Link Data Share Target:  
   a. In Salesforce Data Cloud, click on **Data Shares**. 
   b. Select the Data Share created in Task 2.
    c. Click **Link/Unlink Data Share Target**.

   ![image|689x241](upload://3zb25uG6kA8RivuBsMfF9cLQvx8.png)


  
   d. Select the **Data Share Target** created in Step 3\.  
   e. Click **Save**.

   ![image|690x162](upload://y7lOaSB0SziOfU53qwsI85hCi1C.png)


2. Test the Connection: 
   a. Return to the **Workday Data Catalog** Connection page.  
   b. Select **Test Connection**.  
   c. **Verification:** Confirm the status displays: *“Test connection succeeded.”*

# Task 4: Create an External Catalog

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
| **Namespace** | `DEVCON2026` (This is the Data Share API Name from Salesforce) |
| **Description** | (Optional) The description of the external catalog. |

5. Click **Save**. 

6. Wait for the **Success** notification that the External Catalog was created.

7. Select **Back to Data Catalog**.

8. Select **External Catalog**, and validate that schema sync is successful on your external catalog.

## Result

After you save the External Catalog, Workday automatically runs a **Schema Sync** to:

●      Read the metadata of Iceberg tables in the specified external partner namespace.

●      Register those tables as **External Tables** in Workday.

●      Make table structures visible in the Data Catalog **without moving or copying data**.

The duration of this sync depends on the number and complexity of tables in the namespace.

# Task 5: Explore and Validate External Tables

## Context

Explore the external tables registered from the external partner and perform basic schema validation inside Workday.

This procedure applies to these external partners: Snowflake and Salesforce.

## Steps

1. Locate the external partner’s external tables:  
   a. Access the Data Catalog report.  
   b. Select **External Tables** from the left navigation.  
   c. ​In the ​​​​​​External Tables​​​​ view, confirm that all Iceberg tables from Snowflake are listed under the schema/namespace you selected during external catalog creation.

2. Inspect an external table:  
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
* Optional: [Create Visualizations using Salesforce Data](/docs/data-cloud/data-in/create-visualizations.md)