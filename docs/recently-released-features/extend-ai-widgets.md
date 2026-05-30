# Product Update: Extend AI Widgets are Now Generally Available

## Overview

We are excited to announce that Extend AI Widgets are now Generally Available (GA) as part of the 26R1 release! Accessible to all Extend Professional customers, this feature allows you to embed custom generative AI prompts directly into your application’s rich text interfaces.

## What’s New

The Extend AI Widget is a native capability within the rich text widget powered by Workday AI. By configuring AI prompts, developers can enable end-users to automate time-consuming tasks—like drafting job descriptions or condensing content—directly within their Extend applications.

## Key Capabilities

* **Seamless Configuration**: Add AI functionality directly to your edit pages using App Builder (in either Visual or Code mode).
* **Context-Aware Generation**: Reference live data from other fields on your page to ensure the generated content is grounded in real-time, relevant information.
* **Conditional Logic**: Control when the AI is active by setting dependencies, ensuring users only trigger a prompt once all necessary information has been provided.
* **Instant Iteration**: Test and refine your “Human-in-the-loop” experience instantly within App Preview to see exactly how your prompts will perform for end-users.
* **Consistent Experience**: Deliver a unified user interface that aligns with the AI interactions found within Workday’s core applications, providing a familiar and intuitive experience for your end-users.

## Impact

AI Widgets move the complexity of LLM integration into a simple metadata configuration, allowing you to build “AI-infused” apps without managing infrastructure.

* **Rapid Prototyping**: Transition from an idea to a functional AI feature in minutes using standard PMD attributes.
* **Contextual Accuracy**: Ground prompts in real-time application data through easy variable referencing, ensuring highly relevant results for your users.
* **Zero Infrastructure Management**: Workday handles the underlying LLM calls, security wrappers, and enterprise safety guardrails, letting you focus entirely on the prompt intent and user experience.

## Considerations & Limits

To ensure optimal performance and security within your applications, please keep the following technical constraints and best practices in mind:

### System & Usage Limits

* **Application Limit**: You can configure a maximum of 3 AI-enabled rich text widgets per Extend application.
* **Button Limit**: Each AI widget supports a maximum of 2 prompt buttons.
* **Character Constraint**: The total length of the prompt—including your authored instructions and the values of all referenced variables—must not exceed 1,000 characters. Requests exceeding this threshold will result in an error.

### Technical Placement & Behavior

* **Supported Contexts**: AI Widgets are only supported on Edit Pages. They cannot be used within a grid, panelList, loop, or pod.
* **Event Handling**: The `onChange` event on the richText widget is not triggered when the aiPrompt modal populates the editor with generated text. If your application logic relies on these changes, ensure you account for this behavior in your script.
* **Tag IDs**: When utilizing dependencyTags to control button enablement, ensure that all referenced input tags have a unique and correctly assigned id attribute.

### Data Privacy & Safety

* **PII and Sensitive Data**: Developers must strictly avoid including Personally Identifiable Information (PII) or sensitive data within prompt instructions or referenced variables.
* **Review Process**: As with all generative AI features, users must review all generated content for accuracy and professional tone before saving the data to the system of record.