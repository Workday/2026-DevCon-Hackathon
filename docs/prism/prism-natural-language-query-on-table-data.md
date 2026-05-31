# Prism Natural Language Query (NLQ) on Table Data

## Overview

**Prism Natural Language Query (NLQ) on Table Data** is an intelligent, generative AI assistant integrated directly within Workday Prism Analytics. Designed to streamline data validation and data preparation workflows, this feature allows developers, implementers, and consultants to query tables using plain conversational language.

Instead of creating temporary reports, exporting datasets to external tools like Excel, or enabling full tables for heavy analysis, you can instantly ask about these questions right where your data lives: 

* Data integrity  
* Fetch summaries  
* Data definition

## **Accessing the AI Chat Assistant**

The Prism NLQ on Table Data feature is embedded within the **Data Catalog** task in the tenant. To initiate a session:

1. Navigate to the **Data Catalog** task.  
2. Search or locate the target **Prism Table** you want to analyze.  
3. *Optional Tip:* Sorting tables by number of rows in descending order helps isolate datasets by size to plan your data scoping strategy.  
4. Right-click the table row, or left-click the **Ellipsis (...)** icon to trigger the **Related Actions / Context Menu**.  
5. Select **View...** to navigate to the **View Table Details** page.  
6. Click the **AI Sparkle Icon** located right next to the *Quick Actions* button to launch the **AI Chat Assistant** dialog.

## **Technical Capabilities & Core Use Cases**

### **1\. Live Data Slicing & Filter Creation**

For extensive datasets, you can toggle the conversation dynamically to create data slices that can persist across your sessions.

**Workflow Example:** 

1\. Ask a question on a 1M+ row table: `"Can you give me distinct values for [fieldA]?"` 

2\. The AI will prompt you to scope the dataset. 

3\. Toggle to **Filter Mode** directly in the prompt text window. 

4\. Input your syntax using targeted field names (e.g., `[fieldA] = xyz` or utilizing a specific date field). 

5\. The assistant creates and renders the live filter block right inside the chat window, allowing you to resume prompting (e.g., `"How many nulls exist for fieldB within this window?"`).

### **2\. Multi-Shot Refinement & Context Maintenance**

The Prism NLQ on Table Data feature retains conversational context across a sequence of prompts. You can establish a baseline query and continuously narrow or redirect your questions without restating previous criteria.

* **Initial Question:** `"Can you give me distinct values for [fieldA]"`  
  * *Expected Output:* The AI provides the distinct value list alongside a structural explanation.  
* **Follow-up Question:** `"How many rows exist for those criteria?"`  
  * *Expected Output:* The assistant accurately references the context of the previous prompt to provide a precise row count.

## **Best Practices**

* **Use Explicit Bracket Notation:** When referencing specific column names or metrics in your table, wrap them in square brackets (e.g., `[Worker_ID]`, `[Transaction_Date]`). Typing `[` inside the prompt box will automatically pull up an auto-complete drop-down menu of available dataset fields.  
* **Iterate Layer by Layer:** If you are hunting for complex anomalies or cross-referencing multi-layered criteria, use short, iterative prompts. Build a baseline data slice first, and then apply deeper descriptive questions.