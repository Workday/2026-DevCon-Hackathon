# What It Is

You can use this topic to help make decisions when considering these strict guidelines. Violating them will cause broken user experiences, silent failures, or blocked configurations. It explains:

* Task Design Rules
  * Use only tasks that edit data.
Do not use tasks that create data. **Configurable ConnectR** supports edit (update) tasks only. Create tasks have a different lifecycle and are not compatible with the wizard flow. They are not idempotent and will create orphan instances every time the user submits that section.

  * Build single-step workflows only.
Your **Extend** task must be a single-step form. Do not build multi-step wizards or flows inside the task—nesting a wizard inside a wizard is not supported and will result in broken navigation.

  * Do not use **Extend** tasks already embedded in Hubs or another Wizard
     Do not reuse a task already rendered inside a Hub page definition. Tasks designed for those contexts have different rendering and navigation assumptions that conflict with the ConnectR flow. The Hub navigation will be removed if it is added.

  * Do not create landing pages with navigation buttons.
   If your **Extend** task renders a landing page with a "Go to X" or redirect button,   clicking that button removes the user entirely from the ConnectR workflow. The user will lose their wizard context and any unsaved work from prior steps.

  * Do not use the editWizard widget inside your task.
   The editWizard (Wizard widget in Extend) cannot be nested inside a Workday-delivered ConnectR. Using it will produce unpredictable rendering and navigation behavior.

## Limitations

The following capabilities that work in other Extend contexts are not supported when your task is embedded in a ConnectR:

|Feature|Behavior in ConnectR|
| --- | --- |
|**Print / Export**|Extend sections are excluded from the Task Wizard print/export function. Users cannot print **Extend** sections.|
|**Save for Later**|Save for Later does not support **Extend** sections. Users will see a tooltip indicating "Not all pages support save for later" when **Extend** sections are present.|
|**Auto-Save**|The Auto-Save button is disabled for **Extend** sections. A tooltip reads "This page does not support automatic saving."|
|**Recovery Assistant**|Recovery Assistant does not restore data from **Extend** sections.|
|**Extend Warnings**|Warning-level validations returned by Extend are not surfaced by the Task Wizard. Only critical/blocking errors affect submission.|


## Business Processes Restriction

### Do not use business-process-backed tasks.

Workday-delivered ConnectRs support only one business process. If your **Extend** task is backed by a business process, it will conflict with the ConnectR's own business process. This is a known limitation. Support for Consolidated Business Processes within Extend is not yet available. Use a non-BP-backed task.

## Section Ordering Constraints

* You can add a maximum of 5 custom sections to a single wizard via Configure ConnectR.
* Workday-delivered sections have a fixed relative order that cannot be changed. Placing a Workday-delivered section out of its defined order will produce a blocking validation error.
* You can interleave your custom sections between Workday-delivered sections at any position.

## Change Handling

Workday may update the underlying wizard definition (add or remove delivered sections). When this happens:

* Administrators will view a **Change Detection** screen the next time they open **Configure ConnectR**, prompting them to review and confirm the changes before editing.
* New Workday-delivered sections will be appended to the end of the configured order.
* Workday-delivered sections that are removed will be detected, and the administrator will be alerted.
* When a user runs the Wizard, if the administrator has not yet reviewed a pending change detection, Workday will automatically apply the Workday change to ensure the wizard is in a valid state.

## Context Passing

If your **Extend** task needs context from the **ConnectR** (for example, the **Hire Event** or **Staffing Event** being processed), the administrator can configure a **Context Instance** when adding your section. This can be:

* Derived from the **Wizard Context**—the business object the wizard is operating on (the default behavior).
* Derived via a **Report Field**—using a global field to derive the context (that is, current worker).

Work with your administrator to determine the correct context setup for your use case.

## Validation Behavior

When the user clicks Submit at the end of the ***wizard***:

* The **Wizard** calls Workday-delivered section validations.
* In parallel, it calls the **validation logic** of each **Extend** section.
* **Extend** validation has a 60-second timeout. If the timeout is exceeded, the error will be attributed to the **Extend** section and displayed to the user.
* Any critical or blocking error from either side will prevent event submission for business-process-backed **Wizards.**
* Error nodes from **Extend** validations are mapped back to their respective sections—the section icon in the side panel will display an error indicator, and deep links will route to the correct section.

## Task Review Step

When the user reaches the **Review & Submit** step:

* Workday-delivered sections render in their normal review format.
* Extend sections render in view-only mode—the **Task Wizard** calls **Extend's** initial URI, gets the view URL, and renders the **Extend** page as a read-only summary.