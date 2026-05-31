## Prerequisites

* Workday tenant and credentials provided by the Hackathon facilitators.
* The ISU username.
* Generate a public key
See [Generating Public and Private Keys](/docs/data-cloud/live-data-query/generating-public-and-private-keys.md).

## Context

Register an API Client in Workday for an Integration System User (ISU). You’ll need the API Client to set up Snowflake and Tableau.

## Steps

1. Sign in the Workday tenant.
2. Access the Register API Client task.
Security: *Set Up: Tenant Setup - Security* and *Security Administration* domains in the System functional area.

3. Complete the task:

|Option|Description|
| --- | --- |
|Client Name|Enter a unique API client name.|
|Client Grant Type|Select Jwt Bearer Grant.|
|x509 Certificate|If you have an existing x509 certificate, select it from the drop-down list. To create a new one, select the ​​​​​​Create x509 Public key​​​​ option from the drop-down list. Provide a name and add the public key. For information about the public key, see [Generating Public and Private Keys](/docs/data-cloud/live-data-query/generating-public-and-private-keys.md).|
|Integration System User|Add the ISU user.|
|Scope|Select any value. This field is required but not utilized by Live Data Query.|
|Include Workday Owned Scope|Select the check box.|

4. Click **OK**.
The **Register API Client** task displays the API Client information.

5. ​​​​​​Save the API Client information from the **​​​​​​Register API Client**​​​​ page. ​​​​
You’ll need this information to configure Workday connector properties in the partner platform:

|Information|Example or Details|
| --- | --- |
|Token Endpoint|https://HOST/ccx/oauth2/TENANT/token|
|Integration System User|demo-isu|
|Client ID|generated client ID|
|Workday REST API Endpoint|https://HOST/ccx/api/TENANT<p>**Note**: You’ll need the HOST value when you configure the JDBC URL.|

## Next Step
Set up Live Data Query with your selected external client and driver combination:
|External Client|Driver|Setup Instructions|
| --- | --- | --- |
|Snowflake|Python|<ul><li> [Set Up Live Data Query in Snowflake with Python Driver](/docs/data-cloud/live-data-query/set-up-live-data-query-in-snowflake-with-python-driver.md)</li></ul>|
|Tableau|JDBC|<ul><li> [Set Up Live Data Query in Tableau with JDBC Driver](/docs/data-cloud/live-data-query/set-up-live-data-query-in-tableau-with-jdbc-driver.md)</li></ul>|
|Python|Python|<ul><li> [Set Up Live Data Query Using Python Client and Driver](/docs/data-cloud/live-data-query/set-up-live-data-query-using-python-client-and-driver.md)</li></ul>|
|DBeaver|JDBC (JWT)|<ul><li> [Set Up Live Data Query in Dbeaver for ISU](/docs/data-cloud/live-data-query/set-up-live-data-query-in-dbeaver-for-isu.md)|