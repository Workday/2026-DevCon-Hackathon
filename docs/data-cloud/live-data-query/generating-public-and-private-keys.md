Public or private keys are required when you configure Workday Live Data Query and client connector properties:

* The x509 public key is required when you set up Live Data Query in Workday, specifically when you register the API Client for interacting with Workday.
* The private key is required when you configure Workday connector properties in your client (such as Tableau and Snowflake).

Ensure that you keep the keys protected.

# Running the OpenSSL Command

To generate the public and private key, run the openssl command from the terminal of your choice.

To check if your machine has openssl, run this command and confirm it prints a version:
```
openssl —-version
```
If you’re using a Windows machine and you don’t have openssl, use the Windows Package Manager (winget) to install openssl. Example:
```
winget install -e --id ShiningLight.OpenSSL
```
The openssl command is pre-installed with the Mac OS.

This sample command generates the key in PEM format:
```
openssl req -x509 -newkey rsa:2048 -keyout private-key.pem -out public-key.cert -sha256 -days 365 -nodes
```
# Related Documentation
* [Register API Client in Workday for ISU](/docs/data-cloud/live-data-query/register-api-client-in-workday-for-isu.md)
* [Set Up Live Data Query in Tableau with JDBC Driver](/docs/data-cloud/live-data-query/set-up-live-data-query-in-tableau-with-jdbc-driver.md)
* [Set Up Live Data Query Using Python Client and Driver](/docs/data-cloud/live-data-query/set-up-live-data-query-using-python-client-and-driver.md)
* [Set Up Live Data Query in Dbeaver For ISU](/docs/data-cloud/live-data-query/set-up-live-data-query-in-dbeaver-for-isu.md)