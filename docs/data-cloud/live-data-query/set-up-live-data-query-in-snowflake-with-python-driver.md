# Prerequisites

* [Register API Client in Workday for ISU](/docs/data-cloud/live-data-query/generating-public-and-private-keys.md).
* Snowflake account with access to a Snowflake tenant. See [Snowflake Tenant Access](/docs/data-cloud/hackathon-tenant-and-external-partner-access.md).

# Context

This diagram summarizes the architectural components used between Live Data Query and Snowflake:

Snowflake Notebook
```
└── Python (workday_dataservice wheel)
         └── JWT Bearer OAuth2 auth → Workday Token Endpoint
                  └── Trino-over-HTTPS → Workday Live Data Query service
                           └── Returns rows from Workday Unified Data Catalog
```
# Setup Summary

* [Task 1: Set Up Snowflake](#task-1-set-up-snowflake-4)
* [Task 2: Set Up Snowflake Python Notebook](#task-2-set-up-snowflake-python-notebook-13)
* [Task 3: Connect to Snowflake and Run Queries](#task-3-connect-to-snowflake-and-run-queries-18)

To use Live Data Query:

* Run [Live Data Query Sample Queries](/docs/data-cloud/live-data-query/live-data-query-sample-queries.md)
* Use [Cortex Code with Workday Data](/docs/data-cloud/live-data-query/using-snowflake-cortex-code-with-live-data-query.md)

# Task 1: Set Up Snowflake

## Prerequisites

* Gather this information from the Workday tenant:

|Information|Example or Details|
| --- | --- |
|ISU username|isu_ldq_user|
|Client ID|YTg4YjRk...(base64 string)|
|Token Endpoint|https://<host>/ccx/oauth2/<tenant>/token|
|Private key file|private-key.pem (RSA 2048, PEM format)|
|Workday Host|https://wcpdev-identity.wd103.myworkday.com (Hackathon tenant host)|

* ACCOUNTADMIN (or equivalent) role in Snowflake to create the initial role and user in Step 1.2.
All subsequent steps use the dedicated WORKDAY_LDQ_TEST_ROLE.

* The ldq-python-client-*.whl wheel file. You’ll download it in Step 1.1.

Naming convention: The Snowflake setup documentation uses WORKDAY_LDQ_TEST as the base name for all Snowflake objects:

* WORKDAY_LDQ_TEST database
* WORKDAY_LDQ_TEST_ROLE role
* WORKDAY_LDQ_TEST_USER user
* WORKDAY_LDQ_TEST_EAI integration

Replace these with names that match your organization's naming conventions. If you do, be sure to substitute consistently throughout all steps.

## 1.1 Download the Python Driver (Wheel File)

The LDQ Python connector ships as a .whl (wheel) file. Download this wheel file.

## 1.2 Create a Role and User

Create a dedicated role and user for LDQ testing instead of using ACCOUNTADMIN.

This role will have only the minimum privileges required to create and manage the LDQ objects.

This step requires ACCOUNTADMIN (or SECURITYADMIN + SYSADMIN). All subsequent steps use the new WORKDAY_LDQ_TEST_ROLE role.

Sign in Snowflake using your registered Snowflake account.

**Option A — SQL**

1. From the left navigation panel, select Projects -> Workspaces.
2. Click + Add new and choose SQL File.
3. Set your role to ACCOUNTADMIN, and run:
```
-- Create the role

CREATE ROLE IF NOT EXISTS WORKDAY_LDQ_TEST_ROLE;
-- Create a dedicated user and grant the role
CREATE USER IF NOT EXISTS WORKDAY_LDQ_TEST_USER
DEFAULT_ROLE = WORKDAY_LDQ_TEST_ROLE
MUST_CHANGE_PASSWORD = TRUE
PASSWORD = 'ChangeMe123!'; -- Replace with a strong password

GRANT ROLE WORKDAY_LDQ_TEST_ROLE TO USER WORKDAY_LDQ_TEST_USER;

-- Grant privileges needed for setup

GRANT CREATE DATABASE ON ACCOUNT TO ROLE WORKDAY_LDQ_TEST_ROLE;
GRANT CREATE INTEGRATION ON ACCOUNT TO ROLE WORKDAY_LDQ_TEST_ROLE;
GRANT USAGE ON WAREHOUSE COMPUTE_WH TO ROLE WORKDAY_LDQ_TEST_ROLE;
```
Replace COMPUTE_WH with the warehouse name you plan to use. After the database and schema are created in step 1.3, the role will automatically own those objects and have full privileges on them.

**Option B — Snowsight**

1. From the left navigation panel, select **Admin > Users & Roles > Roles**.
2. Click + Role, enter **WORKDAY_LDQ_TEST_ROLE**, and click **Create Role**.
3. Navigate to **Admin > Users & Roles > Users**.
4. Click **+ Use**r, enter **WORKDAY_LDQ_TEST_USER**, set default role to **WORKDAY_LDQ_TEST_ROLE**, and click **Create User**.
5. Grant the role to the user and grant account-level privileges as shown in the SQL above.

**Important**: Switch to **WORKDAY_LDQ_TEST_ROLE** for all remaining steps. You no longer need ACCOUNTADMIN.

## 1.3 Create Database and Schema

Create a dedicated Snowflake database and schema to hold the LDQ stage, network rule, and other objects created in the steps below.

**Option A — SQL**

Open a Snowflake Worksheet (Projects > Worksheets > + Worksheet in Snowsight), set your role to WORKDAY_LDQ_TEST_ROLE, and run:
```
CREATE DATABASE IF NOT EXISTS WORKDAY_LDQ_TEST;

CREATE SCHEMA IF NOT EXISTS WORKDAY_LDQ_TEST.LIVEDATA;
```
**Option B — Snowsight**

1. From the left navigation panel, select **Catalog > Database Explorer**.
2. Click + Database, enter **WORKDAY_LDQ_TEST**, and click **Create**.
3. Open the new database, click **+ Schema**, enter **LIVEDATA**, and click **Create**.

## 1.4 Create a Stage for the Python driver (Wheel File)

A Snowflake internal stage is used to store the wheel file so it can be installed inside a Snowflake Notebook. Create the stage first, then upload the file downloaded in step 1.1.

Follow these steps to upload the wheel file to stage.

**Snowsight**

1. From the left navigation panel, select **Catalog > Database Explorer**
2. Search for the database **WORKDAY_LDQ_TEST** in search bar and click on it
3. Click **Schemas**, then **LIVEDATA**, then **Stages**
4. Click + **Stage > Snowflake Managed**.
5. Enter **LDQ_STAGE** as the name and enable Directory table.
6. Click **Create**.

To upload the wheel file:

1. Open the **LDQ_STAGE** stage.
2. Click the **Files** tab > **+ Files**.
3. Select the wheel file from your local computer that you downloaded in Step 1.1.
4. Click **Upload**.

## 1.5 Create a Network Rule

By default, Snowflake Python Notebooks cannot make outbound network calls. A Network Rule defines which external hosts are permitted. Create one that allows HTTPS traffic to the Workday host (for OAuth2 and the live data service) and to PyPI (so pip install can resolve and download the wheel's dependencies).

**Option A — SQL**

In the same Snowflake Worksheet (with WORKDAY_LDQ_TEST_ROLE role), run:
```
CREATE OR REPLACE NETWORK RULE WORKDAY_LDQ_TEST.LIVEDATA.WORKDAY_LDQ_TEST_RULE
MODE = EGRESS
TYPE = HOST_PORT
VALUE_LIST = (
'impl-services1.wd12.myworkday.com:443',
'pypi.org:443',
'files.pythonhosted.org:443'
);
```

**Option B — Snowsight**

1. From the left navigation panel, select **Admin > Security > Network Rules**.
2. Click **+ Network Rule**.
3. Fill in the fields:

  * Name: **WORKDAY_LDQ_TEST_RULE**
  * Database / Schema: **WORKDAY_LDQ_TEST / LIVEDATA**
  * Type: **Host & Port**
  * Mode: **Egress**
  * Hosts: **<host>:443, pypi.org:443, files.pythonhosted.org:443**
Replace **<host>** with the host from your Token Endpoint URL. The same host serves both the data service and token endpoint. The **pypi.org** and **files.pythonhosted.org** entries are required so that pip install can download the wheel's dependencies at runtime.

4. Click **Create Network Rule**.



## 1.6 Store the Private Key as a Snowflake Secret

Storing the RSA private key as a Snowflake Secret prevents it from being hardcoded in notebooks or properties files. The secret is encrypted at rest by Snowflake and only accessible to notebooks that have been granted access via the External Access Integration in step 1.7.

**Option A — SQL**

In the same Snowflake Worksheet (with WORKDAY_LDQ_TEST_ROLE role), run:
```
CREATE SECRET WORKDAY_LDQ_TEST.LIVEDATA.WORKDAY_PRIVATE_KEY
TYPE = GENERIC_STRING
SECRET_STRING = '-----BEGIN RSA PRIVATE KEY-----
<% paste the full contents of your private-key.pem here %>
-----END RSA PRIVATE KEY-----';
```

**Security note**: Paste the PEM content directly into the SQL worksheet — do not save it to a file or share it in plain text. Once created, the secret value cannot be retrieved via SQL.

**Option B — Snowsight**

1. From the left navigation panel, select **Admin > Security > Secrets**.
2. Click **+ Secret**.
3. Fill in the fields:

  * Name: **WORKDAY_PRIVATE_KEY**
  * Database / Schema: **WORKDAY_LDQ_TEST / LIVEDATA**
  * Type: Generic String
  * Secret value: paste the full contents of **private-key.pem** including --BEGIN-- and --END--

4. Click **Create Secret**.

**Security note**: The secret value is encrypted at rest and cannot be retrieved after creation via theUI or SQL.

## 1.7 Create an External Access Integration

An External Access Integration (EAI) references the Network Rule and the private key Secret, and acts as the Snowflake-level permission grant that allows a notebook to use both. The EAI must be attached to your notebook before any outbound calls to Workday will succeed.

Creating an External Access Integration requires the CREATE INTEGRATION privilege on the account, which was granted to WORKDAY_LDQ_TEST_ROLE in step 1.2.

**Option A — SQL**

In the same Snowflake Worksheet (with **WORKDAY_LDQ_TEST_ROLE** role), run:
```
CREATE OR REPLACE EXTERNAL ACCESS INTEGRATION WORKDAY_LDQ_TEST_EAI
ALLOWED_NETWORK_RULES = (WORKDAY_LDQ_TEST.LIVEDATA.WORKDAY_LDQ_TEST_RULE)
ALLOWED_AUTHENTICATION_SECRETS = (WORKDAY_LDQ_TEST.LIVEDATA.WORKDAY_PRIVATE_KEY)
ENABLED = TRUE;
```
**Option B — Snowsight**

1. From the left navigation panel, select **Admin > Security > External Access Integrations**.
2. Click **+ External Access Integration**.
3. Fill in the fields:

  * Name: **WORKDAY_LDQ_TEST_EAI**
  * Allowed Network Rules: **select WORKDAY_LDQ_TEST.LIVEDATA.WORKDAY_LDQ_TEST_RULE**
  * Allowed Authentication Secrets: **select WORKDAY_LDQ_TEST.LIVEDATA.WORKDAY_PRIVATE_KEY**
  * Enabled: **toggle on**

4. Click **Create External Access Integration**.

This integration must be attached to the notebook before running any LDQ code (see Step 2).

# Task 2: Set Up Snowflake Python Notebook

## 2.1 Create a Notebook

Create a new Python notebook in a Snowflake Workspace — this is where all LDQ connection and query code will run.

1. In Snowsight, navigate to **Workspaces**.
2. Open an existing workspace or click **+ Workspace** to create one.
3. Inside the workspace, click **+ > Notebook**.

## 2.2 Attach the External Access Integration

The notebook needs explicit permission to make outbound calls to Workday. Attach the EAI created in step 1.7 to grant this access.

1. Open your notebook.
2. Click the dropdown arrow next to Connected at the top of the notebook.
3. The service details panel shows Enabled EAI's. If **WORKDAY_LDQ_TEST_EAI** is not listed, click **Manage service** to add it.
4. If you don't see the EAI listed or don't see it under **Manage service** then you can click **+ Create** new service to create a new service and attach the EAI to it.
5. Restart the notebook session if prompted.

**Important**: Without the EAI attached, all outbound HTTP calls to Workday will fail silently or with a network error.

## 2.3 Install the Wheel File

Install the **workday_dataservice** Python package from the stage into the notebook's runtime environment. This makes the **DataServiceConfig** and **create_connection** APIs available for use.

Note that the installation is per-session and must be re-run after each notebook restart. The wheel's dependencies (e.g., **trino**, **requests**, **lz4**) are not bundled — **pip** will download them from PyPI automatically. This is why the Network Rule in step 1.5 includes **pypi.org** and **files.pythonhosted.org**.

In the first cell of your notebook:
```
import sys
import subprocess
import shutil
from snowflake.snowpark.context import get_active_session

session = get_active_session()

# Replace with the correct wheel filename
session.file.get(
"@WORKDAY_LDQ_TEST.LIVEDATA.LDQ_STAGE/ldq_python_client-1.0.3-py3-none-any.whl",
"/tmp"
)

# Replace with the correct wheel filename
subprocess.check_call([
sys.executable,
"-m",
"pip",
"install",
"/tmp/ldq_python_client-1.0.3-py3-none-any.whl",
"--quiet"
])

print("Wheel installed successfully!")
```
**Important**: The wheel is installed into the notebook's session-local environment and does not persist across notebook restarts. You must re-run this cell every time you restart or reconnect the notebook. Keep this as the very first cell so it always runs first.

## 2.4 Create the Configuration

The connector accepts credentials as an in-memory dictionary via DataServiceConfig(). The private key is stored securely as a Snowflake Secret (step 1.6) and retrieved at runtime using a temporary UDF — the key is never written to disk or persisted in the notebook.

In a notebook cell, run:
```
from snowflake.snowpark.context import get_active_session
from snowflake.snowpark.functions import udf
from workday_dataservice import DataServiceConfig

session = get_active_session()
session.sql("USE DATABASE WORKDAY_LDQ_TEST").collect()
session.sql("USE SCHEMA LIVEDATA").collect()

@udf(
name="get_secret_temp",
is_permanent=False,
replace=True,
external_access_integrations=['WORKDAY_LDQ_TEST_EAI'],
secrets={'pk': 'WORKDAY_LDQ_TEST.LIVEDATA.WORKDAY_PRIVATE_KEY'}
)
def get_secret_temp() -> str:
import _snowflake
return _snowflake.get_generic_secret_string('pk')

private_key_pem = session.sql("SELECT get_secret_temp()").collect()[0][0]

config = DataServiceConfig({
'wd.authn.clientId': 'YOUR_CLIENT_ID',
'wd.authn.isu': 'YOUR_ISU_NAME',
'wd.authn.accessTokenEndpoint': 'https://YOUR_HOST/ccx/oauth2/YOUR_TENANT/token',
'wd.authn.privateKey': private_key_pem,
'wd.host': 'YOUR_HOST',
'wd.port': '443'
})

del private_key_pem
session.sql("DROP FUNCTION IF EXISTS get_secret_temp()").collect()

print("Config created successfully!")
```
**How this works**: Workspace notebooks cannot directly access Snowflake Secrets via _snowflake or st.secrets. Instead, we create a temporary UDF that runs inside the Snowflake execution environment where _snowflake is available. The UDF retrieves the secret, returns it to the notebook session, and is immediately dropped after use.

**Why this is secure**: The private key is retrieved from Snowflake's encrypted store at runtime, passed into the config object in memory, and immediately cleared with del. The temporary UDF is dropped right after use. The key is never written to disk.

**Database/schema context**: The USE DATABASE and USE SCHEMA commands are required so that the temporary UDF can be registered. This does not affect your Workday queries — they run against Workday's catalog, not Snowflake tables.

|Property|Description|
| --- | --- |
|wd.authn.clientId|Client ID from the Register API Client task|
|wd.authn.isu|ISU username (e.g., snowflake_ldq_user)|
|wd.authn.accessTokenEndpoint|Full OAuth2 token URL|
|wd.authn.privateKey|PEM private key content — retrieved from Snowflake Secret|
|wd.host|Workday service host|
|wd.port|Always 443|

**Proxy note**: If your Snowflake environment routes traffic through a proxy, set **wd.host=<proxy_host>** and update **wd.authn.accessTokenEndpoint** to point to the proxy's token endpoint.

# Task 3: Connect to Snowflake and Run Queries

## 3.1 Establish a Connection

Load the config created in step 2.4 and open a connection to the Workday LDQ service. A successful connection confirms that authentication, networking, and configuration are all correct.
```
from workday_dataservice import create_connection
connection = create_connection(config)
print("Connected successfully!")
```
## 3.2 Execute a Query

Use a cursor to send a SQL query to the Workday data service and retrieve the results. Queries run against Workday's Unified Data Catalog — not Snowflake tables.
```
cursor = connection.cursor()
cursor.execute("SELECT COUNT(*) FROM workday_core.public.worker")
results = cursor.fetchall()
print(results)
cursor.close()
```
The [Sample Queries](/docs/data-cloud/live-data-query/live-data-query-sample-queries.md) Workday data - not Snowflake tables - and must be executed through the LDQ Python connector. Paste each query into a cursor.execute() call inside your Snowflake Notebook, not in a Snowflake Worksheet.

## 3.3 Load Results into a DataFrame

For easier analysis, load query results into a pandas DataFrame using the cursor's column metadata and row data.
```
import pandas as pd

cursor = connection.cursor()
cursor.execute(
"SELECT * FROM workday_core.public.worker LIMIT 100"
)

columns = [desc[0] for desc in cursor.description]
rows = cursor.fetchall()

df = pd.DataFrame(rows, columns=columns)

cursor.close()
df.head()
```
## 3.4 Close the Connection

Always close the connection when finished to release resources on both the Snowflake and Workday sides.

connection.close()

# Next Steps

* Run [Live Data Query Sample Queries](/docs/data-cloud/live-data-query/live-data-query-sample-queries.md)
* [Use Cortex Code with Workday Data](/docs/data-cloud/live-data-query/using-snowflake-cortex-code-with-live-data-query.md)