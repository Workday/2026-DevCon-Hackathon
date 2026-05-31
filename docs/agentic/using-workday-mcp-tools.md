Workday exposes 300+ **MCP tools** — callable by any agent, on any platform, that has been granted access through ASOR. These are the same tools that power Workday Custom Agents built with Developer Agent.
This guide covers two paths:

* **Workday custom agents** (built with Dev Agent in App Builder) — tool discovery and registration are handled automatically when you Save & Deploy.

* **External platforms** (Claude, Cursor, Copilot Studio, Google Agentspace, and others) — you register your agent in ASOR via API, configure which tools it can call, and connect it to the Workday MCP server. This guide walks through that end to end.
 
## How It Works
Workday MCP is built on the standard JSON-RPC 2.0 spec. Your agent connects to a single endpoint and calls tools using the MCP protocol — the LLM handles tool selection and call syntax automatically.

**MCP endpoint**: https://us.agent.workday.com/mcp

Three concepts to understand before you start:
* **Agent** — the top-level entity representing a persona or functional domain (e.g. "Payroll Agent", "Recruiting Assistant"). This is what gets registered in ASOR and what administrators manage.
* **Skill** — a discrete capability the agent can perform. A single agent can have multiple skills. Skills are the unit that gets secured with permissions and activated for users.
* **Tool** — a concrete Workday Agent-Ready tool a skill calls to do its work. One skill can call multiple tools. In Delegate mode, a tool call is executed on behalf of the end user — the user's permissions apply, not a service account.
 
## Discovering Available Tools
Workday has 300+ Agent-Ready tools covering Workforce, Financials, Talent, Payroll, Recruiting, and more. Two ways to find what you need — use whichever fits your workflow.

**Option A — MCP Explorer (recommended starting point)**
Go to https://developer.workday.com/mcp-explorer. This is a browsable UI showing all Agent-Ready tools and Workday-built agents available for use. You can filter by functional area, search by name, and see descriptions without writing any code. Use this to explore what's available before moving to registration.

**Option B — Finder API (programmatic discovery)**
The Finder API lets you search available tools programmatically — useful when you want to query and filter tools as part of a script or build process.

**Endpoint**: GET https://us.agent.workday.com/asor/agentResourceSearch/v1

**Auth**: Bearer token authenticating to your Workday tenant. The same token from the Developer Site copy-token flow (Step 2 below) works, as does any standard tenant OAuth token for a user with the Setup: Agents domain.

**Query parameters**:
 
|**Parameter**|**Required**|**Values**|**Description**|
| --- | --- | --- | --- |
|searchString|Yes|Any text|Searches against tool names. Not case-sensitive, partial matches accepted.|
|toolType|No|AGENT_READY, AGENT|Filter by tool category. See below.|
|operationType|No|CREATE, PATCH, VIEW|Filter by operation type.|
|protocol|No|MCP|Returns only tools accessible via the Workday MCP server.|

**toolType values:**
* AGENT_READY — Agent-Ready tools: Workday's MCP-enabled native tools. These are what you register in your agent definition.
* AGENT — Workday agents OR Custom Agents you build with Developer Agent. These can be called as tools by your agent.

**Example — find all Agent-Ready tools matching "time off":**
 

```
GET https://us.agent.workday.com/asor/agentResourceSearch/v1
  ?searchString=time off
  &toolType=AGENT_READY
  &protocol=MCP
```

 
Example response:

```
 
[
  {
	"type": "AGENT_READY",
	"name": "getTimeOffEntries",
	"description": "Returns time off entries for a worker.",
	"wid": "386c0e439ecd100039e864720d570000",
	"mcpEnabled": true,
	"httpEnabled": false
  }
]
```
The wid in each result is exactly what you pass into agent_resource.id in your agent registration (Step 3 below).
Example — discover available Workday agents as callable tools:

```
GET https://us.agent.workday.com/asor/agentResourceSearch/v1
  ?searchString=self service
  &toolType=AGENT
  &protocol=MCP
```

**Note**: If a tool you need isn't showing up in either the MCP Explorer or the Finder API, check the ASOR API documentation on GitHub or contact your Workday rep.

 
## Step 1 — Configure Your Tenant
This is a one-time setup. You don't need to repeat it for every agent you register.

**1.1 — Enable Agent System of Record (ASOR)**
ASOR is the control plane for all agents in your tenant — it determines which tools each agent can call and which users it can act on behalf of.
1.  Run the **Maintain Functional Areas** task.
2.  Filter on "Agent System of Record", check the **Enabled** column, and click **OK → Done**.
3.  Run **Activate Pending Security Policy Changes** — add a comment, check **Confirm**, click **OK**.
4.  For each of the four domains — Agent Management Hub, Manage: Agents, Reports: Agent Reporting, Setup: Agents — do the following:
* Run **View Domain: {domain}**.
* Click the magnifying glass for **Domain Security Policy**.
* Go to **Related Actions → Domain Security Policy → Enable**, then click **Confirm**.
* Go to **Related Actions → Domain Security Policy → Edit Permissions**.
* Add your agent-managing security group (e.g. HR Administrator) to **Report/Task Permissions** with **Modify**.
* Add the same group to **Integration Permissions** with **Put**. *(Required for the registration API call.)*
5.  Run **Activate Pending Security Policy Changes** again to apply all domain changes.

To verify: search for **Agent Management Hub** in Workday — if it appears, ASOR is enabled correctly.
 
**Note**: ASOR currently only supports Unconstrained security groups.

## Step 2 — Get an Access Token for Registration
You need a Bearer token to call the ASOR registration API. The method depends on whether you're a Workday Extend or Orchestrate customer.

**If you're a Workday Extend or Orchestrate customer (recommended)**
1.  Navigate to https://developer.workday.com and sign in.
2.  Click the **Manage Tenant Connections** icon in the top-right menu.
3.  Under **App Builder**, select the appropriate tenant from the dropdown and click **Connect**.
4.  Sign in with a user who has the Manage: Agents domain permission.
5.  Click **Copy Token**.
You now have a valid Bearer token to call the registration API.

**If you're not an Extend or Orchestrate customer**
1.  Run the **Register API Client** task in your tenant and fill in:
a.  **Client Grant Type**: Authorization Code Grant
b.  **Access Token Type**: Bearer
c.  **Redirection URI**: any valid URL (e.g. https://www.google.com)
d.  **Scope (Functional Areas)**: Agent System of Record
e.  **Non-Expiring Refresh Tokens**: checked
f.  **Include Workday Owned Scope**: checked

2.  Record the **Client ID, Client Secret, Token Endpoint**, and **Authorization Endpoint**. The secret cannot be retrieved later.

3.  Paste the following URL into a browser to get an authorization code (appended to your redirect URI):

```
https://us.agent.workday.com/auth/authorize/<tenant_name>
  ?response_type=code&client_id=<clientID>
  &state=test3pagent1&redirect_uri=https://www.google.com
```
 
4.  Use that code to get your Bearer token:
 
```
POST https://<tenantHost>/ccx/oauth2/<tenantName>/token
Body: grant_type=authorization_code&code=<authCode>
      &client_id=<clientID>&client_secret=<clientSecret>
```

## Step 3 — Register Your Agent in ASOR
Build the JSON body that describes your agent, then POST it to the registration endpoint.

**Endpoint**: POST https://us.agent.workday.com/asor/v1/agentDefinition

**Required headers:**
```
Authorization: Bearer <your token>
wd-agent-tenant-alias: <your tenant name>
Content-Type: application/json
```
 
**Agent definition structure:**
```
{
  "name": "MCP Staffing Agent",
  "description": "A Workday agent for staffing and workforce operations",
  "provider": { "id": "Provider=SELF-BUILT" },
  "url": "https://agent.workday.com",
  "platform": { "id": "Platform=OTHER" },
  "skills": [{
	"id": "staffing_1",
	"name": "Staffing",
	"description": "Handles terminations and worker info retrieval.",
	"inputModes": [{ "type": "application/json" }],
	"outputModes": [{ "type": "application/json" }]
  }],
  "workdayConfig": [{
	"skillId": "staffing_1",
	"executionMode": { "id": "Mode=Delegate" },
	"workdayResources": [
  	{
    	"tool_name": "getWorkers",
    	"description": "Get Workers",
    	"agent_resource": { "id": "94914bb1185d10000de91f2013e70032" }
  	},
  	{
    	"tool_name": "terminateEmployee",
    	"description": "Terminate Employee",
    	"agent_resource": { "id": "05b4bf58adc31000117fcd2ffe400000" }
  	}
	]
  }],
  "capabilities": {
	"stateTransitionHistory": false,
	"streaming": false,
	"pushNotifications": false
  },
  "version": "v1"
}
```

* **Finding tool WIDs**: use the **MCP Explorer** at https://developer.workday.com/mcp-explorer or the **Finder API** (see Discovering Available Tools above). The wid from either is what goes into agent_resource.id.

* **Finding your provider ID**: run **View Agent Providers** in your tenant. For a self-built agent, use Provider=SELF-BUILT.

* **Finding your platform ID**: run **View Agent Platforms** in your tenant. Example values: Microsoft Copilot Studio, Amazon Bedrock AgentCore, Google Agentspace. Use Platform=OTHER if yours isn't listed.

A successful POST returns a 201 with the agent definition including its assigned WID. Navigate to **Agent Management Hub → Agent Registry** in your tenant — your agent should appear with status **Inactive**.
 
**Updating an agent**: repost to the same endpoint with the same name value and it will update in place rather than create a new registration.

## Step 4 — Configure and Activate Your Agent
1.  In your tenant, navigate to **Agent Management Hub → Agent Registry**.
2.  Click your agent, then click **Configure Agent**.
3.  For each skill, toggle **Status** to enabled and add the security group(s) whose users can invoke it in the **Available To** column. Click **Save**.
4.  Enter a **Redirect URI** when prompted and click **OK**.
5.  **Save your OAuth credentials immediately** — this screen will not be shown again:
* Agent OAuth Authorize Endpoint URL
* Agent OAuth Token Endpoint URL
* OAuth 2.0 Client ID
* OAuth 2.0 Client Secret
6.  Click **Activate Agent → Activate**.

Your agent should now show **Status: Active** in the Agent Registry. These credentials are what your agent uses at runtime to call MCP tools on behalf of users.
 
**Note**: The security groups in **Available To** must also have permissions to the domain or business process security policies that the tools access. Use the **View Security for Agent Skill** report to check.

## Step 5 — Connect to the MCP Server
With your agent active and OAuth credentials in hand, you're ready to call the MCP endpoint.

**Authentication at runtime**
MCP calls use the ASU OAuth flow. Your agent must obtain an access token before calling Workday tools:

1.  **Get an authorization code** — call the Authorize endpoint, which opens a login window for the end user. After the user logs in, the authorization code is appended to your redirect URI:

```
GET https://us.agent.workday.com/auth/authorize/<tenant>
Params: client_id, redirect_uri, response_type=code
```
2.  Exchange for an access token:

```
POST https://us.agent.workday.com/auth/oauth2/<tenant>/token
Headers: Content-Type: application/x-www-form-urlencoded
Body: grant_type=authorization_code&code=<code>
      &client_id=<id>&client_secret=<secret>
 
Response: { "access_token": "...", "token_type": "Bearer", "refresh_token": "..." }
```

3.  **Refresh before expiry** — access tokens expire after 60 minutes, refresh tokens after 24 hours:
 
```
POST https://us.agent.workday.com/auth/oauth2/<tenant>/token
Body: grant_type=refresh_token&refresh_token=<token>
```

**Note**: Most agent platforms and API clients (Bruno, Postman) handle the OAuth 2.0 flow natively — you shouldn't need to hand-code the auth steps above.

**Verify your connection — list your agent's tools**
Before wiring this into your platform, confirm the tools are accessible:

```
curl --request POST \
  --url https://us.agent.workday.com/mcp \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Bearer <ASU token>' \
  --data '{
	"jsonrpc": "2.0",
	"id": 1,
	"method": "tools/list"
  }'
```
A 200 response with a list of your registered tools confirms everything is wired correctly.
 
## Step 6 — Connect Your Agent Platform

**Cursor**
Cursor has built-in MCP support and is a quick way to test your tools without building a full agent.
1.  Get a fresh ASU access token (from Bruno or your API client).
2.  Open Cursor **Settings → Cursor Settings → Add Custom MCP**.
3.  In mcp.json, add:
```
{
  "mcpServers": {
	"Workday MCP": {
  	"url": "https://us.agent.workday.com/mcp",
  	"headers": {
    	"Authorization": "Bearer <your ASU token>"
  	}
	}
  }
}
```
4.  Enable the toggle. A tool count should appear when connected successfully.
5.  In the Cursor chat panel, prompt naturally — for example: Change Anthony Rizzo's title to Chief Happiness Officer.

Cursor will invoke the appropriate Workday tool. You can verify the result in your tenant, including the audit trail showing the agent acting in delegate mode on the user's behalf.

**Other platforms**
Any platform that supports MCP (Google Agentspace, Amazon Bedrock AgentCore, Microsoft Copilot Studio, and others) connects to the same endpoint using the same OAuth credentials. Refer to your platform's MCP integration documentation for the specific configuration steps — the Workday side is the same regardless of which platform you use.
 
## Developer Notes
* **Updating your agent definition** — POST to the same endpoint with the same name value. If the name matches an existing registration, it updates in place. If it doesn't match, a new agent is created.
* **Re-registering existing agents** — if you registered an agent before a recent ASOR update, you'll need to re-register it for the new functionality to take effect.
* **Access token expiry** — 60 minutes. Refresh token expiry — 24 hours. When the access token expires, use the refresh token to get a new one without requiring the user to re-authenticate.
* **Delegate mode** — in delegate mode, tool calls are executed under the end user's identity. The user must have permission to the underlying Workday data; the agent cannot bypass their security. Use View Security for Agent Skill in your tenant to validate access.