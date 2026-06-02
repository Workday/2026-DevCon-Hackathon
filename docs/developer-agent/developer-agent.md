## Developer Agent

**Utilize your agentic tool of choice and build at the speed of innovation.**

<video>
  <source src="https://github.com/user-attachments/assets/29585bd8-2b2f-4874-8110-efd619d5b144" type="video/mp4">
  Your browser does not support the video tag.
</video>

https://github.com/user-attachments/assets/29585bd8-2b2f-4874-8110-efd619d5b144

Developer Agent lets you scaffold, validate, and ship Workday Extend apps end to end from the tool you already work in.

* **Choose your agentic tool.** VS Code, Cursor, Google Antigravity, Claude Code, Codex or any MCP-compatible client.
* **Integrated with the Workday CLI.** Sign in once, pick your org, and you're ready.
* **From prompt to production app.** Describe what you want to build, and Developer Agent scaffolds, validates, and helps you ship it.



For more details, see the related Documentation:

* [Developer Agent in the Developer Site and App Builder](/docs/developer-agent/developer-agent-in-the-developer-site-and-app-builder.md)  
* [Developer Agent Extension](/docs/developer-agent/developer-agent-extension.md)  
* [Build and Deploy Custom Agents](/docs/developer-agent/build-and-deploy-agents-on-workdays-ai-platform.md)  
* [Build A2UI Pages for Custom Agents](/docs/extend/a2ui/a2ui-pages-for-custom-agents.md)  

## **Known Issue: Local Disk Sync with Orchestrations**

Local Disk Sync does not currently work for Orchestration Builder due to a bug. Until this is resolved, Developer Agent Desktop users with Orchestrations in their apps should follow this workflow:

1. Build your app’s Extend components locally in Developer Agent Desktop (without Orchestrations). Local Disk Sync can still be leveraged for Extend Components via App Builder.  
2. If you need to make Orchestration edits, upload your current Extend changes from Dev Agent Desktop to App Hub via the `wdcli upload` command.  
3. Open the newly uploaded application in Orchestration Builder on Developer Site without Local Disk Sync activated. Make necessary orchestration edits and Save to App Hub.  
4. Download the newly updated app source via the `wdcli download` command and confirm that your newest Orchestration edits were included. 
5. Resume local development for Extend components in Developer Agent Desktop. Repeat steps 2–4 for any future Orchestration changes. 