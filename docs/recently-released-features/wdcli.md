# Product Update: Workday Developer CLI - Now Generally Available

The **Workday Developer Command Line Interface (WDCLI Version 1.7)** is now Generally Available (GA) for all Integration and Extend customers!

## What’s New 

WDCLI is your unified command line interface to build, test, validate, and deploy Workday Integration and Extend apps. It is designed to provide developers with a streamlined way to manage the full application lifecycle through the terminal and automated scripts.

## Key Capabilities

* Upload apps to App Hub
* Download apps from App Hub
* Deploy apps to tenants
* Promote apps to Implementation, Sandbox, and Production tenants.
* Set up a System User account on the Developer Site and enable WDCLI to authenticate programmatically with the Workday developer platform using a secure, non-human user account. 
  * For guidance on how to set up a System User account, reference Developer Site documentation: [Set Up System Accounts for WDCLI](https://developer.workday.com/documentation/GUID-3cf37356-a7dd-4e54-97a5-5bcc4feca343-enHYPHENus?q=system%20user)
* For a full list of supported commands, reference Developer Site documentation
  * [Reference: Workday Developer CLI (WDCLI) Commands](https://developer.workday.com/documentation/GUID-16d9d3f3-ba63-4022-8a84-ad78bd5189ca-enHYPHENus/ReferenceWorkdayDeveloperCLIWDCLICommands) 

## Impact

* Enhance your app release quality and speed by automating build and testing pipelines within your DevOps processes
* Manage end-to-end lifecycle of apps more efficiently with command-line workflows
* Programmatically authenticate with the Workday Developer Platform via the WDCLI

## How to Access

* Download the latest WDCLI version from the [Developer Site Downloads page](https://developer.workday.com/downloads) and install on your machine
* If you choose to leverage a System User account with WDCLI, Admins must first set up a System User on the [Users tab on the Developer Site console](https://developer.workday.com/console/users)

## Considerations & Limits

* **Legacy WCP CLI Versions**:
  * The legacy Workday Cloud Platform CLI (WCP CLI) has been replaced by the updated Workday Developer CLI (WDCLI) and is no longer maintained or available for download.
   * If you have a previously downloaded legacy WCP CLI version, you can continue using it until you are ready to migrate to the updated WDCLI.
   * The WDCLI supports all legacy WCP CLI v3 commands and introduces expanded functionality with one exception: WQL commands are deprecated and not available in the WDCLI. For data queries, migrate to REST API Explorers in alignment with updated Developer Site standards.
* **WDCLI Expectations**:
  * App commands executed via the Workday Developer CLI adhere to the same guidelines, best practices, and deadlines as the Developer Site. This includes specific promotion deadlines for Workday Extend apps with model components
    * [Reference: Promotion Deadlines for Extend Apps with Model Components](https://developer.workday.com/documentation/giw1660842903762)
* **System User**:
  * If you choose to set up a System User account for secure non-human authentication with the WDCLI, please note the following:
    * Each System User account you create is scoped to a single organization and cannot be shared across multiple organizations.
    * System User accounts cannot access or execute tenant-level operations that require specific tenant credentials.

## Demo Video

https://vimeo.com/1181759049/9d8d004e3c?share=copy&fl=sv&fe=ci