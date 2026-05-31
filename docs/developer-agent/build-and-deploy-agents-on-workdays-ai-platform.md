# Build and Deploy Agents on Workday’s AI Platform

### Prerequisites

* [Create Extend Apps Using App Builder](https://developer.workday.com/documentation/xer1651695264256/CreateExtendAppsUsingAppBuilder).   
* To use orchestrations, configure orchestrate agent actions. See [Configure Orchestrate Agent Actions](/docs/orchestrate/configure-orchestrate-agent-actions.md). 
* Browse the MCP Explorer to see all available tools. See [Concept: MCP Explorer](/docs/agentic/mcp-explorer.md).
* [Connect to a Development Tenant](https://developer.workday.com/documentation/GUID-f9ad6bdf-f687-4263-9cd0-12bd2b0ebf54-enHYPHENus/ConnecttoaDevelopmentTenant). 

### Context 

You can build custom agents with a Workday-delivered language learning model (LLM) in App Builder to deploy on Workday’s AI platform. You can either use the Developer Agent or manually build the agent. 

When you use the Developer Agent, it suggests an agent that you can refine or improve. The proposed agent includes: 

* An agent definition (.agent file)   
* Skill files that defines the capabilities the agent can perform (.agentskills file)

When you manually build the agent, you can define your own .agent, .agentskills, and tools.

### Steps

1. Open your Extend app in the App Builder.   
2. In the Developer Agent prompt, enter a prompt to build the custom agent.   
   Example:  *Build me an agent that gets my worker info.*   
3. Review the suggested custom agent.   
4. (Optional) Prompt Developer Agent to edit the skills.   
   Example: *Add a skill to view my available time off.*  
5. (Optional) In the **Skills** component, toggle to **Code** view to manually edit the .agentskills file.  
6. Click **Apply Changes**.  
7. Click **Confirm**.  
8. Click **Save and Deploy** in the App Builder to deploy the agent in your tenant.  
9. Select **View Agent Registry** to activate the custom agent in Agent System of Record (ASOR).   
10. Select the custom agent from the Agent Registry.   
11. Click **Configure Agent**.  
12. From the **Status** column, make the custom agent available by enabling the toggle slider.  
13. Populate the **Available To** prompt with the security groups that you want to have access to the custom agent.  
14. Click **Activate**.

### Results

The custom agent can leverage Workday data to read, write, and take actions inside Workday on behalf of the user. 

The custom agent is automatically agent-to-agent (A2A) ready.

### Next Steps

* From the agent file, select the **Play** icon in the far right side bar in App Builder to preview and test the agent.  
* Select **Agent Traces** from the bottom of the App Builder to view agent execution traces.

# Reference: Custom Agent Tools

This table references the available custom agent model context protocol (MCP) tools.

| Tool | Description |
| :---- | :---- |
| Agent-Ready Tools MCP | Calls Workday APIs that are agent-ready. Includes LLM components.   |
| Orchestrate Actions MCP | Calls Orchestrations registered as Agent Actions as tools. Can include:  Pipedream Connectors Custom Processing logic Workday REST and SOAP API calls You can use this tool when your business has its own proprietary logic to leverage in the custom agent. |
| Workday Live Data Query (LDQ)  | Connects to the Data Cloud as a query tool for SQL generation.   |
| Agent-to-User Interface (A2UI) | Identifies the pattern for the agent to use when rendering UI elements in the Workday Chat Panel.  The .agentskill describes the UI.  |