# Microsoft Agent Sync Configuration

This guide describes how to set up the one-time administrative bootstrap required to synchronize agents from **Microsoft Copilot Studio (CPS)** and **Azure AI Foundry** into the Workday Agent System of Record (ASOR).

# Set Up Workday Agent System of Record

## Prerequisites
Security: *Security Configuration* domain in the System functional area. Required to configure the ASOR security domains.

This phase outlines how to set up Agent System of Record and Agent Management Hub in your Workday tenant in preparation for syncing your Microsoft and Workday agents.

## Steps
1. Log in to your Workday tenant with an account that has access to the *Security Configuration* domain in the System functional area.  
  
2. Access the **Maintain Functional Areas** task.  
  
Select the **Enabled** check box for the Agent System of Record functional area.  
  
Click **OK** and then **Done**.  
  
3. Access the **Activate Pending Security Policy Changes** task.  
* Enter a comment and click **OK**.  
* Select the Confirm check box and click **OK**.  
  
4. Access the Domain Security Policies for Functional Area report.  
* From the Functional Area prompt, select *Agent System of Record*.  
* Select Domain Security Policy > Enable from the related actions menu for these domains:  
  * *Agent Compliance*  
  * *Agent Management Hub*  
  * *Manage: Agents*  
  * *Reports: Agent Reporting*  
  * *Setup: Agents*  
Repeat this step for the *Reports: AI Agent Security* domain in the System functional area.  
  
5. Access the Maintain Permissions for Security Group task.  
* Select the *Maintain* operation.  
  
* From the Source Security Group prompt, select the unconstrained security group that you want to grant ASOR agent management access to.  
  
* You can only configure unconstrained groups on ASOR security policies.  
  
* On the Domain Security Policy Permissions tab, provide *View and Modify* permissions for these domain security policies:  
  * *Agent Compliance*: Access to reports and data required for agent compliance.  
  * *Agent Management Hub*: Access to the Agent Management Hub, which is the centralized interface for viewing and managing agents.  
  * *Manage Agents*: Access to tasks for managing agents including configuration, activation, and deactivation in ASOR.  
  * *Reports: Agent Reporting*: Access to view agent analytics, report data sources, and filters for agent-level reporting.  
  * *Reports: AI Agent Security*: Access to reports about agent security information, such as the View AI Agent User Audit Trail report.  
  * *Setup: Agents*: Access to setup tasks and activities for agents in ASOR.  
  
6. Access the Activate Pending Security Policy Changes task.  
* Enter a comment and click OK.  
* Select the Confirm check box and click OK.  
  
# Configure Microsoft Tenant
  
This phase must be completed by a Microsoft Administrator with Entra ID and Power Platform permissions.  

## Steps
1. Create a Service Principal in Entra ID.  
  a. Access the **Microsoft Entra admin center**:  
  
https://learn.microsoft.com/en-us/entra/fundamentals/entra-admin-center  
  
  b. Register a new **Service Principal** (App Registration) to represent the Workday Agent Sync integration.  
  
  c. Generate and save the **Client ID** and **Client Secret** to your clipboard for later use in Workday.  
  
2. Configure Power Platform (For Copilot Studio Agents).  
  a. Access the **Power Platform Admin Center**.  
  
https://admin.powerplatform.microsoft.com/home  
  
  b. Navigate to **Manage > Select Environment > Settings > User + Permissions > Application Users** .  
  c. Create a new **Application User**.  
  d. Map this user to the Service Principal that you created in the previous section.  
 e. For Copilot Studio Integration:  
  
* Assign the roles that you want to have access to Dataverse (ex. System Administrator).  
  
* PowerShell Authorization: Open PowerShell and run this command to grant the application user access to the agent's custom connectors. This step is required to connect the Service Principal and the PowerApps database.  
  
`# First, connect to your Power Platform accountAdd-PowerAppsAccount# Define variables$connectorName = "%2Fproviders%2FMicrosoft.PowerApps%2Fapis%2Fshared_staffing-20change-20job-5f37e4e62c5a1b3800-5f947f09f4586dacb3"$environmentId = "ad3c7726-1a23-e541-a1d1-0b4b68e34c1d"$userObjectId = "305cfb44-9301-4fed-8d9f-0b45a861dd6e"# Assign permissionsSet-AdminPowerAppConnectorRoleAssignment -ConnectorName $connectorName -EnvironmentName $environmentId -RoleName CanView -PrincipalType User -PrincipalObjectId $userObjectId `  
  
  f. For Azure Foundry Integration:  
  
Access **Foundry portal > Project Project Details > Parent Resource Users > Add User**.  
  
3. Gather Environment Metadata:  
  
Copy these identifiers to your clipboard for the Workday configuration  
  
  a. Azure Portal  
  
**Tenant ID**: Search for **Microsoft Entra ID > Overview > Tenant ID**.  
  
  b. Power Platform Admin Center (For Copilot Studio Integration)  
  
**Environment URL & Environment ID**: Click **Manage > Environment**.  
  
  c. Azure Foundry Platform (for Foundry Integration)  
  
Per project, click Foundry portalProject Project Details to retrieve:  
* **Resource ID**  
* **Endpoint**  

# Configure Agent Sync in ASOR

## Prerequisites
Security: *Setup: Agents* domain in the Agent System of Record functional area.  
  
This phase is completed by a Workday Administrator in the Workday tenant.  
  
## Steps
1. Initialize Microsoft Integration Setup:  
  a. Access the **Create Microsoft Agent Sync Configuration** task (secured to the *Setup: Agents* domain in the Agent System of Record functional area) in Workday.  
  b. Enter this data from phase 2:  
  
  i.  Create new **Azure Resource Groups** (For Foundry Integration):  
* Microsoft Azure Group Name  
* Microsoft Azure Group URL: **Foundry Project Endpoint**  
* Azure Resource Manager Path: **Foundry Project Resource ID**  
  
 ii. Create new **Copilot Studio Project Groups** (For Copilot Studio Integration):  
  * Environment Name  
  * Environment URL  
  * Environment ID  
  
  c. Create new **External Client CredStore for Sync**:  
*  Auth Scheme for External Client: OAuth 2.0 Client Credential  
*  Client ID: Service Principle Client ID  
*  Client Secret: Service Principle Client Secret  
*  Token Endpoint: https://login.microsoftonline.com/<TenantID>/oauth2/v2.0/token  
  
2. Click **OK** to persist the configuration.  
3. (Optional) Trigger the agent sync manually using the button on the page.  

# Verify and Activate Agent

## Prerequisites
Security: These domains in the Agent System of Record functional area:

* *Agent Compliance*
* *Agent Management Hub*
* *Manage: Agents*
* *Reports: Agent Reporting*
* *Setup: Agents*

This domain in the System functional area: *Security Configuration*

## Steps
1. Verify Agent Sync:  
  a. Access **Agent Management Hub** (secured to the *Agent Management Hub* domain in the Agent System of Record functional area).  
  b. Verify that your Microsoft agents display in the UI.  
  c. Review Agent Tools: Click into an agent to verify that its Microsoft tools (OpenAPI specs or Custom Connectors) have successfully mapped to **Workday Tools**.  
2. Configure and Activate:  
  a. Select an agent and click **Configure and Activate**.  
  b. **Security Handshake**: You will need to enter the agent’s redirect URI from the agent builder platform. Take note of the **Agent Service User (ASU)** credentials generated by Workday, as you will need to enter them at the end.  
  c. Finalize in Microsoft:  
  
    * **For CPS**: Edit the security settings of your Custom Connector and enter the Workday ASU credentials.  
    * **Test**: Use the CPS agent playground to invoke the agent and confirm it can successfully call the Workday API.  