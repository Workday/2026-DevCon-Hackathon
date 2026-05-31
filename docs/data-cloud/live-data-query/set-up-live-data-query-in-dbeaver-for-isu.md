## Prerequisites

* [Register API Client in Workday for ISU](/docs/data-cloud/live-data-query/register-api-client-in-workday-for-isu.md).
* Download the latest Dbeaver client from [DBeaver](https://dbeaver.io/download/).
* [Download the JDBC driver jar file](/docs/data-cloud/live-data-query/live-data-query-downloads-for-hackathon-2026.md) and save the file to a local directory.

## Context

Through a JDBC connection, Live Data Query enables SQL access from Dbeaver to Workday business objects.

In this setup procedure, you’ll:

* Add driver to Driver Manager.
* Import a Driver JAR.
* Create Database Connection.
* Execute a Query.

## Steps

### **Add Driver to Driver Manager**

1. Open **DBeaver**.
2. Select **Database → Driver Manager**.
3. Click **New** to create a new driver.
4. Configure the below details in the Settings tab:

|Driver Name|Driver Name of your choice|
| --- | --- |
|Driver Type|Generic|
|Class Name|com.workday.dataservice.driver.api.DataServiceDriver|
|URL Template|Use the exact URL template below:<p>**jdbc:workday://{host}:{port}**|
|Default Port|443|

Leave all other fields as default.

### **Import the Driver**

5. In the Driver Manager window (still open from Step 2), click the **Libraries** tab.
6. Click **Add File** and upload the downloaded .jar file.
7. Click **Find Class** to verify the driver class is detected.
Ensure com.workday.dataservice.driver.api.DataServiceDriver appears in the driver class dropdown.
8. Click **OK** to save the driver configuration.

### **Create Database Connection**

9. Select Database → New Database Connection.
10. In the connection wizard, select the All tab.
11. Find and select the Driver you created in Step 3.
12. Click Next.
13. In the Connection Settings, select Main > Connect by URL, and enter the following detail:
JDBC URL:  **jdbc:workday://HOST:443**
**Note**: Replace HOST with the HOST value from the Workday Rest API​ Endpoint created in the Live Data Query setup.

14. On the Driver Properties tab, add these properties:

|wd.authn.clientId|Client Id created in the Live Data Query setup.|
| --- | --- |
|wd.authn.isu|ISU created in the Live Data Query setup.|
|wd.authn.accessTokenEndpoint|Token Endpoint created in the Live Data Query setup.|
|wd.authn.privateKey|Privacy-Enhanced Mail (PEM) format text. Must include the header/footer.<p>For information about the private key, see [Generating Public and Private Keys](/docs/data-cloud/live-data-query/generating-public-and-private-keys.md).<p>Note: The **wd.authn.privateKey** and **wd.authn.privateKeyFilePath** properties are mutually exclusive. If you specified **wd.authn.privateKey**, don’t specify **wd.authn.privateKeyFilePath**.|
|wd.authn.privateKeyFilePath|The full path of the private key file.

**Notes**:

* For a Windows machine, you must specify the **wd.authn.privateKeyFilePath** property. Example: C:\Users\user.name\Documents\<privateKeyFile.txt>.
* The **wd.authn.privateKeyFilePath** and **wd.authn.privateKey** properties are mutually exclusive. If you specified **wd.authn.privateKeyFilePath**, don’t specify **wd.authn.privateKey**.|

15. Click **Test Connection** to verify the configuration.
A successful test results in a Connected message. When there are errors, verify that the driver properties and connection settings are configured correctly.
16. Click **Finish** to save the connection.

### Execute a Query

17. In the **Database Navigator**, expand the new connection.
18. Right-click the connection and select **SQL Editor → New SQL Script**.
19. Enter the query in the SQL Editor and click **Execute** to run it.

## Result

Results display in the Results panel.

## Next Steps

* Run [Live Data Query Sample Queries](/docs/data-cloud/live-data-query/live-data-query-sample-queries.md)