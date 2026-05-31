#  Add a Custom Extend Task to a Configurable ConnectR Wizard

## Prerequisite

This process outlines how an Extend developer integrates a custom user interface, created as an Extend edit task, into a ConnectR wizard for use and configuration by an administrator.

1. **Create a standard Extend edit task**.

This task is the page or form that will render as a section inside the wizard.

2. **Ensure your task is context-compatible**.

If your Extend task needs data from the ConnectR's associated business object (for example, Hire Event or Staffing Event), build it as a **context-based** task using that same business object as the context. Consult your administrator or the ConnectR documentation for the business object of the specific wizard you are targeting.

3. **Write your validation logic**.

The Wizard will call your Extend task's **validation logic** during the "Submit" phase of the wizard. If your step should be **mandatory**, implement validation logic that returns a blocking (critical) error when the required fields are incomplete.

**Note**: If you do not write validation, users will be able to skip your step entirely and still submit the wizard successfully.

4. **Register your task with the Workday administrator**.

The administrator must access the **Configure ConnectR** task, which is secured to the **Set Up: ConnectRs** domain.

Provide the administrator with the following information:
  * The name of your Extend edit task.
  * The section label you want displayed in the wizard.
  * The administrator will then add your task as a section, position it in the wizard order, and save the configuration.

## Result

The admin will add your task as a section, position it in the wizard order, and save the configuration.