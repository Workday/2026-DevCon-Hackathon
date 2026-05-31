# Prerequisites

* An Extend app in App Builder.  
* The Extend app contains a custom agent with at least 1 skill in the Extend app.   
  See [Build and Deploy Agents on Workday’s AI Platform](/docs/developer-agent/build-and-deploy-agents-on-workdays-ai-platform.md).  
* Familiarity with [PMD page](https://developer.workday.com/documentation/oau1528861566075) basics. 

# Context

A2UI chat tags enable your custom agent to render simple widgets directly in the Workday Self-Service Chat window. Using the chat tags, you can create forms users can fill out, cards that display data, buttons that send actions back to the agent. The end user stays in the chat experience the whole time. Build the chat tags in App Builder the same way you build any other PMD page, then a skill in your agent invokes the generate\_a2ui tool to render the A2UI page in the chat window.

# Steps

## 1.0  Create a New Page

In App Builder, create the page that will become your A2UI surface.

1. Open the Extend app In App Builder.

2. Add a **Page** component.

3. In the **Add Page** window, select **Create as A2UI Page**.  
   App Builder creates an A2UI-enabled page that sets the a2ui attribute to true at the root of the page.   
   By default, the page contains a chatCard with a chatText.

## 2.0  Build the Page Structure

Add chat tags to the page.

You can add chat tags and their corresponding attributes using either Visual or Code mode. Alternatively, you can use Developer Agent to add chat tags to the page.

For details about each chat tag and its attributes, see [Reference: Extend Chat Tags](/docs/extend/a2ui/a2ui-chat-tags-for-custom-agents.md).

Example: This sample A2UI-enabled page contains chat tags:
```
{
  "id": "CreateDogRegistry",
  "a2ui": "true",
  "endPoints": [],
  "presentation": {
    "title": {
      "type": "title",
      "label": "Workday Extend"
    },
    "body": {
      "type": "chatCard",
      "children": [
        {
          "type": "chatText",
          "text": "Create Dog Registry",
          "variant": "h3"
        },
        {
          "type": "chatDivider"
        },
        {
          "type": "chatRow",
          "justify": "START",
          "children": [
            {
              "type": "chatTextField",
              "label": "Dog Name:",
              "value": "<% queryParams.dogName ?? 'Enter Dog Name' %>",
              "valuePath": "/dogName"
            }
          ]
        },
        {
          "type": "chatRow",
          "align": "CENTER",
          "children": [
            {
              "type": "chatButton",
              "label": "Submit",
              "action": "add",
              "variant": "PRIMARY",
              "submitDataModel": true
            },
            {
              "type": "chatButton",
              "label": "Cancel",
              "action": "cancel",
              "variant": "PRIMARY",
              "submitDataModel": false
            }
          ]
        }
      ]
    }
  }
}
```

### **3.0 Add the routingPattern for the A2UI Page is in the AMD**

After your A2UI page has been created, ensure that the page entry in the Application Metadata (AMD) of your Extend app has a routingPattern that matches the A2UI page id. 

Example: This sample is an AMD entry for an A2UI-enabled page named CreateDogRegistry and routingPattern /CreateDogRegistry.

```
{
   “id”: “CreateDogRegistry”,
   “routingPattern”: “/CreateDogRegistry”,
   “page”: {
        “id”: “CreateDogRegistry”
   }
}
```

## 3.0  Deploy the App

Before your skill can reference the A2UI page, the page needs to exist in App Hub. 

Additionally, before your skill can use the Model Component Tools (APIs), the Extend Business Object(s) must first be deployed to the tenant for the skill to find their endpoints.

Click **Save and Deploy** in App Builder and confirm the deploy.

> **Tip**: If you skip deoloying the app and try to call `generate_a2ui` from your skill, the tool won't find the page. A2UI pages are resolved at runtime by their `appReferenceId/pageId` path against deployed App Hub artifacts.

## 4.0  Add A2UI to Your Agent Skill

Instruct your agent's skill on how to invoke the A2UI page. Open the skill in App Builder (or via Dev Agent) and update three things in the frontmatter metadata:

### 4.1  Allow the generate\_a2ui tool

In the allowed-tools section, add `generate_a2ui`.

```
allowed-tools:
  - generate_a2ui
```

The `generate_a2ui` tool gives the skill permission to render A2UI pages.

### 4.3  Tell the agent when to render the page

In the body of the skill, add a Steps section explaining when to invoke A2UI:
```
## Steps
 
1. To use A2UI, invoke the `generate_a2ui` tool and pass in:
   - Page ID (the page name)
   - Optional query parameters, if the page supports any
 
This should always be the last tool you use in a given turn.
```

> **Important**: The "**last tool in a turn**" rule matters: A2UI rendering ends the agent's turn and hands control to the user, who then interacts with the rendered page in the chat window and submits data back.

## 5.0  Handle the Submission

When the user clicks a Submit-type button (a chatButton with submitDataModel: true), the data flows back to your agent as a new turn. The agent receives the form data shaped by the valuePath definitions and the button's action value.

Your skill should describe what happens after submission. Example pattern from a working skill:
```
## Steps
 
1. When the user asks to create a new dog registry, call
   `generate_a2ui` with page name "CreateDogRegistry".
   This renders the form.
 
2. After the user submits the form, call the `post_dog_registry`
   tool with the form data to create the record.
 
3. Confirm success to the user with the relevant details.
```

The agent's LLM dispatches across these steps based on what just happened — when it sees the form data come back, it knows to call the persistence tool.

## 6.0  Deploy and Test

1. Save and Deploy the app with the updated skill.
> ****Note**: If your skill uses Model Component tools (POST, PUT, GET, DELETE against Extend Business Objects), you must first deploy your app with your business object(s) to your tenant, before your skill will be able to find their endpoints.**

2.  Open the Workday Chat Panel as a user in the agent's security group  
3.  Send a message that triggers the A2UI flow (example: *"Register a new dog"*)  
4. Confirm the A2UI page renders in chat window.  
5. Fill in the data and click the submit button.  
6. Verify the agent receives the data and correctly performs the action.