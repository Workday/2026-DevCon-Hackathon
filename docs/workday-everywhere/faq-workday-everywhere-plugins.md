### Can I build a plugin without an existing Extend app?

No. You must first create and publish an Extend app using established Workday Build tooling. This prerequisite step is crucial because it establishes the business objects, logic, graph queries, and data operations that the plugin utilizes.

### Which platforms are supported for WE plugins?

Workday is targeting Microsoft Teams first because it is popular with the user base and offers fewer design restrictions. While the current focus is on Teams, the system is being developed with Slack and other agentic surfaces, such as Copilot, Gemini, and OpenAI cards, in mind for the future.

### Do I need to be a professional developer to use the SDK?

Yes. Workday is targeting professional developers first as they have the most knowledge of SDK usage and complex development. Creating tooling for low-code and no-code users would require larger investments in UI and data models that are not part of the initial phase.

### How are notifications handled in the plugin?

Notifications are not included in the plugin SDK currently.

### How do I ensure my plugin follows Workday design standards?

The Workday Everywhere SDK provides the appropriate guardrails for development while offering developers a wide range of options to build their plugin.

### How do I test the plugin with real data?

You test the complete experience by connecting to the Microsoft Teams simulator via OAuth. This allows you to link to your DevSite tenant and interact with your plugin using the DevCon tenant data.

### Can I build a plugin that is independent of a Workday app?

No. Developers can only add Workday Everywhere functionality to an existing Workday Extend app.