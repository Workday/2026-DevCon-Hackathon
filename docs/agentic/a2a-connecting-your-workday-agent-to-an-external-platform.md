# How It Works

Agent-to-Agent (A2A) enables an external AI platform — like Google Gemini Enterprise or Microsoft Copilot Studio — call your Workday Custom Agent or any Workday-built agent as a delegate. The end user chats in their external platform, and your Custom Agent built on the Workday AI platform handles the request.

**This guide covers the generic setup. See the platform-specific notes at the end for Gemini Enterprise and Copilot Studio.**

The flow:

1. **Discover** — External platform fetches the agent card (GET /.well-known/agent-card.json) to learn how to talk to your Custom Agent
2. **Agent Card returned** — A2A Server responds with the Custom Agent’s capabilities, endpoints, and auth requirements
3. **Invoke** — External platform sends the user's message as an A2A request (with a bearer token)
4. **Route** — Agent Gateway authenticates the request via ASOR and hands it to Self-Service Agent.
5. **Respond** — Self-Service agent processes the task and returns a natural language response back through the chain

# Prerequisites

**In Workday**

* A Workday tenant with Agent System of Record (ASOR) enabled.

* The Self-Service Agent registered in Workday ASOR, active, with at least one skill and a security group assigned (e.g. All Employees, or All Users)

* A System Admin account to run Register API Client for agents.

**In your external platform**

* Ensure your Self-Service Agent is callable from Gemini, Copilot Studio, and other A2A-compatible platforms

* An account with permission to create and configure agents

* Ability to paste a JSON agent card and configure OAuth 2.0 (client ID, client secret, auth URL, token URL).

**Note:**
If you don't have Self-Service agent set up yet? See the **Activate Self-Service Agent in ASOR** section below. If it's already active in ASOR, skip to Section 4.

# Activate Self-Service Agent in ASOR (if not already done)

1. Sign in as an ASOR admin and open **Agent Management Hub**.
2. In **Agent Registry**, confirm the Workday Self-Service Agent exists
3. Click the agent → **Configure Agent**
4. Set **Available To** to the appropriate security group (e.g. All Employees or All Users).
5. Click **Activate**.

Once active, the agent is callable via Agent Gateway and A2A.

# Create an A2A OAuth Client in Workday

This client is what the external platform uses to authenticate users and call your agent.

1. Sign in as System Admin in your Workday tenant
2. Run the **Register API Client for agents** task
3. Configure the client:
  a. **Grant type**: Authorization Code
  b. **Access token type**: Bearer
  c. **Redirect URL**: use the OAuth redirect URL from your external platform (see the **Test Your Integration** section). If you don't have it yet, use a placeholder and update it later.
  d. **Scope / agent selection**: select the delegate agent you want this client to access (one client per agent)
e. Save and record:
   * Client ID
   * Client Secret
   * Agent OAuth Authorize Endpoint URL
   * Agent OAuth Token Endpoint URL
   * Agent Gateway Host URL

You'll paste these into your external platform when you configure it.

# Get the Agent Card

The agent card is a JSON document that tells the external platform how to talk to your Workday agent — its endpoints, capabilities, and auth requirements.

## Get a Bearer Token from the Developer Site

The easiest way to get a token for testing is directly from the Workday Developer site:
1. Go to https://developer.workday.com and open your app in App Builder
2. Click **Manage Tenant Connections** and connect to your tenant.
3. After successfully connecting, click the **Copy Token** link that appears next to your tenant
4. Save this token — you'll use it in the next step

## Fetch the Agent Card

Use the token from Section 5.1 to call the agent card endpoint:

```
BEARER_TOKEN="<token from step 5.1>"

TENANT_ALIAS="<tenant_alias>" # e.g. hackteam116_wcpdev1

AGENT_NAME="self-service-agent"

curl -X GET "https://us.agents.workday.com/v1/a2a/${TENANT_ALIAS}/${AGENT_NAME}/.well-known/agent-card.json" \

-H "Authorization: Bearer ${BEARER_TOKEN}" \

-H "Accept-Encoding: identity" \

-H "Content-Type: application/json"
TENANT ALIAS FORMAT 
```
**Example**:
If your tenant URL is https://wcpdev.wd101.myworkday.com/hack116_wcpdev1/d/home.htmld, your alias is hack116_wcpdev1.

Copy the full JSON response. This is your agent card.

# Configure the External Platform

The exact UI varies by platform, but the inputs are always the same.

**Agent metadata**

• Paste the agent card JSON from Section 5.2

OAuth / Authentication settings

|Field|Value|
| --- | --- |
|Client ID|from the **Create an A2A OAuth Client in Workday** section.|
|Client Secret|from the **Create an A2A OAuth Client in Workday** section.|
|Auth URL|https://us.agents.workday.com/auth/authorize/<tenant-alias>?response_type=code|
|Token URL|https://us.agents.workday.com/auth/oauth2/<tenant-alias>/token|
|Redirect URL|must match what you set on the A2A client in the **Create an A2A OAuth Client in Workday** section.|
|Scopes|leave empty / allow all|

At runtime, the platform sends users through this OAuth flow, stores the returned access token, and calls the A2A message endpoints from the agent card with the token as Authorization: Bearer <token>.

# Grant End-User Access

**In Workday**: Confirm that the end users you want to test with belong to the security group assigned to Self-Service agent in ASOR (Section 3).

**In your external platform**: Assign the Workday agent configuration to the appropriate users or groups. The exact steps depend on the platform — look for a "User permissions" or "Agent access" setting and assign an "Agent User" role (or equivalent).

# Test the Integration

As an end user (not admin):

1. Open the external platform where you configured the Workday agent
2. Select the Workday Self-Service Agent
3. Start a chat — you'll be redirected to Workday to authorize access
4. Sign in with your Workday credentials and grant access
5. Once authorized, your messages will be handled by Self-Service agent.

Example prompts to try:
* What is my time off balance?
* Show me my payment elections
* I want to take time off tomorrow
* Show me my goals

# Platform-Specific Notes

## Google Gemini Enterprise

1. Open Gemini Enterprise → go to Agents in the left panel.
2. Click **Add agent → Custom agent via A2A**.
3. Paste the agent card JSON → click **Preview Agent Card → Next**.
4. Fill in the OAuth fields from the Configure the External Platform section.
5. To grant access: go back to Agents → open the Workday agent → **User permissions** → **Add User** → assign role **Agent User**.

Reference documentation for Google Gemini Enterprise with A2A:
https://docs.cloud.google.com/gemini/enterprise/docs/register-and-manage-an-a2a-agent

## Microsoft Copilot Studio

1. Open Copilot Studio — in the top toolbar, go to **Agents**.
2. Click **Add — Connect to an external agent — Agent2Agent**.
3. For **Agent endpoint URL**, use:

  a. https://us.agents.workday.com/v1/a2a/<tenant-alias>/self-service-agent

4. Set **Name** to Workday Self-Service Agent.
5. Fill in the OAuth fields from Section 6 (no scopes).
6. Click **Create** — a redirect URL is generated. Go back to your A2A client in Workday and update the Redirect URL field with this value.
7. To grant access: select **Create new connection** — sign in to Workday — confirm the green checkmark — **Add and configure**.

**Note: TROUBLESHOOTING — CONNECTION FAILS IN COPILOT STUDIO**

Try creating the connection via PowerApps instead:
• Go to **PowerApps → Connections → + New Connection**.
• Search for your agent name, select it, and sign in to Workday when prompted

## A2A Inspector

A2A Inspector is a lightweight A2A client that allows users to interact via A2A protocol without the need for access to a specific platform.

Use the OAuth Client created in Section 4.0 to retrieve a user token

1. Navigate to **View API Clients** task in your Workday environment
  a. Find the Agent OAuth client created
  b. Edit the redirect URL to a generic accessible site, such as https://www.google.com
2. Copy the Authorization Code from the browser URL
  a. e.g. https://www.google.com/?code=d8234uuu09derdqq04ti49rxl
3. Make a request to the **Agent OAuth Token Endpoint URL** to retrieve a bearer token
4. Navigate to the A2A Inspector
5. Paste the **Agent OAuth Authorize Endpoint URL** in a browser window

  a. You’ll be redirected to Workday to authorize
```
TENANT_ALIAS="<tenant_alias>" # e.g. hackteam116_wcpdev1

AUTH_CODE=code retrieved from authorization redirect

CLIENT_ID & CLIENT_SECRET=retrieved from OAuth Client

curl -X POST "https://us.agents.workday.com/auth/oauth2/${TENANT_ALIAS}/token" \

-H "Content-Type: text/plain" \

--data "grant_type=authorization_code&code=${AUTH_CODE}&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}"
```
Run the A2A Inspector

1. See official docs to run the client: https://github.com/a2aproject/a2a-inspector
2. For **Agent Card URL**, use:
  a. https://us.agents.workday.com/v1/a2a/<tenant-alias>/self-service-agent
3. Expand **Authentication & Headers** — select **Bearer Token**.
  a. Paste bearer token retrieved from steps above
4. Click **Connect**.
5. If everything was configured correctly, it will say “Agent Card is valid.”
6. You can now use the chat section to communicate with the agent