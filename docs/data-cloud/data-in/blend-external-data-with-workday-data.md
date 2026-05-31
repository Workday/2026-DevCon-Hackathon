# Blend External Data with Workday Data

## Context

Use Prism Data Management to transform and aggregate data from your external tables (and optionally Workday tables) into an analysis-ready dataset.

In this procedure, you’ll:

* Create a derived dataset based on one or more existing tables.  
* Join external data with Workday data, if needed.  
* Add calculated fields and reshape the data so it is easier to analyze.  
* Publish the dataset so it can be used in reports and visualizations.

This procedure applies to these external partners: Snowflake and Salesforce.

## Steps

1. **Create a Derived Dataset.**

   a. Access the **Data Catalog** report.

   b. Select **Create \> Derived Dataset**.

   c. Select one or more source tables or datasets that contain the measures and dimensions you want to analyze.

      Example: a Workday dataset plus an external table.

   d. Enter a meaningful **Dataset Name** that clearly describes the content and purpose of the dataset.

   e. Select **Save** to open the dataset in the **Edit Dataset Transformations** page.

2. **Join Workday and external data.**

   a.  On the **Edit Dataset Transformations** page, in the top panel, select **Add Stage \> Join**.

   b.  In the **Primary Pipeline**, keep your main table or dataset (for example, Workday data).

   c.  In the **Select a Pipeline** dropdown list, choose **Add Another Pipeline** and select the external table from your External Catalog.

   d.  Use business keys (Example: worker IDs, transaction IDs, cost center IDs) to define the join.

   e.  Optionally, use Copilot suggestions to identify high-confidence field pairs and then confirm the ones you want to use.

   f.  Choose an appropriate **Join Type** (example: Inner Join when you only want records that exist in both sources).

   g.  In **Select Fields**, include only the fields that are relevant to your analysis.

3. **Create calculated fields.**  
   To prepare your measures for analysis:  

   a.  On the **Edit Dataset Transformations** page, select **Add Field**.  
   b.  Use the Prism Copilot Expression Builder to:

   * Standardize numeric measures (for example, convert text to numeric, or normalize to a common currency or unit).

   * Derive business metrics (such as totals, ratios, flags, or categories).

   * Clean or format text and date fields into more report-friendly values.

    c. Give each calculated field a clear, business-friendly name.

   d. Select **Save** in the upper-right corner to save the updated dataset definition.

4. **Reshape and aggregate the data.**

   Depending on your reporting needs, you may want to reshape or pre-aggregate the data:

    * Use stages such as Unpivot or Pivot to restructure multiple related measures into a format that is easier to slice and compare.  
   * Use Aggregate or Grouping-style transformations to pre-roll up data at a useful grain (for example, by worker, cost center, time period, or product).

   After making changes, use the preview pane to confirm that:

    * The key dimensions (for example, organizations, dates, entities) appear as expected.  
   * The measures are in the correct format and at the correct level of detail.

5. **Publish the dataset.**  
   When you are satisfied with the structure and contents of the dataset:

   a. From the **View Dataset Details** page, select **Quick Actions \> Publish**.

   b. Select **Submit** to start the publish operation.

   c. Use the **Refresh** icon to update the status until the publishing operation is complete and shows as Published.

## Result

The published dataset is now ready to be used as a data source for visualizations and dashboards.
