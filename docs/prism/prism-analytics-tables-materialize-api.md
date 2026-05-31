The **Materialize API** provides a high-performance, developer-friendly method to load data into Prism tables. By utilizing stable API names and native asynchronous callbacks, it eliminates the maintenance overhead of traditional Data Change Tasks (DCTs) and simplifies cross tenant portability of Orchestrations.

## **Key Capabilities**

**API Name Stability** : Unlike legacy APIs that rely on environment specific WIDs, this endpoint uses **apiNames** for both source and target. Since API names remain consistent across tenants, your orchestration code works seamlessly across environments without needing to first resolve a WID from an API name and then call a WID based API.

**Direct App Source Loading** : Load data directly from an **App Source** (Reports, SFTP, S3 and soon other Connector types) into a Target Table. This bypasses the need to create intermediate "Prism Data Change" objects, reducing your application's footprint.

**Pipeline Materialization** : Use this API to load **Derived Datasets** or Pipelines directly into tables, ensuring your reporting layer is always backed by the latest transformed data.

**Native Orchestrate Support** : Fully integrated with the **Send Prism Request** component in Workday Orchestrate.

**Asynchronous Callbacks** : Orchestrate automatically provides a callback URI, pauses execution, and resumes only when Prism confirms job completion.

**Zero Timeout Issues** : Long running jobs do not count against Orchestrate’s processing limits, as the orchestration "sleeps" until the callback is received.

## **API Reference**

### **Endpoint:**

```
POST /api/prismAnalytics/v3/tables/materialize
```
### **Request Body:**

### The payload requires the unique **apiName** for both source and target.

|**Field**|**Type**|**Description**|
| --- | --- | --- |
|source|String|The **apiName** of the source (App Source, Table, or Dataset).|
|target|String|The **apiName** of the target Prism Table.|
|operation|String|**Insert** or **TruncateAndInsert**.|

### **Example Request**
```
{
  "source": "apiName_Source",
  "target": "apiName_Target",
  "operation": "TruncateAndInsert"
}
```

## **Security & Access Requirements**

### **Principal Identity**

The API supports standard auth protocols but recommends to use ISU access token

### **Permissions & Scoping**

Execution will only succeed if the authenticated identity has authorized access to both ends of the data pipeline:

**Source Access** : The user must have "View" or "Read" access to the **apiName** referenced in the **source** field (e.g., the specific Report, or Derived Dataset).

**Target Access** : The user must have "Manage" or "Edit" permissions for the Prism Table referenced in the **target** field to perform **Insert** or **TruncateAndInsert** operations.

## **Validation & Schema Rules**

To ensure a successful data load, the source and target must satisfy these requirements:

**Schema Fit** : The **Target** table can have a larger schema than the source. Any target fields not present in the source will be populated with null.

**Source Constraint** : The **Source** cannot contain fields that do not exist in the target. If the source has more fields than the target, the validation will fail.

**Field Matching** : Each field in the source must have a corresponding field in the target with the **exact same API name** and a **compatible data type**.

**Operation Support** : Currently supports **Insert** and **TruncateAndInsert**. Support for **Update**, **Upsert**, and **Delete** is on the roadmap.

## **Verification & Monitoring**

### **UI Verification**

You can monitor the status of the load in the **Activities** tab of the Target Table.

**Success Criteria** : Status will show as "Successful" with the total rows processed.

**System Fields** : Fields like **WPA_LoadID** or **WPA_LoadTimestamp** are managed by the system and will be ignored during schema matching.

### **API Verification**

Retrieve full activity details using:
```
GET /api/prismAnalytics/v3/tables/activity/<activity_wid>
```
## **Orchestrate Integration**

When used within Workday Orchestrate using **Send Prism Request**, the system handles the asynchronous handshake automatically. Upon completion, Prism sends a callback payload to Orchestrate:

XML
```
<wd:Prism_Orchestration_Service_Callback_Payload  xmlns:wd="urn:com.workday/bsvc">
  <wd:id>69855bbea0389000bd112984ca830000</wd:id>
  <wd:status>Load completed successfully to Table. Total rows processed: 22</wd:status>
  <wd:state>Success</wd:state>
</wd:Prism_Orchestration_Service_Callback_Payload>