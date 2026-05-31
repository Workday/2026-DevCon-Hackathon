# Read Agent Traces

### Prerequisites
[Build and Deploy Agents on Workday’s AI Platform](/docs/developer-agent/build-and-deploy-agents-on-workdays-ai-platform.md).

### Context
Every time a user sends a message to your deployed agent, the platform captures a full trace of what happened. This includes which services were invoked, what was passed in, and what came back. You can view these traces directly in App Builder.

There’s no single right way to read a trace. You can click around and get familiar with what each node shows. A few things worth checking:

* **Did the model call a tool?** Look at the Outputs of `AviatoTurboChatCIS`, the tool calls appear there as structured JSON alongside the response text.
* **What tools was the agent given?** Look at the Inputs of `AviatoTurboChatCIS`, the the tool list is passed in at inference time.
* **Did the response render correctly?** Check the Outputs of `A2UIMiddleware`, this is what the Chat Panel actually received.

Each trace is a complete picture of one run. When you’re debugging, comparing a trace from a working run against one that failed to quickly find what changed.

### Steps
1. Open your extend app in the App Builder.
2. Click the Agent Traces tab at the top of the page.
You’ll see a table listing recent runs. Each row is one agent invocation — it shows the timestamp, the agent that ran, and a snippet of what the user asked. Click Refresh Table if you don't see any rows. Traces can take a few seconds to appear after a run completes.

3. From the run you want to inspect, click View trace details.
From the detailed view, you'll see a tree of nodes. Each node is a service or component that processed the request as it moved through the platform.

4. Click any node to expand it.
Each node has two sections:
   * Inputs, the data passed into that component
   * Outputs, the data it returnedWork through the tree from top to bottom to follow the path your agent took.

5. Explore `A2UIMiddleware` in the trace tree.
A2UIMiddleware is the component that handles rendering in the Chat Panel. It processes the agent’s response and prepares it for display.
      1. Click `A2UIMiddleware` in the trace tree.
      2. Click Inputs to see the raw payload the middleware received from the agent.
      3. Click Outputs to see what it produced for the Chat Panel to render.

6. Explore `AviatoTurboChatCIS` in the trace tree.
AviatoTurboChatCIS is the chat inference service and is where the model call happens. This is the most informative node in the trace.
      1. Click `AviatoTurboChatCIS` in the trace tree.
      2. Click Outputs to see the model’s full response, including any tool calls it decided to make.
      3. Click Inputs to see exactly what the model was given: the system prompt, conversation history, and the tool definitions your agent was loaded with.

    The Inputs to `AviatoTurboChatCIS` are the clearest way to verify that your agent’s tools and instructions are being passed correctly. If a tool is missing here, check your ASOR configuration and redeploy.

### Next Steps
* Iterate on your agent. Return to App Builder, prompt Dev Agent to make changes, and redeploy.
* Add an orchestration to connect your agent to external services.



# Preview Agents
### Prerequisites
[Build and Deploy Agents on Workday’s AI Platform](/docs/developer-agent/build-and-deploy-agents-on-workdays-ai-platform.md).

### Context

When building your agent, you can use the Agent Preview to review your agent’s response to iterate quickly.

1. Open your extend app in the App Builder.
2. Select the agent you want to test.
You must have done at least 1 **Save and Deploy** and configured and activated the agent in ASOR.

3. Select the **Play** icon in the far right side bar to open the Agent Preview.
4. Type into the Agent Preview to test the agent.

### Results

You test your agent and its skills without redeploying to the tenant.