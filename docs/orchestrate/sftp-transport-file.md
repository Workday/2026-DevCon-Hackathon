## Purpose
Use this component to handle SFTP file operations, dynamically determining paths at runtime.

## Template Availability
This component is available in these orchestration templates:
* Integration
* Asynchronous
* Business Process

## Properties
Provide a meaningful **Reference Name** for the step as it will appear in your orchestration.

Select an **Operation** and use Expression Builder to specify the **Endpoint** where the files are currently located or should be delivered to. The component supports these SFTP operations:
* *Retrieve*: Fetch files from the endpoint.
* *Deliver*: Send files to the endpoint.
* *List*: Obtain a list of files at the endpoint.
* *Move*: (Not available at DevCon 2026.)
* *Delete*: Remove files from the endpoint.

You specify details about the files you're working with on the **Configuration** tab.

**The Auth Tab**

Use this tab to specify the type of SFTP authentication your request uses. You can create SFTP authentication types on the **SFTP Auth** tab in your orchestration's **Settings**. Types you create there are available from the drop-down menu here.

Select **Default Credential** to use the SFTP credential configured as default for this orchestration.

**The Configuration Tab**

The fields available here depend on the selected operation. This table summarizes them:

| Operation | Configuration Field Descriptions |
| :--- | :--- |
| Retrieve | * Directory Path: Specify the endpoint directory where the files are located.<br>* File Pattern: Specify the files you want to retrieve. |
| Deliver | * Directory Path: Specify the endpoint directory where the files should be delivered.<br>* Temporary Naming: Provide a temporary name for use during file transfer. When transfer is complete, the name is automatically changed to the Target File Name. This ensures that any automated systems awaiting a particular file name at the destination only pick up fully delivered files.<br><br>Click **Upload File** to add content for delivery, and complete these fields:<br>* Input Content: Specify the data that should be included in the file.<br>* Target File Name: Provide the name for the fully delivered file. |
| List | * Directory Path: Specify the endpoint directory where the files are located.<br>* File Pattern: Specify the files you want to list. |
| Move | (Not available at DevCon 2026.) |
| Delete | * Directory Path: Specify the endpoint directory where the files are located.<br>* File Pattern: Specify the files you want to list. |

**The Settings Tab**

Specify a Failure Strategy to be enacted when a file operation fails:
* *Exit on first failure*: Orchestrate immediately moves on to the next step in the flow if an operation fails.
* *Continue to attempt operation*: Orchestrate continues to retry the failed operation.