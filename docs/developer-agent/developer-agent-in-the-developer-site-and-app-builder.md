## Overview

The Developer Agent helps you quickly generate and modify Extend components - including Pages, PMDs, Business Objects, and GraphQL queries - using natural language. By leveraging a new agentic architecture, you can move from high-level questions to iterative code refinement in a single, conversational session.

## Accessing the Developer Agent

Developer Agent is automatically available in all Hackathon organizations with no additional configuration required. You can access the Developer Agent through several entry points:

* Developer Site Home Page: Ask a quick question directly from the Developer Home page using the centralized prompt bar.
* Developer Site: For general Q&A and technical documentation search via the icon in the top-left of the page.
* App Builder: For context-aware code generation and application modification within a specific project.

## Generating Components

The Developer Agent allows you to describe the functionality you need in plain English.

1. Open the Agent: In App Builder, click the Developer Agent icon in the header.
2. Enter a Prompt: In the chat input, describe the component or feature you wish to build.

  * Tip: Click the Prompt Library icon for pre-defined templates.

3. Refine the Output: If the generated code needs adjustment, simply provide feedback in the chat (e.g., "Add a required constraint to the Submit button").
4. Enter Draft Mode: Once satisfied, click Review Proposed Edits.

## Example Prompts

Building New Components & App Shells

* "Create a PMD for a worker profile page that displays name, title, and start date."
* "Build a complete shell for a 'Project Tracker' app including a Business Object for Tasks and a dashboard page to view them."
* "Generate a Business Object for tracking 'Office Supplies' with fields for Item Name, Quantity, and Cost."
* "Build a GraphQL query to fetch all open job requisitions assigned to the current user."

Modifying Existing Components

* "Update my 'EmployeeList' PMD to include a search bar at the top that filters by last name."
* "Add a validation rule to my existing 'SubmitExpense' script to ensure the total is greater than zero."
* "Modify the 'ProjectDetails' page to change the 'Status' field from a text input to a dropdown with 'Active', 'Pending', and 'Closed' options."
* "Optimize the 'getAllWorkers' GraphQL query to also return the 'primaryPosition' for each worker."

## The Draft Mode Workflow

To ensure safety, all code generation follows the Draft Mode lifecycle, providing a draft version to test changes before they affect your app.

### 1. Review Edits

Entering Draft Mode changes the App Builder state:

* Visual comparison: For existing files, the editor shows a side-by-side diff (green for additions, red for deletions).
* Scoped Explorer: The file tree filters to show only the files impacted by the Agent's changes.
* Read-Only: The editor is locked to read-only during review to prevent manual conflicts.

### 2. Chat to Refine Edits

While reviewing the draft, you can continue to chat with the Developer Agent. If you notice something that needs a tweak during your review or while using the App Preview, simply ask the Agent in the chat panel.

* Example: "The layout looks good, but can you change the primary button color to blue?"
* The Agent will update the draft in real-time, allowing you to iterate without leaving the review environment.

### 3. View Edits in App Preview

While in Draft Mode, click Open App Preview. This launches a temporary, live instance of your application with the Agent's proposed changes applied. Use this to verify UI layout and logic.

### 4. Finale Edits and Exit Draft Mode

* Apply Changes: Click "Apply" to view a checklist of all files. Confirming will commit the code to your application and exit Draft Mode.
* Discard Draft: Click "Discard" to revert all proposed changes and return to the last saved state.

Note: Developer Agent Proposed changes are not saved to your application until you apply the changes. These changes will not persist to your app if you do not click that button.

## Supported Components

The Developer Agent can generate and modify:

* App Configurations & UI Components: Pages, AMD, and SMD.
* Logic: PMD Scripts (both full modules and within a PMD).
* Data & Integration: Business Objects, Security Domains, Mock Data, Graph Queries, and Workday Query Language (WQL) components.

Note: While the agent is specifically optimized to generate the components listed above, it also has access to the full breadth of Workday documentation. Consequently, it may be able to generate or provide guidance on other application components if prompted.

## Limitations & Considerations

* Refreshing in Draft Mode: Refreshing your browser while in draft mode will result in the loss of the draft, so be sure to accept or reject changes prior to refreshing.
* Specific Context: The Agent understands the app you are currently in. For the best results, reference specific existing component names in your prompts.
* Manual Edits: You cannot manually type in the code editor while the Agent is in a "Generating" or "Draft" state.
* Character Limit: A single prompt can have a maximum of 10,000 characters.

## Best Practices

* Be Specific: Instead of "Make a list," try "Create an Instance List on the 'Home' page that pulls data from the 'Supplies' BO."
* Clear Chat for New Contexts: Use the "Clear Chat" button when moving to a completely different feature to reset the Agent's memory and improve accuracy.
* Review Documentation Sources: When the Agent provides a conceptual answer, toggle the "Show Documentation Sources" to view the underlying Workday documentation.