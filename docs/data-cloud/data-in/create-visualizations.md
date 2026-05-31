# Create Visualizations

## Context

Build Discovery Board visualizations that use your external tables and the derived dataset you created in the Explore and Validate External Tables step ([Snowflake](/docs/data-cloud/data-in/set-up-data-in-with-snowflake.md#task-3-explore-and-validate-external-tables-11)) ([Salesforce](/docs/data-cloud/data-in/set-up-data-in-with-salesforce.md#task-3-create-zero-copy-connection-to-salesforce-5)).

In this procedure you’ll:

* Create a Discovery Board.  
* Add a visualization that uses only external data.  
* Add a visualization that uses your blended, published dataset.  
* Save the board so others can explore the results.

This procedure applies to these external partners: Snowflake and Salesforce.

## Steps

1. **Create a Discovery Board.**

   a. From the Workday header, select your profile picture and then select **Drive**.

   b.  Navigate to a folder.

   c.  Select **\+ New \> Discovery Board**.

   d.  Enter a name such as ZeroCopy\_Analysis\_.

   e.  Click **Create**.

   A new Discovery Board opens with a single blank visualization.

2. **Add a visualization using only external data.**

   a.  In the Discovery Board, open the Data Sources panel.

   b.  In the Search Data Sources box, search for the external data source associated with your external catalog.

   c.  Select this external data source.

   d.  Open the Builder Panel to view available fields.

   e.  To the right of the Search Fields box, select the down arrow next to the default visualization type and choose a chart that fits your data (for example, a bar chart, column chart, or donut chart).

   f.  Drag one or more numeric measures from the external source into the appropriate blocks (for example, Y-Axis or Angle).

   g.  Drag a dimension field (such as a category, organization, or level) into the X-Axis, Color, or Filter blocks to segment the measure.  
      This creates a visualization that uses only the external data queried via Zero Copy.
  
   h. Click **Save**.

3. **Add a visualization using the blended dataset.**

   a. On the Discovery Board, select **Add Viz** to add another visualization.

   b. In the **Data Sources** panel, select the Back arrow and search for the published dataset you created in Task 3\.

   c. Select that dataset (for example, a dataset that blends Workday and external data).

   d. Open the **Builder Panel** to display the dataset fields.

   e. Choose a chart type that helps compare your key measures across categories or over time (for example, a column chart).

   f. Drag your primary measure (for example, a total or sum) into the Y-Axis block.

   g. Drag a relevant dimension (for example, a category, period, or organization field) into the X-Axis block.

   h. Optionally add more dimensions to Color or Filter to allow users to segment the results.

   This creates a visualization that showcases the blended dataset you prepared in the Explore and Validate External Tables step ([Snowflake](/docs/data-cloud/data-in/set-up-data-in-with-snowflake.md#task-4-explore-and-validate-external-tables-12)) ([Salesforce](/docs/data-cloud/data-in/set-up-data-in-with-salesforce.md#task-5-explore-and-validate-external-tables-12)).

   i. Click **Save**.

4. **Review and refine the board.**

   Review the Discovery Board:

   * Confirm that the first visualization is based only on external data.  
   * Confirm that the second visualization uses your published, blended dataset.  
   * Check that labels, titles, and legends make it clear what each visualization represents.

## Next Steps

You can:

* Add filters (such as time period, organization, or population).  
* Adjust formatting and layout so the board is easy to interpret.  
* Share the board according to your hackathon needs.