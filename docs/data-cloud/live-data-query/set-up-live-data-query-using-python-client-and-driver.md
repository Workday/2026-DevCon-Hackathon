# Prerequisites

* [Register API Client in Workday for ISU](/docs/data-cloud/live-data-query/register-api-client-in-workday-for-isu.md).
* Gather this information from the Workday tenant:

|Information|Example or Details|
| --- | --- |
|ISU username|isu_ldq_user|
|Client ID|YTg4YjRk... (base64 string)|
|Token Endpoint|https://HOST/ccx/oauth2/TENANT/token|
|Private key file|private-key.pem (PEM format)|
|Workday REST API Endpoint Host|https://HOST/ccx/api/TENANT<p>You can use the View API Clients task in Workday to get this information. You’ll need the <HOST> value when you configure the JDBC URL.|

* Download [Python](https://www.python.org/downloads/) (v3.9 or above).
* [Download the Live Data Query Python wheel file](/docs/data-cloud/live-data-query/live-data-query-downloads-for-hackathon-2026.md) and save the file to a local directory.

# Context

Connect to Live Data Query using a Python script.

# Steps

1. Create the **dataservice.properties** file with the following configuration details using the **property=value** format. For example: **wd.authn.clientId=<ClientID>**

We recommend that you place the properties file and the connector wheel file in the same Python project directory before installation and execution.

|Option|Description|
| --- | --- |
|wd.authn.clientId|Client ID created in the Live Data Query setup.|
|wd.authn.isu|ISU created in the Live Data Query setup.|
|wd.authn.accessTokenEndpoint|Token Endpoint created in the Live Data Query setup.|
|wd.authn.privateKey|Privacy-Enhanced Mail (PEM) format text, must include the header/footer.<p>For information about the private key, see [Generating Public and Private Keys](/docs/data-cloud/live-data-query/generating-public-and-private-keys.md).|
|wd.host|Host URL<p>**Note**: The host is the <HOST> value from the Workday Rest API​ Endpoint created in the Live Data Query setup.|
|wd.port|443|

Note: If the client is behind a proxy server, then specify **wd.host={proxy host}** and **wd.authn.accessTokenEndpoint={proxy TokenEndpoint}**.

2. Install the Python library ([wheel file](/docs/data-cloud/live-data-query/live-data-query-downloads-for-hackathon-2026.md)) using following command:
```
pip3 install <wheel.whl>
```
3. Create a Python script that loads the configuration from the dataservice.properties file, creates connection to the service, and executes the query.

Example:
```
from workday_dataservice import DataServiceConfig, create_connection

# Load configuration from current directory
config = DataServiceConfig.from_file("dataservice.properties")

# Create connection
connection = create_connection(config)

# Execute the Query
cursor = connection.cursor()
cursor.execute("SELECT * FROM {tablename} LIMIT 10")
results = cursor.fetchall()
cursor.close()
connection.close()
```
# Next Steps

* Run [Live Data Query Sample Queries](/docs/data-cloud/live-data-query/live-data-query-sample-queries.md)