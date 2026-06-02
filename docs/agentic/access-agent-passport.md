# Access Agent Passport

## Prerequisites
Security: These domains in the Agent System of Record functional area:

-   _Agent Compliance_
-   _Agent Management Hub_
-   _Manage: Agents_
-   _Reports: Agent Reporting_
-   _Setup: Agents_

This domain in the System functional area:  _Security Configuration_

On the **Maintain Permissions for Security Group** task, configure  _View and Modify_  permissions for these domain security policies for the security group associated with the Workday account that you sign in with:

-   _Agent Compliance_: Access to reports and data required for agent compliance.
-   _Agent Management Hub_: Access to the **Agent Management Hub**, which is the centralized interface for viewing and managing agents.
-   _Manage Agents_: Access to tasks for managing agents including configuration, activation, and deactivation in ASOR.
-   _Reports: Agent Reporting_: Access to view agent analytics, report data sources, and filters for agent-level reporting.
-   _Reports: AI Agent Security_: Access to reports about agent security information, such as the **View AI Agent User Audit Trail** report.
-   _Setup: Agents_: Access to setup tasks and activities for agents in ASOR.

## Steps
1.  (Optional) [Register an external agent](https://doc.workday.com/admin-guide/en-us/workday-ai/agents/agent-system-of-record/external-agents/register-and-define-your-agent-through-an-api.html). Example: a [deepagent](https://docs.langchain.com/oss/python/deepagents/overview).

**Note**: If you have already registered an external agent in your tenant, you can skip to step 2.
    
 2. Access **Agent Management Hub**.  
    a. Click **Agent Registry**  to open the agent profile of the agent you imported.  
    b. Activate your agent.  
    c. Click the **Scan Agent** button. As the scan runs, the **Trust and Safety Passport** tab appears on your agent's profile and displays the stamps for your agent. When the scan completes, the **Scan Agent** button reappears and the stamp statuses detected by the scan display on the **Trust and Safety Passport** tab.  
    
3. Click the **Trust and Safety Passport** tab:  
    -   On the **Trust and Safety Passport** tab, you can view and edit the stamps that an agent needs to operate safely in Workday. It displays information related to the names, statuses and descriptions of the stamps.  

## Test Cases

**Scenario**: Activate an agent to display the **Trust and Safety Passport** tab.

**Context:** The ASOR **Trust and Safety Passport** tab should only display after you activate an agent.

**Steps:**  
1. Access **Agent Management Hub**.  
2. Select an inactive agent.  
3. View the agent profile of the inactive agent. The **Trust and Safety Passport** tab should not display.  
4. Activate the agent.  
a. For an external agent, ensure that the agent is available to *All Users*.  
5. Reopen the agent profile of the agent. The **Trust and Safety Passport** tab should now display.  
6. Browse the agent passport information on the agent.  
____
**Scenario**: Check the stamp status of an external agent.

**Context:** How to check the stamps for an external agent.

**Steps:**
1. Access **Agent Management Hub**.
2. Select an agent and open its agent profile.
3. Click the **Trust and Safety Passport** tab.
4. Check the status of these stamps:

|Stamp|Target Status|
| --- | --- |
|Runtime Guardrails|Passed|
|PII Exfiltration Resistance|Pending|
|System Prompt Confidentially verified|Pending|
|Output Safety filtering|Pending|
|Jailbreak resistance|Pending|
____

**Scenario**: Check Cisco scan status for an external agent.

**Context:** How to check the stamp status after completion of a CISCO scan.

**Steps:**
1. Access **Agent Management Hub**.
2. Select an agent and open its agent profile.
3. Click the **Trust and Safety Passport** tab.
4. Check the status of these stamps:

|Stamp|Target Status|
| --- | --- |
|Runtime Guardrails|Passed|
|PII Exfiltration Resistance|Either **Passed** or **Failed**. Should not be **Pending**|
|System Prompt Confidentially verified|Either **Passed** or **Failed**. Should not be **Pending**|
|Output Safety filtering|Either **Passed** or **Failed**. Should not be **Pending**|
|Jailbreak resistance|Either **Passed** or **Failed**. Should not be **Pending**|

## Troubleshooting: If a Stamp Fails

See: [Addressing Successful Attacks from AI Validation.pdf](https://github.com/user-attachments/files/28514453/Addressing.Successful.Attacks.from.AI.Validation.pdf)
