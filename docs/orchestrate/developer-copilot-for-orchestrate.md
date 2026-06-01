# Developer Copilot for Orchestrate

## Overview
<img width="1920" height="1104" alt="image" src="https://github.com/user-attachments/assets/95978273-1625-4426-bbd1-e8956b793f8d" />


**[Workday Developer Copilot for Orchestrate](https://developer.workday.com/documentation/GUID-671bc6ea-7223-411c-867d-09b01d4ad6f6-enHYPHENus)** shifts how you build integrations — from manually selecting, placing, and configuring components step by step, to describing what you want in natural language and letting Copilot generate enterprise-ready orchestrations. It's purpose-built for Workday Orchestration Integrations (not a general-purpose code assistant) and it's intended to augment, not replace, the integration developer in the loop.

Copilot has two modes, both reached from the Developer Copilot button in the top-left corner of Orchestration Builder:

- **Edit mode** — describe a requirement, Copilot adds and configures the steps. Every proposed change is shown on a non-live copy of the orchestration and waits for your **Accept** or **Reject**.
- **Ask mode** — natural-language Q&A against your orchestration **and** all of Workday's Developer Site and Administrator Guide documentation. No risk of unintended edits — Ask mode can't modify the orchestration.

> ⚠️ **Developer Copilot is experimental. You must always review its work.** This is the same instruction Workday gives in the in-product banner, and it applies to your hackathon project too.

## The big idea: conversational generation with built-in Workday knowledge

A general-purpose AI coding assistant doesn't know that **Send Workday RaaS Request** is configured differently from **Send HTTP Request**, that ISU credentials live in a particular tab, or that your loop must be aggregate to produce a CSV. Developer Copilot for Orchestrate does.

Each step type ships with **planner guidance** (when to pick this step) and **configuration guidance** (how to configure it correctly for common patterns: authentication, error handling, pagination etc). That's what lets a one-line prompt — *"loop the workers in this report and store a CSV with employee ID, name, and hire date"* — turn into a correctly wired orchestration in seconds rather than a long afternoon. Enhance what gets built by using Custom Skills to bake in your teams knowledge into Copilot. 

Other notable mechanics:

- **Plan-and-execute architecture.** Each prompt is analysed against the current orchestration state, uploaded files, and conversation history, then execute an Agentic Loop across multiple reasoning rounds to refine until the plan is complete.
- **Transparent reasoning.** The chat panel streams the planning steps so you can see *why* Copilot chose what it chose — not a black box.
- **Human in the Loop.** Steps Copilot adds carry the Copilot icon until you Accept; you can adjust manually before accepting; **Reject cannot be undone**.
- **Persistent context across sessions.** Threads survive browser refreshes and sessions for **180 days**.


## How it works — [Edit mode](https://developer.workday.com/documentation/GUID-2490793c-adfa-4545-8850-49bf805a242b-enHYPHENus)

**Prerequisite:** an orchestration open in Orchestration Builder.

1. Click the **Developer Copilot** button in the top-left of the Orchestration Builder interface.
2. Ensure **Edit** is selected in the mode selector at the bottom of the **Prompt Input** box.
3. In the **Prompt Input** field, describe your requirements.
4. (Optional) Click **Add Sources** to attach up to **10 files** (≤ 250 KB each) — schemas, API specs, examples
5. (Optional) Click **Settings** to attach a `rules.md` or `skills.md` files/
5. Watch the chat panel — Copilot streams its status updates and may ask follow-up questions. If a response is taking too long, use **Stop Response** in the Prompt Input field to terminate it.
6. Copilot provisionally makes the change(s) on a non-live draft copy of the orchestration and summarises what it did. Inspect the new step properties.
7. Click **Accept** to commit, or **Reject** to discard. Rejection cannot be undone. Any manual edits you made since the Copilot prompt are bundled into the same change — accepting accepts everything, rejecting rejects everything.

## How it works — [Ask mode](https://developer.workday.com/documentation/GUID-6443e182-59b5-40d3-834a-96b319742bfb-enHYPHENus)

When you switch into it, the **Ask Agent** takes over and routes between four specialists sub agents — you don't have to think about which one to ask:

| Specialist | Grounded in | Example query |
|---|---|---|
| **Inspector Agent** | The actual steps, expressions, and data flow of the orchestration on your canvas right now | *"How does data flow from this HTTP request through my error handler?"* |
| **Docs Agent** | RAG retrieval over the Developer Site and Administrator Guide; answers include citation links | *"What's the difference between a Loop and a BatchLoop, and when should I use each?"* |
| **Insights Agent** | Historical runtime data Workday already collects | *"Which of my orchestrations has the highest p95 latency over the last 30 days?"* |
| **Execution Agent** | Recent failure details from runtime | *"My CCW outbound integration failed twice yesterday — what went wrong and how do I fix it?"* |

**The flow:**

1. Click the **Developer Copilot** button.
2. Select **Ask** in the mode selector.
3. Enter your query.
4. (Optional) Click **Add Sources** to attach reference files or skills.
5. Copilot answers in the chat panel. If it used published documentation in its thinking, it provides links to its sources.

> 💡 **Copilot remembers context within a thread.** You can chain queries — ask *"Which steps in my orchestration call RaaS reports?"* and then *"Tell me about them"* — Copilot resolves *"them"* from the previous turn.

## [Threads](https://developer.workday.com/documentation/GUID-04ecbff4-563a-48c3-826d-deb50e89c3a0-enHYPHENus) — your conversational workspace

Each prompt opens (or continues) a **thread**. Each orchestration can have multiple Active threads. Threads carry:

- **The orchestration state, your uploaded files, the full prompt/response history.** Injected into every follow-up turn so you can say *"update that message to say FAILED"* without re-stating context.
- **180-day retention.** Threads survive browser refreshes and sessions. After 180 days, the thread and all related data (messages, runs, files) are automatically cleaned up.
- **Auto-compaction.** As you approach the context window limit, older messages are summarised while recent messages and key decisions are preserved.
- **Owner-based privacy.** All threads are private by default — only the creator can view, modify, or delete their conversations.

**Supported file uploads** (10 files max, 250 KB each):
`.txt` · `.csv` · `.json` · `.xml` · `.xsd` · `.yaml` · `.yml` · `.spec` · `.md` · `.orchestration` 

## Customise Copilot with [Rules and Skills](https://developer.workday.com/documentation/GUID-810b9b3f-1a03-47ed-9083-cc0657eff29a-enHYPHENus)

A generic agent can build a generic orchestration. To get **your team's** standard error-handling pattern, **your** audit-log format, **your** CSV schema conventions — the agent needs your knowledge in the Agentic loop. That's what Skills and rules.md are for. This pattern isn't unique to Workday — it's the industry convention everyone is converging on. Claude Skills, Cursor rules, GitHub Copilot custom instructions, OpenAI's `AGENTS.md`. Same family of patterns, applied to integration development.

### [Writing a skills.md file](https://developer.workday.com/documentation/GUID-e45425df-4316-4953-a218-a4d4248ee8a2-enHYPHENus)

A skill is a Markdown file with a clear `description` (so Copilot can match it to your prompt) and at least one worked example. Keep skills self-contained — they should explain *when* the pattern applies and *what* it produces, with a sample Copilot can pattern-match against. Activate via the **Settings** icon in the Copilot chat panel.

### [Writing a rules.md file](https://developer.workday.com/documentation/GUID-8968a854-fb27-438f-b2ce-2f311e7087be-enHYPHENus)

Best practice for rules.md so Copilot can cite specific clauses when it audits the orchestration:

- **Use RFC 2119 keywords in caps:** MUST, MUST NOT, SHOULD, MAY.
- **Numbered sections (§1, §2 …)** so verification checks can name violated clauses precisely.
- **Scope statement at the top** — what this file is authoritative for, what attached files are authoritative for (column lists, format mechanics live in the attachments — don't duplicate).
- **Explicit "MUST NOT appear" section** for anti-patterns. Implicit prohibitions get missed.
- **§-prefixed Verification section** at the bottom — each check ties back to a declaration earlier in the file.
- **Keep it under 250 lines.**
- **Don't try to control Copilot's conversation flow or plan-confirm cadence** — that fights Copilot's intrinsic behaviour and produces nondeterministic results.

To upload: click the **Settings** icon at the top of the Copilot chat panel → **Add rules.md File** (or **Add skills.md File**). Rules attach at the orchestration level — every developer who opens that orchestration gets them — and only the developer who attached the file can remove it.

## Prompting tips

**For Edit mode:**
- **Break down complex requests.** Ask Copilot to make changes in stages so you can Accept/Reject incrementally
- **Be specific and provide detail.** Not *"add error handling"* — try *"add a global error handler that logs the error message and returns a 500 status with error details"*
- **Include context.** Step names, field names, expected behaviours
- **Reframe if it consistently misunderstands.** Rephrase with more specifics, upload reference files (schemas, API specs, examples), or break the request into smaller steps

**For Ask mode:**
- **Ask in separate simple queries** rather than one unbroken chunk — you'll get clearer answers
- **Chain queries** — Copilot remembers earlier turns in the same thread, so follow-ups can use pronouns naturally

**Universal:**
- Use **Good response** / **Bad response** buttons after replies — that feedback shapes future fine-tunes. We will be actively monitoring it and are considering extending insitu feedback generally to Orchestration Builder. 
- Click **How was this generated?** on any response to see Copilot's reasoning, or **Copy** to grab the text

## Hackathon project ideas

- **Talk an orchestration into existence.** Start from an empty orchestration. Prompt: *"Build an outbound orchestration that reads a RaaS report of new hires, validates required fields per worker, writes valid rows to a CSV, and attaches the CSV plus an audit log to the integration event."* Accept changes incrementally
- **Teach Copilot your team's house style.** Author a `rules.md` with your naming conventions, your standard error-handler shape, and your audit-log file naming pattern. Build the same hackathon project with and without the rules — show the difference in the output
- **Skills library for a common pattern.** Write a `skills.md` for "Workday RaaS pagination handling" with two worked examples. Have Copilot apply it to three different scenarios in three different apps
- **Self-documenting orchestration.** Open an existing orchestration in Ask mode. Prompt: *"Summarise what this orchestration does, list its inputs and outputs, and flag any steps that don't have error handling."* Use the response as the basis of a README
- **Stepwise refinement demo.** Start with a 3-step stub. In separate prompts: add error handling → add audit logging → add input validation → add pagination. Show the diff between each Accept
- **The debugging tutor.** Open a failing orchestration. Prompt: *"Why might step X be failing? What inputs does it need and which upstream step is supposed to produce them?"* Use the reasoning to fix it manually
- **Spec → orchestration.** Upload an external system's API spec (JSON / YAML / XSD) as a thread attachment. Prompt: *"Build an outbound orchestration that calls this API for each worker in my RaaS report and stores the response."*

## Privacy & trust

- **Your tenanted data is never used for training.** Full stop.
- **All threads are private** with owner-based access control. Only the creator can view, modify, or delete a thread.
- **180-day retention** then automatic cleanup of messages, runs, and files.
- **No live tenant context.** Copilot cannot reach into a tenant to inspect custom objects or business processes — it works from the orchestration JSON, your uploaded files, and the documentation corpus and runtime data.
- **Review is mandatory.** Every change lands on a non-live draft copy and waits for your Accept. Steps Copilot added carry the Copilot icon until you accept them.
- **Same runtime governance as any orchestration.** Generated orchestrations follow the existing runtime security model — ISU, ISSG, and domain policies all apply. There's nothing special about a Copilot-generated orchestration in production.

## Availability & limitations

**Availability:** Extend Pro customers and Extend Pro Partners, **North America and EMEA (Germany)** regions only.

**Component types Copilot cannot create, configure, or connect:**

- Put Amazon EventBridge Event
- Invoke AWS Lambda Function
- Get Amazon S3 Object
- Store Amazon S3 Object
- Send Prism Request
- Trigger PDF Generation
- Trigger Integration
- Create Workday Home Card

**Other limitations:**

- **Connectors:** Copilot does not work with [Pipedream Connectors](/docs/orchestrate/pipedream-connectors-for-orchestrate.md) today. If your hackathon project needs a third-party connector action, you place it manually
- **Orchestration types:** Copilot doesn't work with Journey orchestrations, Workday Studio integrations, or EIB integrations though Ask mode will happily address questions about these.
- **File attachments:** 10 files per thread, 250 KB each
- **Discarded changes can't be recovered** — be deliberate about Reject
- **No live tenant introspection** — Copilot can't browse your tenant for custom objects or BPs. This is on the Roadmap.
