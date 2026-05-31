## Prerequisites

* [Register API Client in Workday for user.user](/docs/data-cloud/live-data-query/register-api-client-in-workday-for-user-user.md)
* Download the latest Dbeaver client from [DBeaver](https://dbeaver.io/download/).
* [Download the JDBC driver jar file](/docs/data-cloud/live-data-query/live-data-query-downloads-for-hackathon-2026.md) and save the file to a local directory.

## Context

Through a JDBC connection, Live Data Query enables SQL access from Dbeaver to Workday business objects using the OAuth 2.0 Authorization Code Grant flow. With this flow, DBeaver opens your browser so you can log in interactively with your Workday credentials — no private key or service account required.

In this setup procedure, you’ll:

* Add driver to Driver Manager.
* Import a Driver JAR.
* Create Database Connection.
* Execute a Query.

## Steps

### Add Driver to Driver Manager

1. Open **DBeaver**.
2. Select **Database → Driver Manager**.
3. Click **New** to create a new driver.
4. Configure the below details in the **Settings** tab:

|Driver Name|Workday Data Service|
| --- | --- |
|Driver Type|Generic|
|Class Name|com.workday.dataservice.driver.api.DataServiceDriver|
|URL Template|Use the exact URL template below:<p>**jdbc:workday://{host}:{port}**|
|Default Port|443|

Leave all other fields as default.

### Import the Driver

5. In the Driver Manager window (still open from Step 2), click the **Libraries** tab.
6. Click **Add File** and upload the downloaded .jar file.
7. Click **Find Class** to verify **DataServiceDriver** is detected.
Ensure **com.workday.dataservice.driver.api.DataServiceDriver** appears in the driver class dropdown.
8. Click **OK** to save the driver configuration.

Create Database Connection

9. Select **Database → New Database Connection**.
10. Select **Workday Data Service** and click **Next**.
11. In the **Connection Settings**, select **Main**, and enter the following detail:

|JDBC URL|jdbc:workday://{HOST}:{PORT}?SSLVerification=NONE|
| --- | --- |
|**{HOST}**|Your SDE hostname. The <HOST> value from the displayed Workday Rest API​ Endpoint when you registered the API Client.|
|**{PORT}**|443 (Scylla)<p>8089 (SUV)|

Leave Username and Password blank — authentication is handled by OAuth.

12. On the Driver Properties tab, add these properties:

|wd.authn.authModel|AUTHORIZATION_CODE|
| --- | --- |
|wd.authn.clientId|generated client ID|
|wd.authn.clientSecret|generated client secret|
|wd.authn.accessTokenEndpoint|https://HOST/ccx/oauth2/TENANT/token|
|wd.authn.authorizationEndpoint|https://HOST/ccx/oauth2/TENANT/authorize|
|wd.authn.redirectUrl|https://localhost:8888/callback|

13. Click **Test Connection** to verify the configuration.
A successful test results in a Connected message. When there are errors, verify that the driver properties and connection settings are configured correctly.
14. Click **Finish** to save the connection.

### Connect

15. Double-click the new connection (or right-click → **Connect**).
16. DBeaver will trigger the OAuth flow:
a. Your default browser opens automatically at the Workday login page.
b. Log in with your Workday credentials and approve access.
c. The browser redirects to https://localhost:8888/callback.

17. A browser security warning may appear because the callback server uses a self-signed certificate. Accept the warning to proceed (this is expected — the certificate is local to your machine).
**Note**: You have 5 minutes to complete the browser login. If you exceed this, the connection attempt fails and you can retry.

18. Once the browser shows a success message, return to DBeaver. The connection completes automatically.

## Next Steps

* Run [Live Data Query Sample Queries](/docs/data-cloud/live-data-query/live-data-query-sample-queries.md)