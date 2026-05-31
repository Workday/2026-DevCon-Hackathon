## Overview

The Workday Everywhere plugin architecture leverages an SDK that allows professional developers to take an existing Workday Extend app and its functionality and quickly develop a version for Microsoft Teams. By providing official Workday surfaces like Microsoft Teams, Workday aims to boost the utility, adoption, and value of Extend applications.

## Key Attributes

* Integration Requirements: Developers can only add WE functionality to an existing Workday Extend app.
* User Experience: Assigned Extend apps appear as a native part of the Workday Everywhere experience. This maintains consistency between the capabilities available on the web and in Microsoft Teams.
* Design System: Workday Everywhere specifies the UI components and presentation available for developers to ensure the plugin follows well-known UI development patterns.
* Platform Support: Microsoft Teams is the primary target for initial development because it is popular with the Workday user base and offers fewer design restrictions. Future iterations may include Slack, Copilot, Gemini, and OpenAI cards.
* Non-Conversational Focus: This feature targets non-conversational surfaces, as orchestrated agents handle conversational interfaces.

## Business Benefits

* For Customers: Customers can access unique Extend functionality within the flow of work, reducing the need for users to log directly into Workday for specific tasks.
* For Users: Accessing Extend apps in Microsoft Teams results in less context switching and makes Workday functionality easier to access.
* For Developers: Developers gain additional surfaces to make their solutions available, driving usage and revenue.

## Use Case Examples

* User Access Review: Managers can review and edit access requests directly within Microsoft Teams.
* Vehicle Registration: Employees can handle vehicle registration processes without leaving the Teams interface.
* 360 Feedback: Employees can perform the feedback flow in Microsoft Teams rather than navigating to Workday.
* Payroll Information: Users can view payroll information from third-party providers like ADP directly in Teams.

## Limitations and Considerations

* Professional Developers First: The initial SDK and tooling target professional developers who have experience with React and complex development.
* Notifications: Notifications are not included in the plugin SDK; these are provided by the Workday Operational Everywhere (OE) service and triggered via traditional Workday processes.
* Prerequisite Work: Developers must first establish the business objects, logic, and data operations within a published Extend app before building the plugin experience.