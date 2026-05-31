## Prerequisites

* Workday tenant and credentials provided by the Hackathon facilitators.

## Context

Register an API Client in Workday for a user.user. You’ll need the API Client to set up DBeaver with Authorization Code Grant (user.user) flow. With this flow, DBeaver opens your browser so you can log in interactively with your Workday credentials — no private key or service account required.

## Steps

1. Sign in the Workday tenant.
2. Access the **Register API Client** task.
Security: *Set Up: Tenant Setup - Security* and *Security Administration* domains in the System functional area.

3. Complete the task:

|Option|Description|
| --- | --- |
|Client Name|Enter a unique API client name.|
|Client Grant Type|Select Authorization Code Grant.|
|Access Token Type|Select Bearer.|
|Redirection URI|https://localhost:8888/callback|
|Refresh Token Timeout (in days)|Keep the default value.|
|Scope|Select any value. This field is required but not utilized by Live Data Query.|
|Include Workday Owned Scope|Select the check box.|

4. Click **OK**.
The **Register API Client** task displays the API Client information.

4. ​​​​​​Save the API Client information from the ​​​​​​**Register API Client​​​​** page. ​​​​
You’ll need this information to configure Workday connector properties in the partner platform:

|Information|Example or Details|
| --- | --- |
|Client ID|generated client ID|
|Client Secret|generated client secret|
|Token Endpoint|https://HOST/ccx/oauth2/TENANT/token|
|Authorization Endpoint|https://HOST/ccx/TENANT/authorize|
|Workday REST API Endpoint|https://HOST/ccx/api/TENANT <p>**Note**: You’ll need the <HOST> value when you configure the JDBC URL.|

## Next Step
* [Set Up Live Data Query in Dbeaver for user.user](/docs/data-cloud/live-data-query/set-up-live-data-query-in-dbeaver-for-user-user.md)