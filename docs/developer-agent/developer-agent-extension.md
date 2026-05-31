## Overview of Developer Agent Extension

Developer Agent Extension provides access to Workday Developer Agent in any VS Code Compatible IDEs (VS Code, Cursor, Antigravity, etc..). Workday Developer Agent is an AI-based developer tool that helps you create and change Extend apps. Developer Agent Extension includes a preconfigured MCP Server and model provider to support your app development.

Developer Agent Extension shares the same session as the Workday Developer Command Line Interface (WDCLI), so you can log in to both Developer Agent Extension and WDCLI as a single action and work with both tools seamlessly in the same environment.

Developer Agent Extension can:

* Build an entire Workday Extend app from a single natural language prompt, including creating business objects and security domains.
* Modify an existing app.
* Validate its own work in real-time, allowing it to iterate and fix its own errors.
* Create and modify files within the repository automatically, if enabled.
* Independently generate mock data files, such as mock responses and configurations, to populate application views.

Using the WDCLI, you can:

* Run a local server to proxy a live preview of the application the agent is currently building.
* Use Developer Console Explorer to drill into page variables, such as those on edit pages.
* Run scripts live within the developer console.

## Create and Change Apps with Developer Agent Extension

### Prerequisites

*Workday Developer Agent* extension must be installed in your Visual Studio Code (VS Code) Editor. See the [Developer Tools](https://developer.workday.com/downloads) download page.

### Context

Use your VS Code Editor with the *Workday Developer Agent* VS Code extension to work with apps in your environment.

### Steps

1. In your VS Code Editor, start the *Workday Developer Agent* VS Code extension.
2. Enter your Developer Site credentials at the prompt.
3. In the chat prompt, enter prompt instructions for *Workday Developer Agent*.
4. To open a preview of your app, click in the terminal command field and enter `wdcli app preview app_name`, where *app_name* is the name of the app that you want to preview.