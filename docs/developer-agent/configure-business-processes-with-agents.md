# Configure Business Processes with Agents

### Prerequisites

* Register Workday-built agents in the agent system of record. See [Register and Configure Workday-Built Agents](https://doc.workday.com/admin-guide/en-us/workday-ai/agents/agent-system-of-record/agent-management/manage-agents.html). 
* [Build and Deploy Agents in Workday's AI Platform](/docs/developer-agent/build-and-deploy-agents-on-workdays-ai-platform.md).

### Context

You can invoke agents within business processes by configuring inputs for these agents to use in business process definitions:

* Workday-built agents.
* Custom agents built and deployed with the Extend Developer Agent in App Builder.

### Steps

1. Access the **Configure Business Process Agent** task.
Security: The *Workday Extend* domain in the System functional area.
2. Complete the task:

|Option|Description|
| --- | --- |
|Agent Definition|Select the agent to configure business process inputs for.|
|Agent Skill|Select the skills for which you want to define the input.|
|Allowed on Business Process|Select the business process to enable the agent on.|

3. Add a row to the table to configure agent inputs.
4. Complete the **Agent Input Configurations** section:

|Option|Description|
| --- | --- |
|Text|Enter a text input to instruct the agent.  Example: *Create a mentorship event for the worker being onboarded. Define the purpose as “Consultation on Career Goals”. |
|Field|Select the report fields associated with the business process event. Example: *Event Target* or *Initiator*.|

   Because users can't interact with the agent, you must explicitly instruct the agent to perform the operations without confirmation from the user. Example: *Execute actions immediately; do not seek user confirmation.* 

5. Click **OK**.
6. [Edit Business Processes](https://doc.workday.com/admin-guide/en-us/manage-workday/business-processes/customize-business-processes/dan1370797384762.html).
Add the **Invoke Agent** service business process step to an allowed business process for the agent.

7. Select the **Configure Event Service** button displayed on the **Invoke Agent** service business process step.

   These domains in the the System functional area:
   * *Business Process Administration*
   * *Manage: Business Process Definitions*

8. Complete the **Configure Event Service** section:

|Option|Description|
| --- | --- |
|Agent Definition|Select the agent to configure on this business process.|
|Run As Security Group|For Delegation Mode Agent Skills, choose a security group that the agent can do the work on behalf of. If there are multiple users in the security group, Workday selects one user.|
|Agent Input|Select one of the agent input configurations.|

9. Click **OK**.

### Results
When the service step of the business process initiates, Workday verifies the agent is registered and active in the Agent System of Record and launches the agent before moving on to the next step.