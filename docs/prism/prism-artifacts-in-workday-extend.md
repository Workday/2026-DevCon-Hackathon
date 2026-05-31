# Prism Artifacts in Workday Extend

## Overview

You can easily package Workday Prism Analytics Artifacts as Extend app components within the App Builder, App Hub, and Developer Site framework, streamlining custom app development and deployment workflows. This feature enables you to directly add Prism artifacts from the tenant into your Extend and Built on Workday apps. Instead of managing analytic structures and app development completely independently, you can instantly surface Prism artifacts right where your Extend app ecosystem lives.

## Accessing and Packaging Prism Artifacts

The Prism Artifacts integration is embedded within developer tenant tasks and the App Hub framework.

You must deploy the Extend app to the Development tenant before exporting the Prism artifacts.

To package Prism Artifacts and export them into an Extend app:

1. Sign in the development tenant.
2. Access the **Export Prism Artifacts** task .
3. In the **App Reference ID** field, select the target Extend app.
4. In the **Tags** column, select tags that filter the **Prism Tables**, **Datasets**, and **Data Change Tasks (DCTs)** you want to export.The package will automatically include the artifact dependencies (Example: Tables, datasets from lineage).
5. Select the **Prism Artifacts** you want to include in the package.
6. Click **OK**.
7. Ensure that you select the **Trigger New App Hub Build** checkbox.
8. Select the **Confirm** checkbox.
9. Click **Submit**.The confirmation page includes a link to the Developer Site.
10. Open your Extend app in App Builder. The **Prism Artifacts** component displays the list of exported artifacts. Query components in your Extend app such as a WQL query can now reference the Prism artifacts.

## Technical Capabilities and Core Use Cases

**Unified App Hub Packaging**

For extensive enterprise application footprints, you can package your analytics backend alongside your transactional frontend.

Developers can seamlessly bundle foundational Prism Tables and Datasets directly into their Extend app within the App Hub, ensuring that critical data pipelines migrate consistently with the application lifecycle.

Extend apps can query and display data from Prism Tables to drive UI experiences and application logic. Some examples:

* **UI Display via WQL**: Query Prism datasources directly using WQL to display real-time analytics within Extend Page components.
* **Workflow Decisions**: Use WQL query results within an Orchestration step to drive dynamic routing or conditional logic in a Business Process.
* **Data Extraction (RaaS/WQL)**: Extract Prism data securely via RaaS or WQL endpoints for use by external systems or Extend apps.
* **Embedded Reporting**: Use Prism data sources to power custom reports, dashboards, and Discovery Boards embedded directly into the Extend app UI.

Extend apps can use Workday Orchestration to trigger data ingestion and orchestrate data pipelines into Prism tables.

* **File Based Ingestion**: Move file based data sources into Prism using the WBucket v2 API or File Containers combined with a Data Change Task (DCT).
* **Source to Table Ingestion**: Use the "Execute Data Change" or [Prism Analytics Tables Materialize API](/docs/prism/prism-analytics-tables-materialize-api.md) to bring data from non-native Prism sources (like SFTP, external connections, or Custom Reports) into Prism using App Sources.

**Developer Site Visibility and Component Management**

The Extend developer platform retains full visibility of your bundled analytic structures across your build lifecycle.

Once components are attached to your unique App ID, a dedicated section appears within the developer site interface:

* **App Builder View:** Toggle dynamically into App Builder to view a new **Prism Components** panel that reflects your bundled Prism artifacts.
* **App Overview View:** Check the broader App Overview page to review your attached Prism component.

## Best Practices

**Use Precise App IDs:** When executing the **Export Prism artifacts** task, ensure you have verified your exact target App ID so that components deploy to the correct Extend app.

**Leverage Structured Tagging:** Before initiating a packaging pipeline, apply clean, standardized tags to your Prism Tables, Datasets, and DCTs. This ensures complex multi-layered analytic models can be efficiently filtered and aggregated into the app.