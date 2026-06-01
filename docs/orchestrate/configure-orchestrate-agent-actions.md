## Overview


We have developed Agent Actions for Workday Orchestrate, a new capability that lets you expose your synchronous orchestrations as AI-callable tools through the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/). Any MCP-compatible External AI client — Claude Code, Cusor, or your own custom agent built with Developer Agent, will be able to discover and invoke your orchestrations through natural language.

Rather than AI agents navigating across hundreds of APIs or low-level actions, they will directly invoke the deterministic business logic and system connectivity you’ve already built in Orchestrate. A single orchestration configured as an Agent Action becomes usable by every AI assistant in your organisation.

## How It Works
### Prerequisites

Deploy an Extend application containing a Synchronous orchestration.

Agent actions use the same authentication method as the Orchestrate Launch API. If you've already configured API client authentication for launching orchestrations, you can use the same Bearer token for MCP requests.

If not, follow these steps to obtain a new bearer token:

1. [Create Your API Client](https://developer.workday.com/documentation/zwx1518028675482). Save the Client ID and Client Secret.
2. Register the API Client for Integrations in your Workday tenant. Select the appropriate scopes for your orchestrations.
3. [Request a New Access Token with a Refresh Token](https://developer.workday.com/documentation/cqa1553122177664) to obtain a Bearer token for MCP requests.

> **Note:** Access tokens expire after approximately 1 hour. MCP clients must refresh tokens before expiry to maintain connectivity.

### Context

You can configure a Synchronous orchestration to make it discoverable and invokable by a Model Context Protocol-compatible AI agent using natural language queries.

### Steps

1. On the orchestration's **Start** step, click **Configure Request**.
2. On the **Agent Actions** tab, select the **Enable as Agent Action** checkbox.
3. Provide an **Action Name** and **Action Description** that describe the orchestration's functionality to the AI agent. The description should explain not just what the tool does but also when it should be used.
  * *Example:* 'Makes an ad-hoc percentage adjustment to an employee's base pay. Use this action when there is a request to increase or decrease an employee's base compensation by a percentage.'
4. In the **Action Annotations** section, select an effect level for the orchestration so the AI agent can understand its potential impact.The options are as follows:
  * *Destructive*: Indicates that the action can modify or delete data. AI agents may prompt the user for confirmation before executing.
  * *Idempotent*: Indicates that the action can be safely retried without unintended effects. Calling it multiple times produces the same result as calling it once.
  * *Read-Only*: Indicates that the action can read data but can't modify or delete it. AI agents can call this action without user confirmation.
> **Note:** Select only 1 annotation. *Destructive* and *Read-Only* are mutually exclusive.

5. In the **Request Properties** section, Orchestration Builder lists any input parameters defined on the **Start** step's **Requests** tab. Provide a description for each parameter, including the expected format and a sample value. Don't leave any descriptions empty. Empty descriptions significantly degrade AI agent tool selection and parameter handling.
  * *Example:* 'The percentage changes as a decimal string. A 10% increase is 0.1, a 5% decrease is -0.05.'
> **Note:** Only flat (non-nested) JSON schemas are currently supported. All parameters must be top-level properties.

6. In the **Headers** section, define any custom HTTP headers to be forwarded to the orchestration at runtime. These allow AI clients to pass contextual metadata alongside the tool arguments. Remove any unused header rows before saving. Empty rows can produce invalid tool schemas that prevent MCP clients from loading tools.
7. In the **Query Parameters** section, define any query parameters to be forwarded to the orchestration at runtime. Remove any unused query parameter rows before saving. Empty rows can produce invalid tool schemas that prevent MCP clients from loading tools.
8. On the orchestration's **End** step, click **Configure Response**. In the **Request Properties** section, Orchestration Builder lists any output parameters defined on the **End** step's **Requests** tab. Provide a description for each output parameter so the AI agent can interpret the response.

### Result

After deployment, the orchestration is available as an MCP tool at the following endpoint:

* `POST https://api.eu.wcp.workday.com/orchestrate/v1/apps/agentactionsfororchestrate_cw10_hswtbn/mcp`

MCP clients must include the following headers on every request:

* `Authorization: Bearer`
* `Content-Type: application/json`
* `Accept: application/json, text/event-stream`

The orchestration appears in the tool catalogue returned by the `tools/list` MCP method. Any MCP-compatible AI client connected to the endpoint can discover and invoke it through natural language.

### Examples

**Claude Code:**

`Bashclaude mcp add --transport http workday-orchestrate   
https://api.[region].wcp.workday.com/orchestrate/v1/apps/[appId]/mcp --header "Authorization: Bearer [access-token]" --header "Accept: application/json, text/event-stream"`

**curl (for testing):**

`Bashcurl -X POST https://api.[region].wcp.workday.com/orchestrate/v1/apps/[appId]/mcp \
    -H "Content-Type: application/json" \
    -H "Accept: application/json, text/event-stream" \
    -H "Authorization: Bearer [access-token]" \
    -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'`