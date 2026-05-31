# DevCon 2026 Feature: Pipedream Connectors for Orchestrate

## Overview
https://vimeo.com/1197158006/491bbf2840?share=copy&fl=sv&fe=ci

Connectors for Orchestrate are preconfigured bridges that link your Extend Or Integration app's orchestrations to external services, without writing custom HTTP requests, parsing low level API responses by hand, or implementing OAuth flows yourself. Add a connector to your app, supply credentials once per environment, and a catalog of pre-built actions becomes available as drag-and-drop steps inside Orchestration Builder.

Orchestrate ships with 250+ connectors across 19 categories, with hundreds of actions each. Examples include **GitHub** (create issue, open PR, comment), **Salesforce** (create contact, update opportunity), **Slack** (send message, post to channel), **Gmail** (send email, create draft), **Anthropic / OpenAI / Gemini** (chat, classify), **AWS** (S3 upload, Lambda invoke, DynamoDB query), **Stripe** (create charge, refund), **Jira** (create issue, transition), **Google Sheets** (append row, query), **Notion**, **Calendly**, **Mailchimp**, **Microsoft Teams**, **HubSpot**, **Asana** — and many more (full catalog below).

The **Action Explorer**, available from every action's properties panel in Orchestration Builder, lets you test an action live, provide sample inputs, and inspect the JSON response — so you can wire downstream steps with confidence before you ever run the orchestration end to end.


##  No OAuth app registration needed

For OAuth-based services — Google Workspace, Microsoft 365, Slack, Salesforce, GitHub, Calendly, HubSpot, and dozens more, you don't have to register your own OAuth application with the provider. Each connector uses a pre-approved official Pipedream OAuth client for the relevant provider.


No client ID/secret to manage, no redirect URI to host, no app-verification submission to the provider, no internal "please-approve-this-OAuth-app" ticket. For a hackathon project this collapses days of platform onboarding work into a single click.


## How it works

### 1. Add a connector to your app

**Prerequisite:** an existing Extend or Integration app.
![image|690x398](upload://jYq9dDG0IYpdMwMwmBdKawYlQLx.png)
![image|690x397](upload://5NcqcC5Gs0AtID8gUbMYYwtfQj5.png)

1. On the app's home page, click the Connectors tab, then Add Connector.
2. Pick a connector from the searchable Catalog of Connectors (sorted by category). Click **Continue**.
3. Provide a **Name** and click **Create**. The connector appears under **Development** on the Connector Tab — click it.
4. Connect at least one service accounts as **Default** (used in Development, Implementation and Sandbox unless overridden). Click **Connect** beside a tenant type and supply the credentials the connector requires (varies by service — could be username/password, an API key, or an OAuth flow).

> ⚠️ **Never use a personal account.** Use a dedicated service / integration account.

**Result:** the connector — and all of its actions — is available to every orchestration in that app, in the configured environment.
![image|690x396](upload://AsasYjFQD4y89H1sEwu4n5F8m45.png)

### 2. Add a connector action to an orchestration

**Prerequisite:** the connector is added to the app and linked to a service account.

1. In the orchestration, click the **Connectors** button on the left of the canvas (below **Components**). Orchestration Builder shows a searchable list of available connectors.
2. Pick the connector you want to use. Builder shows the actions associated with it.
3. Drag the action to the right place in the flow. Its properties panel opens automatically.
4. Give the step a **Reference Name**.
5. Under **Connected Account**, select the connector instance.
6. Configure the action's properties (required fields are shown by default, with optional fields below). Use Expression Builder to reference upstream step outputs. Example: the Google Docs **Create a New Document** action has a required `Title` and optional `Text`, `Use Markdown Format`, and `Folder`.
7. (Optional but recommended) Click **Open Action Explorer** to test the action with live inputs, see the actual JSON response shape, and use it to build downstream steps. Close the explorer when you're done.
8. Click **Close** when properties are configured.

**Result:** the action behaves like any other step — accepts upstream input, produces output, fits into loops, conditions, suborchestrations, and error handling exactly like the rest of the orchestration palette.
![image|690x397](upload://sfZUoX2tkXHwcFBYxo0g6ZA8iab.png)


## Tips before you start

* Connectors are scoped to the **app**, not the individual orchestration. Once added, every orchestration in the app can use the connector's actions
* For hackathon you only have Development tenants so can skip production configurationl
* Use **Action Explorer** to discover the JSON shape
![image|690x389](upload://msF7S8FdOTPs2H0nSh7do1QqLII.jpeg)


## Hackathon project ideas

* **HR → external CRM:** Hire BP fires → orchestration creates a Salesforce contact and Slack-DMs the buddy with the new hire's info
* **Approvals → comms:** BP exception in Workday → Microsoft Teams adaptive card with approve/deny that resumes the orchestration on callback
* **AI-in-the-loop classification:** Send a free-text manager request to Anthropic (Claude) → parse the model's classification → route the BP based on the result
* **Documents anywhere:** RaaS report runs → upload the CSV to Google Drive (or OneDrive / Box / Dropbox) → drop the link into a Slack channel
* **Onboarding kit-out:** Job requisition approved → GitHub creates a hiring repo from a template, Jira opens the onboarding epic, Calendly schedules the kickoff call
* **Compliance evidence:** Sensitive BP completes → push a record into Snowflake / PostgreSQL with the audit trail
* **External event → Workday:** Stripe webhook (via HTTP/Webhook connector) → orchestration creates an expense entry or triggers a BP
* **Doc generation:** Worker performance review submitted → call PDF.co or PandaDoc to render a templated PDF → store as a worker document

## Connector catalog (full list, by category)

### Artificial Intelligence
AI Textraction · Anthropic (Claude) · Brave Search API · Devin · Exa · Fal.ai · Fathom · Final Scout · Fireflies · Google Cloud Vision · Google Gemini · Google Vertex AI · Groq Cloud · OpenAI (ChatGPT) · OpenRouter · Perplexity · Pinecone · Tavily · Webscraping.AI · x.AI

### Business Management
Cal.com · DataForSEO · Dex · Drata · Findmymail · Gong · Google Business Profile · Microsoft Bookings · Odoo · Rentman · SharePoint · Trustpilot (Customer) · WICS · Zoho Desk

### Commerce
Belco · BigCommerce · Bitget · Chargebee · DHL · EODHD APIs · Flodesk · FreeAgent · Gorgias · Mercury · Monta · Pexels · Pinterest · Plaid · Quickbooks · Selling Partner API · Shopify (OAuth) · Stripe · WooCommerce · Xero Accounting · Zenfulfillment · Zoho Books

### Communication
2Chat · Aircall · Bot for Slack · Canva · ClickSend SMS · Discord · Discord Bot · Gmail · Google Chat · Intercom · LinkedIn Ads · Microsoft Outlook Email · Microsoft Teams · MixMax · Pushover · Quo · Resend · RingCentral · Slack · Slack (Legacy)

### CRM
Apollo.io · Attio · Brevo · Close · ContactOut · Copper · Google Contacts · HubSpot · Hunter · Lusha · Microsoft Dynamics 365 Sales · Overloop · Pipedrive · Prospero · Salesforce · Streak · Wealthbox

### Data Analytics
Google Ads · Google Analytics · Metabase · News API · People Data Labs · PostHog · RentCast · RocketReach

### Database
Baserow · Database · Microsoft Azure SQL Database · Microsoft SQL Server · MongoDB · MySQL · Neon Postgres · PostgreSQL · Snowflake · Supabase · Supabase Management API

### Developer Tools
Ahrefs · Apify · Apify (OAuth) · Browserless · Cloudinary · GitHub · GitLab · LinkupAPI for LinkedIn · Phantombuster · Sentry · WhoisFreaks · ZeroBounce

### Entertainment
Spotify · Strava · Twitch · YouTube Analytics · YouTube Data

### File Storage
Box · Docusign · Dropbox · Google Docs · Google Drive · Google Photos · Google Slides · Microsoft OneDrive · PandaDoc · PDF.co · SFTP (password-based authentication) · Zoho WorkDrive

### Help Desk and Support
Delighted · Freshchat · Freshdesk · Help Scout · ServiceNow · TOPDesk · Trengo · Zendesk

### Human Resources
Ashby · BambooHR

### Infrastructure and Cloud
AWS · Cloudflare · Digital Ocean · Firebase Admin · Google Cloud · HTTP/Webhook · Outscraper · Pipedream Utils · SSH (key-based authentication) · Vercel

### Marketing
ActiveCampaign · Beehiiv · Cloud Convert · Emailable · EmailListVerify · Eventbrite · Facebook Pages · Figma · Front · lemlist · LinkedIn · Mailchimp · MailerLite · Mailgun · Outreach · Postmark · Reddit · Twilio SendGrid

### Productivity
Acuity Scheduling · Airtable · Asana · Basecamp · Calendly · ClickUp · Clockify · Coda · Confluence · Google Calendar · Google Search Console · Google Sheets · Google Tasks · Grain · Harvest · Jira · Linear (APIKey) · Linear (OAuth) · Microsoft 365 Planner · Microsoft Excel · Microsoft Outlook Calendar · Microsoft To Do · Monday · Motion · Notion · Notion (APIKey) · Podio · Raindrop · Shortcut · Teamwork · TickTick · Todoist · Trello · Wordpress.com · Zoho Projects

### Social Media
Bluesky · Wordpress.org

### Surveys and Forms
Google Forms · Jotform · SurveyMonkey · Tally · Typeform

### Travel
Booking Experts · Google Maps (Places API) · OpenWeather API

### Web and App Development
Alpaca · change.photo · DotSimple · FireCrawl · Ghost.org (Admin API) · Google Tag Manager · Mintlify · Miro Developer App · Netlify · Polygon · Sanity · ScrapingBot · SerpApi · Turso · Webflow


## Security considerations

Connectors use **Pipedream-managed authentication**. For OAuth-based services, that authentication is performed via the **official Pipedream OAuth client** — a single, shared client per third-party provider that is used across the entire Pipedream platform. It is **not** an OAuth client unique to your Workday organisation.

The third-party provider, and any administrator who reviews approved apps in that provider, identifies the connection by this OAuth client. When you connect an account using the default flow, you are authorising the official Pipedream OAuth client.

### If your organisation restricts third-party OAuth apps

Some service providers let an administrator control which third-party OAuth applications can access organisation data — for example, **Google Workspace API controls** or **Microsoft Entra admin consent**. If your organisation uses these controls, review the following before connecting accounts:

* **Approving the connector's OAuth app means approving the official Pipedream OAuth client.** Because that client is shared across the platform, the approval is not limited to your organisation.
* **An approval scoped only to the Pipedream OAuth client cannot distinguish one connector in one app from another application that uses the same Pipedream OAuth client.** Approval is at the client level, not at the per-Workday-app or per-connector level.
* **A custom (org-owned) OAuth client is not currently available** in Orchestrate. The approval cannot be scoped to a client that belongs solely to your organisation.

For production rollouts, share this section with your IT/security stakeholders before you connect production credentials.


*Each connector exposes its own catalog of actions — too many to list here. Open Orchestration Builder, drop the connector in, and use Action Explorer to browse the actions and their JSON responses live.*