Studio Builder on the Workday Developer Site runs Studio integrations on Integration Runtime 2 (IR2) and does not support Integration Runtime (IR1). Integrations built with the desktop Workday Studio target IR1 and so must be migrated to IR2. Migration happens automatically the first time you deploy a IR1 integration in Studio Builder.

This topic explains why the migration progress exists, what happens when you deploy, and the statuses you will see while it runs.

### Advantages of IR2

* **Modern Java**: IR2 runs on Java 17. IR1 is tied to Java 8.
* **Modern transport security**: IR2 supports modern SFTP host-key algorithms such as `rsa-sha2-256` and `rsa-sha2-512`. IR1 is limited to older algorithms that external vendors are deprecating.
* **Future-proofing**: All new runtime, performance work, and platform improvements will ship on IR2.
* **Easy migration**: Integrations without custom Java or with custom code that matches the IR2 API are migrated automatically and your assembly is updated automatically.

### How Migration Works

Migration is triggered, if required, when you click **Deploy** in Studio Builder. We recommend you import your existing IR1 integration and deploy it immediately, before making any changes. The assembly you subsequently edit in Studio Builder will already be IR2-compatible.

Migration happens in the background in 1 or 2 modes, as outlined in this table:

|**Mode**|**Applies to**|**Outcome**|
| --- | --- | --- |
|**Automatic**|Integration with no custom code or Integrations whose custom Java / XSLT / MVEL expressions all have direct equivalents in the IR2 API.|The integration is rewritten to be compatible with IR2 and starts running on IR2 immediately. No action required from you.|
|**Partial** (manual input required)|Integrations whose custom Java or expressions don't all map cleanly to the IR2 API. The migrator translates everything it can; you update the remainder manually.|The integration is partially translated. Open the collection in Studio Builder, use the **Download Archive** and **Upload Archive** features to address the unmigrated code, then redeploy. For more information, see: [Local Java Code for Studio Builder Integrations](/docs/studio-builder/local-java-code-for-studio-builder-integrations.md).|

> **Note:** If a migrated integration misbehaves on IR2, you can roll it back to IR1. On your tenant, access the **Cloud Collection Migration Report**, locate the migrated collection, and use the rollback action to restore the original IR1 version. Workday keeps the original IR1 assembly as a backup specifically for this purpose.

The **Deploy** button is inactive during migration. When the migration completes, the collection is now a IR2 integration. You can edit, redeploy, and run it from Studio Builder as normal.

If the migration fails, your integration is left untouched on IR1 and the status changes to Failed. You'll remain unable to deploy it from Studio Builder until it's made compatible with IR2.

### Migration Statuses and Troubleshooting

This table summarizes migration statuses and provides guidance on what to do in the event of difficulties:

|**Status**|**What it means**|**What to do**|
| --- | --- | --- |
|**Not started**|The integration hasn't been deployed yet, or Workday hasn't picked it up.|Click **Deploy** to start the migration.|
|**In progress**|Workday is analysing or rewriting the assembly. Most migrations finish within a few minutes. Those with Java code take longer than those without.|Wait. The status refreshes automatically. You can close the tab and check later.|
|**Completed**|The integration is now on IR2 and is editable in Studio Builder.|Continue working with the integration as normal.|
|**Failed**|The migration didn't complete. Your integration is unchanged and will not be deployed from Studio Builder.|Redeploy the collection to try again. If the migration fails a second time, the integration likely contains custom code that needs manual input. Open the collection in Studio Builder, address incompatible code, then redeploy. For more information, see: [Local Java Code for Studio Builder Integrations](/docs/studio-builder/local-java-code-for-studio-builder-integrations.md).|