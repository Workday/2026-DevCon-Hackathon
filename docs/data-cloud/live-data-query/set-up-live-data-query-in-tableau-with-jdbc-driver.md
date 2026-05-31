## Prerequisites

* [Register API Client in Workday for ISU](/docs/data-cloud/live-data-query/register-api-client-in-workday-for-isu.md)

* Gather this information from the Workday tenant:

|Information|Example or Details|
| --- | --- |
|ISU username|isu_ldq_user|
|Client ID|YTg4YjRk... (base64 string)|
|Token Endpoint|https://HOST/ccx/oauth2/TENANT/token|
|Private key file|private-key.pem (PEM format)|

* Download [Tableau Free Trial](https://www.tableau.com/trial/tableau-software) on your local machine. 
**Note**: Tableau Next doesn’t work with Live Data Query.

* [Download the JDBC driver jar file](/docs/data-cloud/live-data-query/live-data-query-downloads-for-hackathon-2026.md) and save the file to a local directory.


## Context

Through a JDBC connection, Live Data Query enables SQL access from Tableau to Workday business objects.

In this setup procedure, you’ll download the relevant JDBC files and configure them according to your use case.

## Steps

1. Install the JDBC Driver in the Tableau Directory.
For Tableau Desktop, copy the JDBC driver JAR file to:
<ul><li>macOS: ~/Library/Tableau/Drivers/</li>
<li>Windows: C:\Program Files\Tableau\Drivers\</li>
</ul>
If the Drivers directory does not exist, create it.

2. (For Linux only) Set the file permissions.
Ensure Tableau has permission to read the driver file:
```
chmod 755 ~/Library/Tableau/Drivers/<driver>.jar
```
3. Create a **dataservice.properties** file with the following configuration details using the **property=value** format. For example: **wd.authn.clientId={ClientID}**.

|Option|Description|
| --- | --- |
|wd.authn.clientId|Client Id created in the Live Data Query setup.|
|wd.authn.isu|ISU created in the Live Data Query setup.|
|wd.authn.accessTokenEndpoint|Token Endpoint created in the Live Data Query setup.|
|wd.authn.privateKey|Privacy-Enhanced Mail (PEM) format text, must include the header/footer.<p>For information about the private key, see [Generating Public and Private Keys](/docs/data-cloud/live-data-query/generating-public-and-private-keys.md).|

**Note**: If the client is behind a proxy server, add the wd.http-proxy=<proxy-host> property in the dataservice.properties file.

4. Launch Tableau Desktop.
5. Configure the JDBC Connection in Tableau.
a. Open Tableau Desktop.
  b. Click **Connect → To a Server → Other Databases (JDBC)**
  c. Enter the following information:

|Option|Description|
| --- | --- |
|URL|jdbc:workday://<HOST>:443/workday_core? <p>**Note**: Replace <HOST> with the <HOST> value from the Workday Rest API​ Endpoint created in the Live Data Query setup.|
|Dialect|Leave the default|
|Username|Leave it blank|
|Password|Leave it blank|
|Properties File|Upload the dataservice.properties file using the Browse button|

6. Click Sign In to open the Application.

## Next Steps

* Run [Live Data Query Sample Queries](/docs/data-cloud/live-data-query/live-data-query-sample-queries.md).