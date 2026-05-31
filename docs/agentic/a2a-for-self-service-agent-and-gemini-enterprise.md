# Configure A2A for Self-Service Agent and Gemini Enterprise

## Prerequisites

A Gemini Enterprise account

Access to Google Cloud Console and Gemini Enterprise:

* You must be able to open Gemini Enterprise, select a project and app, and reach the Gemini Agents tab.

Security: These domains in the Agent System of Record functional area in Workday:

* *Agent Compliance*
* *Agent Management Hub*
* *Manage: Agents*
* *Reports: Agent Reporting*
* *Setup: Agents*

Security: *Security Configuration* domain in the System functional area.

## Steps
1. Access **Agent Management Hub** in Workday.

a. On the **Agent Registry** tab, verify that the Self-Service Agent displays in the registry.
  b. Click **Self-Service Agent > Configure Agent**.
  c. Configure at least 1 skill on the agent and set the **Available To** column to your target security group. Examples: *All Employees* or *All Developers*. Save your configuration.
  d. Activate the **Self-Service Agent**.

2. Access the **Register API Client for Agents** task. As you complete the task, consider:

  * **Client Name**: Gemini Enterprise Self-Service
  * **OAuth 2.0 Redirect URI**: Use the Gemini Enterprise OAuth redirect URL provided by Google (e.g. the Vertex/Gemini redirect).
  * **Registered Agent**: Delegate group for Self-Service Agent.
  * **Grant Type**: Authorization Code Grant
  * **Token type**: Bearer

3. Save and record:

  * **Client ID**
  * **Client Secret**

This ensures the agent is callable from Agent Gateway / A2A. You will paste these into Gemini later.

4. Retrieve the agent card from Agent Gateway. This is the metadata that Gemini needs so it can talk to the Workday agent.

  a. Access the **Create Integration System User** task.

*  Security: *Integration Security* domain in the Integration functional area.
*  If you’re using existing ISUs, you can skip this step.
*  For each ISU needed for your use case, complete the task:

|Option|Description|
| --- | --- |
|User Name|Enter a unique ISU user name.|
|New Password and New Password Verify|Set a password.|
|Require New Password at Next Sign In|Don’t select the check box.|
|Session Timeout Minutes|Keep the value at zero to prevent the integration system user session from expiring.|
|Do Not Allow UI Sessions|Select the check box.|

  b. Access the **Register API Client for Integrations** task.

Security: *Security Configuration* domain in the System functional area.

As you complete the ask, consider:
    * **Scope (Functional Area)**: Agent System of Record
    * **Include Workday Owned Scopes**: Select this check box.

Take note of the **Client ID** and **Client Secret**.

  c. On the related actions menu of the client you created:

* Click **Client > Manage Refresh Tokens**.

* On the **Workday Account** field, select the ISU you created in the previous step.

* Generate a new refresh token.

* Take note of the new refresh token.

 d. Get the bearer token for the ISU using the refresh token, client ID and client secret.

**Note**
You can use an existing API Client, as long as it:
    i. Includes Agent System of Record functional area.
    ii. Has the **Include Workday Owned Scopes** checkbox checked.
    iii. Optional: It is leveraged through a system user, so the Agent Card doesn’t have any user context filtering.

`# VariablesTENANT_ALIAS - <tenant><environment> (i.e. supern-il0dh38dja90abe)REFRESH_TOKEN - retrieved from step 4CLIENT_ID - retrieved during client setup step 3CLIENT_SECRET - retrieved during client setup step 3# Request token via refresh tokencurl -X POST "https://us.agent.workday.com/auth/oauth2/${TENANT_ALIAS}/token" \     -H "Content-Type: application/x-www-form-urlencoded" \     -d "grant_type=refresh_token" \     -d "refresh_token=${REFRESH_TOKEN}" \     -d "client_id=${CLIENT_ID}" \     -d "client_secret=${CLIENT_SECRET}" `

  e. Make the Agent Card request listed in next section using the ISU bearer token. This will return the full Agent Card for registration.

5. Construct the Agent Card endpoint call:

* GET https://<agent-gateway-host>/v1/a2a/<tenant-alias>/self-service-agent/.well-known/agent-card.json*

* Call this endpoint with a valid bearer token (service or user) that can reach Agent Gateway.

* Copy the JSON response (the agent card).

Sample Agent Card

`# VariablesAGENT_GATEWAY_HOST (i.e. us.agent.workday.com)TENANT_ALIASAGENT_NAMEBEARER_TOKEN - ISU service token for generic card, agent user token for user-based card# Requestcurl -X GET "https://${AGENT_GATEWAY_HOST}/v1/a2a/${TENANT_ALIAS}/${AGENT_NAME}/.well-known/agent-card.json" \     -H "Authorization: Bearer ${BEARER_TOKEN}" \     -H "Accept-Encoding: identity" \     -H "Content-Type: application/json" `

6. Configure Gemini Enterprise to use the Workday agent:
  a. Open **Gemini Enterprise** in the target project and app.
  b. In the left panel, go to **Agents**.
  c. Click **Add agent** and select **Custom agent via A2A**.
  d. When prompted for the agent card:
    * Paste the JSON you copied in the previous step.
    * Click **Preview Agent Card**, then **Next**.

  e. When prompted for OAuth / auth configuration:
    * **Client ID**: the ID from the Workday A2A client from the previous step.
    * **Client Secret**: the secret from the previous step.
    * **Auth URL**: https://<agent-gateway-host>/auth/authorize/<tenant-alias>?response_type=code
    * **Token URL**: https://<agent-gateway-host>/auth/oauth2/<tenant-alias>/token
    * Remove scopes (allow all)
7. Grant end‑user access in Gemini:
  a. In Gemini, open the **Agents** tab.
  b. Open the Workday A2A agent you created in the previous step.
  c. Access the **User permissions** tab.
  d. Click **Add User**.
  e. Add the target users and groups. Example: **All users**.
  f. Assign role as **Agent User**.

This determines who in your org can chat with the Workday agent via Gemini Enterprise.

8. Use Workday Self-Service Agent in Gemini Enterprise:
  a. As a regular end‑user (not an admin), open the Gemini app where the agent is located.
  b. Select the Workday Self-Service agent in **Agents** tab - it should display the name and description from the agent card.
  c. Start a chat.
  d. When prompted to authorize to Workday, sign in and grant access for Gemini * Enterprise to invoke Workday Self-Service agent.

* The responses in Gemini Enterprise will now come from Workday Self-Service agent.

* Example queries/requests:
  * Show me information about [insert employee name].
  * Help me submit a time off request.
  * Show me my goals.
  * Show me my benefits elections.