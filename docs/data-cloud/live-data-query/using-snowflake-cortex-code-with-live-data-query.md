Cortex Code is an AI coding assistant built into Snowflake Workspaces. Since your LDQ notebook runs in a Workspace, you can use Cortex Code to accelerate your work with Workday data — from writing queries to analyzing results and building visualizations.

## Prerequisites

To use Cortex Code, your Snowflake role must have the following database roles granted:
```
GRANT DATABASE ROLE SNOWFLAKE.COPILOT_USER
TO ROLE WORKDAY_LDQ_TEST_ROLE;

GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER
TO ROLE WORKDAY_LDQ_TEST_ROLE;
```
## How to access

Click the Cortex Code icon in the bottom-right corner of your Workspace. A chat panel opens where you can type natural language prompts.

## Example prompts

Once you have a working LDQ connection (Steps 1-3), try these prompts in Cortex Code:

### Explore Workday data

"Write a Python cell that queries workday_core.public.worker for all active workers and loads the results into a pandas DataFrame called workers_df."

### Analyze and visualize

"Using workers_df, create a bar chart showing headcount by management level, sorted descending.”

“Summarize the workers_df DataFrame — show column types, null counts, and basic statistics."

### Transform and join with Snowflake data

"Write code to save workers_df to a Snowflake table called WORKDAY_LDQ_TEST.LIVEDATA.WORKERS_SNAPSHOT, then join it with HR_DB.PUBLIC.DEPARTMENT_BUDGETS on department name."

### Debug and explain

"Why is my LDQ query returning zero rows? Here is the error..." (paste the error message) (Highlight a code cell)

"Explain what this code does."

## Example questions about your Workday data

Once you have Workday data loaded into a DataFrame (e.g., workers_df from Step 3.3), you can askCortex Code questions about the data itself:

### Data exploration

"What columns are in workers_df? Which ones look like they contain PII?"

"How many unique job titles are there? Show me the top 20 by headcount."

"Are there any workers with missing manager IDs?"

### Business insights

"What's the average tenure by department? Which department has the highest turnover?"

"Show me the distribution of workers hired in the last 12 months, broken down by quarter."

"Compare headcount across management levels — are we top-heavy?"

### Data quality

"Check workers_df for duplicate worker IDs."

"Which columns have more than 10% null values?"

"Find any rows where hire_date is in the future."

## Tips

* Cortex Code is aware of your notebook's context — it can see your existing cells and variables.
* It works best after data is loaded into a DataFrame. It cannot call the Workday LDQ connector directly, but it can generate the Python code for you to run.
* Use the Fix button in the results panel if a cell fails — Cortex Code will suggest corrections.
* Type @ in the chat to reference Snowflake tables or views as additional context for your prompts.