# Overview

A custom agent can use A2UI-enabled pages containing chat tags that add visual components to the Workday Self-Service Agent Chat window such as data entry forms, cards that highlight specific information, and buttons that perform actions.

The chat tags in an A2UI-enabled page are a subset of page tags, which support PMD-like bindings and nested structures. Although both PMD and chat tags are defined as page components of an Extend app, they are mutually exclusive. A page can contain only a set of either PMD tags or chat tags.

An A2UI-enabled page contains these root attributes:

* `id`: (Required) Unique page identifier.
* `a2ui`: (Required) Indicates the page is AwUI-enabled; this boolean must be true.
* `presentation`: (Required) Contains the chat tags.
* `endPoints`: (Optional) Contains inbound endpoints that fetch the data displayed by the chat tags.

Chat tags categories:

* **Container tags**: Display a structured layout in the chat window, such as a card and list containing editable or read-only tags.
* **Editable tags**: Used for data entry. These tags require these attributes: valuePath (defines how to send value to the agent) and value (defines the initial value when the chat window loads).
* **Read-only tags**: Used for display-only data.
* **Invisible tags**: Store data but hidden from the chat window, such as for tracking instance IDs.

For details about each chat tag, see [Reference: Chat Tags for Custom Agents](/docs/extend/a2ui/a2ui-chat-tags-for-custom-agents.md).

To add chat tags in an A2UI page and instruct the agent's skill to invoke the A2UI page, see [Add A2UI-Enabled Pages for Custom Agent](/docs/extend/a2ui/add-a2ui-enabled-pages-to-an-extend-app-with-a-custom-agent.md).

# Supported and Unsupported Features

* The A2UI chat tags currently support:
  * Page rendering.
  * Gathering data.
  * Clicking a button.
  * Simple data structures for editing.
  * Objects with dates, booleans, and strings.
  * Simple date formatting. However, dates might need the agent to convert the date from the A2UI format to the persistence format.
  * Numbers are treated as strings. However, `chatTextField` has a NUMBER variant that can support integers.
  * The `chatLoop` tag can iterate and display a list of data.

* These are not currently supported:
  * Outbound endpoints. Instead of adding outbound endpoints to the page, the agent associated with the Extend app can perform the skills that create or update data.
  * Data validation within the A2UI framework.
  * Error handling or persistence failures.
  * Persistence of lists and arrays.
  * Currency.
  * Complex date types.
  * Removing a page from the chat window after the page has rendered.

# Common Patterns

## Pre-populating Data from an Endpoint

If you want the form in the chat window to render with existing data, define an `endPoints` block in the page and reference it in the field values:
```
{
  "id": "EditDogRegistry",
  "a2ui": true,
  "endPoints": [
    {
      "name": "getDogRegistry",
      "url": "<% `https://your-api.com/dogs/` + queryParams.id %>",
      "authType": "sso"
    }
  ],
  "presentation": {
    "body": {
      "type": "chatCard",
      "children": [
        {
          "type": "chatTextField",
          "label": "Dog Name:",
          "value": "<% getDogRegistry.dogName %>",
          "valuePath": "/dogName"
        }
      ]
    }
  }
}
```


The agent invokes the page with a query parameter (example: `{ "id": "abc123" }`), so that when the page calls the endpoint, the form renders with the data.

## Read-only View (display data)

To display view-only data in the chat window, use `chatText` instead of `chatTextField`. The `chatText` tag can also use an endpoint to fetch data to display.

```
{
  "type": "chatText",
  "label": "Dog Name:",
  "text": "<% getDogRegistry.dogName %>",
  "valuePath": "/dogName"
}
```

> Note: The `valuePath` attribute on the `chatText` tag specifies a path in the data payload sent to the agent. The `valuePath` enables display-only fields to send data with an `action` when using a `chatButton`.

## Sending Data to the Agent

A `chatButton` triggers an action event sent to the agent when a user clicks the button. The agent will decide what to do based on the action. The `submitDataModel` attribute determines whether or not to send the form data on the chat window as context to the agent:

* If `submitDataModel` is true, the button's action is sent with all the form data included as context. Use this for Submit, Save, or Update buttons.
* If `submitDataModel` is false, only the action name is sent, no form data. Use this for Cancel, Back, or dismissive buttons.

The `action` attribute on the `chatButton` tag defines the name of the action that will be sent to the agent when this button is clicked. The agent uses the `action` to decide what to do next (example: "add", "update", "cancel")

When a user clicks the `chatButton`, the values on all editable tags in the chat form will be sent as data payload to the agent. The `valuePath` attribute on the editable tags (such as `chatTextField`, `chatCheckBox`, and `chatDateTimeInput`) defines the path location of the tag value in the data payload that’s sent to the agent.

Example:
```
{
  "type": "chatTextField",
  "valuePath": "/owner/name"
}
```
results in this data structure when the form is submitted:
```
{
  "owner": {
    "name": "<whatever the user typed>"
  }
}
```
## Storing a Hidden ID in the Chat Window

Use `chatHidden` to keep values like a record ID in the submitted data without showing them to the user:

```
{
  "type": "chatHidden",
  "value": "<% getDogRegistry.id %>",
  "valuePath": "/wid"
}
```

When the user clicks a `chatButton`, the agent receives the WID in the data payload so it can correctly target the right record on update.