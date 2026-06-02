# Developer Agent Sample Prompts

This documentation contains some sample prompts that may help you understand how to utilize some of Developer Agent's functionality when building and working with apps. Use these as a starting point for your use of Developer Agent, but expand upon these as you get comfortable with the tooling and capabilities.

## Component Generation

<details>

<summary>View sample prompts for component generation.</summary>

| Category | Prompt | 
| -------- | ------ | 
| List PMD | Build a PMD for a 'My Time Off Requests' page that shows a list of the current user's time-off requests with filters for status and date range. | 
| Detail PMD | Create a detail PMD for a Project business object that shows project name, owner, start/end dates, budget, and a related list of tasks. | 
| Wizard PMD | Build a wizard-style PMD with three steps for onboarding a new vendor: basic info, tax details, and banking details, with validation at each step. | 
| Dashboard PMD | Generate a dashboard PMD with three KPI tiles (open requests, pending approvals, overdue tasks) and a chart showing requests by status. | 
| Business Object | Create a business object called `PurchaseOrder` with fields for PO number, vendor, line items (as a related list), total amount, status, and created date. | 
| Business Object with Attachment | Add a `ContractDocument` BO that stores contract metadata and has an attachment field for the signed PDF. | 
| PMD Script - Validation | Write an onSubmit script for `expenseReport.pmd` that validates the total is under $5,000, checks that all line items have receipts attached, and then calls my `submitExpense` endpoint. | 
| PMD Script - Dynamic Filter | Write a script that, when a user selects a country, dynamically filters the 'State/Province' dropdown to only show options for that country. | 
| WQL Endpoint | Build a WQL endpoint that pulls all fields for the `Worker` business object where `hireDate` is within the last 90 days and `status` is 'Active'. | 
| WQL with Grouping | Create a WQL query that returns all open tasks assigned to the current user, grouped by priority, ordered by due date ascending. | 
| GraphQL Query | Generate a GraphQL query against Workday that returns worker ID, legal name, job title, and supervisory organization for a given worker reference. | 
| REST Outbound Endpoint | Build an outbound REST endpoint that POSTs an approved expense report as JSON to an external endpoint, with retry logic on 5xx responses. | 
| Security Domain | Create a security domain for this app that allows HR Partners to read/write and Managers to read-only, and scaffold the permission checks in the relevant PMDs. | 
| Role-Based Visibility | Set up role-based visibility on `salaryAdjustmentPage.pmd` so only users in the 'Compensation Admin' group can see the salary fields. | 
| Reusable Fragment | Generate a reusable PMD fragment for a 'Worker Info Card' that displays photo, name, job title, and manager - so I can embed it in multiple pages. | 

</details>

## Whole App Generation

<details>

<summary>View sample prompts that generate whole applications.</summary>

| Category | Prompt | 
| -------- | ------ | 
| IT Equipment Requests | Build me an Extend app for tracking internal IT equipment requests. Users should be able to submit a request, managers should be able to approve/reject, and IT admins should see a queue of approved requests to fulfill. | 
| Training Management | Create a full Extend app for managing internal training courses. Include course catalog, enrollment, completion tracking, and a manager view showing team completion rates. | 
| Peer Recognition | Build an Extend app that lets employees submit peer recognition ('kudos') to coworkers. Include a feed page, a submission form, and a leaderboard view. | 
| Safety Incidents | Generate an Extend app for tracking workplace safety incidents. Employees file reports, safety officers review and categorize them, and there's a dashboard showing trends by location and severity. | 
| Vendor Onboarding | Create a complete vendor onboarding app with a multi-step form for vendors, an internal review queue for procurement, and automatic notifications at each status change. | 
| File Sharing | Build a file sharing app utilizing attachment objects. Consider the most popular features of similar file sharing apps (authorized users, folder hierarchy, versioning, share links). Include the full object model and all pages. | 
| Desk/Room Reservations | Generate an Extend app for managing office desk/room reservations. Include a floor map view, availability calendar, booking form, and admin config page for adding new spaces. | 
| Internal Marketplace | Build me an Extend app for a company-internal marketplace where employees can post items for sale to coworkers. Include listings, search/filter, messaging between buyer and seller, and a moderation queue. | 
| Travel Approvals | Create an Extend app for tracking business travel requests and approvals. Include the request form, approval routing based on amount, a my-trips view for the requester, and a finance-team reporting view. | 
| Performance Check-Ins | Build an Extend app for quarterly performance check-ins between managers and their direct reports. Include scheduling, a structured check-in form, historical view of past check-ins, and an HR overview dashboard. | 
| Hackathon Portal | Generate an Extend app that serves as a hackathon project submission portal. Teams register, submit their project with description and links, judges score entries, and there's a public leaderboard. | 
| Contract Lifecycle | Build a contract lifecycle management app with stages (draft, review, signed, active, expired), document attachments, renewal reminders, and a search/filter view of all contracts. | 
| Employee Referrals | Create an Extend app for tracking employee referrals. Employees submit referrals, recruiters update status as candidates move through hiring, and there's a view showing referral bonus eligibility. | 
| Volunteer Tracking | Build an Extend app for managing internal volunteer and community service events. Include event listings, RSVPs, hours tracking, and a personal dashboard showing each employee's logged hours. | 
| Credit Card Reconciliation | Generate a full Extend app for managing corporate credit card reconciliation. Users see transactions pulled from an external source, categorize them, attach receipts, and submit the batch for manager approval. | 

</details>

## Debugging

<details>

<summary>View sample prompts for debugging issues with applications.</summary>

| Category | Prompt | 
| -------- | ------ | 
| Broken Preview | One of my PMDs is showing a blank screen in preview. Help me figure out what's broken and fix it. | 
| Runtime Error | I'm getting a runtime error when a user clicks a button on one of my pages. Walk me through debugging this and propose a fix. | 
| WQL Error | One of my WQL endpoints is returning a 500 error. Can you diagnose what's wrong and suggest a correction? | 
| Proactive Bug Scan | Are there any PMD script bugs or runtime errors currently in my app that a user might hit? Order them by severity. | 
| Script Reference Error | I think one of my onLoad scripts is referencing a variable that doesn't exist. Find it and fix it. | 
| Data Flow Trace | A user reports that a dropdown on one of my pages isn't populating. Trace the data flow from the endpoint to the widget and tell me where it's failing. | 
| GraphQL Null Value | My GraphQL query is returning null for a field even though the record has a value in Workday. Help me figure out why. | 
| Undefined Property | I'm seeing 'Cannot read property of undefined' errors in the console on one of my pages. Find the source and patch it. | 
| Security Misconfiguration | My security domain is blocking valid users from accessing the app. Review my security configuration and tell me what's misconfigured. | 
| REST Timeout | One of my REST outbound endpoints is timing out. Can you check my configuration and suggest how to make it more reliable? | 
| Timezone Formatting | A date field on one of my pages is displaying in UTC instead of the user's local timezone. Help me fix the formatting. | 
| Circular Reference | I keep getting a circular reference error when I save one of my PMDs. Identify the loop and resolve it. | 
| Pagination Bug | My pagination on one of my list pages is skipping records between pages. Debug the WQL and pagination logic. | 
| Stale References | Check all PMDs in this app for references to endpoints or BOs that no longer exist, and list them so I can clean them up. | 
| Conditional Visibility | The conditional visibility on a button in my app isn't working - it's always visible even for users who shouldn't see it. Debug the script logic. | 

</details>
