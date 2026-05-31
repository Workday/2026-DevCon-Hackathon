# Create SFTP Credentials

Describes how to create SFTP credentials for use with the SFTP Transport File component.

## Prerequisites

Create an app containing an Asynchronous, Integration, or Business Process orchestration. 

Use the **Create Integration CredStore** task on your tenant to create an SFTP CredStore that you can refer to from the orchestration's Settings:

* **App ID**: You app's id on the Workday Developer Site. 
* **External Reference ID**: Your choice of ID for reference in Orchestration Builder Settings.
* **Functional Feature Service Mapping**: Drax.
* **Integration CredStore Token Type**: Username Password. 
* **Token Data**: The SFTP server password.
* **Username**: The SFTP server username.


## Context

Orchestration Builder's **SFTP Transport File** component enables you to handle SFTP file operations, dynamically determining endpoints, filenames, and paths at runtime. You can create SFTP credentials for it in your orchestration's Settings.


## Steps

1. On your orchestration's **Settings** page, click **SFTP Auth**.
2. Provide a **Name** for the credential.
3. Under the Authentication Type heading, select either *Single* or *Dual* authentication.
   
   Single authentication requires a name and password combination.
   
   Dual authentication requires a name and password combination together with an SSH key.
   
   > **Note:** Only single authentication with name and password is supported at DevCon 2026.

4. Select *Single* authentication and use the **Authentication Reference** field to provide the Access Key ID you stored in the tenant Integration Credstore for your application ID. Optionally, provide a **Description**.
5. Click **Create**.


## Result

Your SFTP credentials are now available for use with the **SFTP Transport File** component.