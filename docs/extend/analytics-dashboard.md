# Concept: Analytics Dashboard

## Introduction

The **Analytics** dashboard provides statistics and logs that developers and administrators can use to gain insight into their apps. Workday aggregates data across all apps in all environments for the dashboard.

The **Analytics** dashboard includes **Metrics** and **Logs** tabs.

## Metrics Overview

You can use the Metrics tab to view:

* **Account Metrics**: a summary of high-level metrics that measure the overall performance of your account.
* **App Summary**: a list of all apps in your account with high-level performance metrics for each app.
* **App Metrics**: detailed statistics for a single app.

## Account Metrics

The **Account Metrics** section includes:

* **Total Requests**: the total number of requests per second for all apps in the account.
* **Total Users**: the number of users who have ever access any app in the account.
* **Total Error Rate**: the total number of errors as a percentage of all actions for all apps in the account.
* **Total Usage**: the total amount of data, in megabytes, used by app apps.
* Environment and date range drop-downs. You can filter results by environment (examples: Production, Sandbox, Development) and by date range.

## All App Summary

The **All App Summary** table includes 1 row for each app. Each row includes a menu button on the right-hand side from which you can access logs, an app overview, and the app in App Builder. The table provides these columns:

* **App ID**: the ID of the app. Click the name to open the **App Metrics** page for the app.
* **API Error Rate**: the error rate for the app during the selected time period. Click the error rate number to open the **Logs** page for the app and view a list of errors..
* **API Request Rate**: the number of API requests per second (RPS) for all API calls made by the app during the selected time period.
* **Page Views**: the number of page view requests made for the app during the selected time period.
* **Page Error Rate**: the number of page errors returned by the app during the selected time period.
* **Data Usage**: the total amount of data used for the app.

## App Metrics

The **App Metrics** page provides detailed data for a single app that you've selected. It provides app-level statistics for total errors, API calls and errors, and page views and errors. The page provides these tabs:

* **Pages** provides details about the performance of each app Page's performance. Hyperlinks provide further details about each statistic, and a right-hand row menu provides further access to each Page in the app.
* **API** provides details about the performance of each API called by the app. Hyperlinks provide further details about each statistic, and a right-hand row menu provides further access to each API used by the app.

## Logs Overview

You can use the **Logs** tab to monitor, troubleshoot, and debug your apps.

Logs are available for these Workday services:

* API Gateway.
* Workday Orchestrate.
* Presentation Components (available to Extend app developers but not to integration app developers).

The Logs tab provides these filter options for narrowing down the list of log files:

* **Search**: enter search terms to locate occurrences of the search terms. Use double-quotes around all search terms.
* **Filters**: define custom filters for frequently-used searches during a single session. You can't save filters for use during a later session.
* **Log View Presets**: filter results by log type (*Access*, *App*, *Audit*, *Remote*).

These logs are available for your app:

|Log Type|Description|
| --- | --- |
|Access|Provides information about client requests.|
|App|Provides debugging, informational, warning, and error messages related to an app request.|
|Audit|Enables administrators to track changes to code, including: <ul><li>What code changes occurred.</li><li>When code changes happened.</li><li>Why developers implemented code changes.</li><li>Who promoted code from development into Production.</li></ul>|
|Remote|Provides information about requests to an external service.|

The **Logs** tab ingests log data with a 1-minute latency. You can update log entries by clicking **Refresh** on the dashboard.

**Note:** Workday permanently deletes log files that are older than 30 days. If you need to keep them for longer than 30 days, users with an Admin role can download logs from the **Logs** tab of the **Analytics** dashboard. When you download the logs, any filters applied in the view aren't included in the downloaded file.

Logs are only available for tenants assigned to your company. Any tenant assigned directly to a user won't display logs on the dashboard.

## Logs Filter, Search, and Download Capabilities

On the **Logs** tab of the **Analytics** dashboard, we provide:

* A filter that enables you to query specific log fields.
* A search bar that enables you to search the indexed log fields for specific values.
* A date picker that enables you to locate app events and logs by date.
* A button that enables you to download logs.

Filters enable you to view more granular data for each log. To add a filter, you must select a field and an operator, and then add a value if necessary.

Example: On the **Logs** tab, you can add a filter with these values to view data usage for all apps in Production environments:

* **Field**: *wd_env*
* **Operator**: *is*
* **Value**: `PROD`

To save a filter query, select **Preset** > **Save New Preset**.

To filter by date range, you can use the *wd_date_time* field and the *is between* operator along with the start and end date and time.

Time units include:

|Unit|Description|
| --- | --- |
|`M`|Months|
|`d`|Days|
|`h`|Hours|
|`m`|Minutes|
|`s`|Seconds|

# Reference: Application Logs

## Log Fields

You can view these log fields for your apps on the **Logs** tab of the **Analytics** dashboard or from an individual app's home page.

**Note:** Workday permanently deletes log files that are older than 30 days. If you need to keep them for longer than 30 days, users with an Admin role can download logs from the **Logs** tab of the **Analytics** dashboard. When you download the logs, any filters applied in the view aren't included in the downloaded file.

The Logs tab provides these filter options for narrowing down the list of log files:

* **Search**: enter search terms to locate occurrences of the search terms. Use double-quotes around all search terms.
* **Filters**: define custom filters for frequently-used searches during a single session. You can't save filters for use during a later session.
* **Log View Presets**: filter results by log type. Options vary for Analytics Dashboard and App Builder:
  * Analytics Dashboard: *Access*, *App*, *Audit*, *Remote*.
  * App Builder: *Debug Logs*, *API Logs*.

You can view these log fields for your apps:

|Log Field|Description|
| --- | --- |
|**wd_api_client_id**|The ID that's generated when you register your API client.|
|**wd_api_type**|The functional area endpoint for the API request.<br>Examples: `aos`, `wql` |
|**wd_api_version**|The API version in use.|
|**wd_app_id**|The ID for the app.|
|**wd_app_component**|The component of the app. Possible values:<ul><li>`abo` – Business Objects.</li><li>`attributeStore` – Attribute Store.</li><li>`ps` – Presentation Components.</li><li>`workflows` – Workday Orchestrate.</li></ul>|
|**wd_billed_request_bytes**|The actual API request bytes for which Workday bills you based on your usage of the resize image feature on the fileUploader widget.|
|**wd_app_promotion_status**|The status of your app in the promotion lifecycle. Possible values:<ul><li>`RELEASE_CANDIDATE`</li><li>`RELEASED`</li>`SANDBOX_RELEASE`</li></ul>|
|**wd_category**|The category of app log information. <br>For Workday Orchestrate, the value `console` returns custom log messages from the log component.<br>For Presentation Components, these are the possible values:<ul><li>`console` – Returns custom log messages from a PMD script that calls the console logging methods. The `wd_level` depends on whether the PMD script called the debug, error, info, or warn method.</li><li>`mobile` – Returns log entries related to presentation components of a custom app when used in the Workday mobile app.</li><li>`pageError` – Returns page exception messages in the wd_message field. The `wd_level` is ERROR.</li><li>`pageError.script` – Returns PMD script exception messages in the wd_message field. The `wd_level` is ERROR.</li><li>`promotion` – Returns information about the promotion of an app during the app lifecycle.</li><li>`responseError` - Returns details about an endpoint that encountered an error, including the HTTP status code, request URL, method, and request headers.</li>`scriptUsage` – Returns CPU and memory usage information about a script execution. The `wd_level` is INFO.</li><li>`sourceUpload` – Returns information about the uploading of source code for an app to App Hub.</li></ul>|
|**wd_common_request_id**|A random, unique ID that tracks an incoming HTTP request. This ID doesn't change throughout all downstream requests.<br>Example: "VPS|5a438101-0f47-4cfa-a3a5-a84c13c3252b". For requests made in App Preview, **wd_common_request_id** starts with `WCPUI-APP-PREVIEW`.|
|**wd_date_time**|The date and time of the API request.|
|**wd_env**|The tenant environment. Possible values:<ul><li>`IMPL` – Implementation tenant.</li><li>`SANDBOX` – Sandbox tenant.</li><li>`PROD` – Production tenant.</li><li>`WCPDEV` – Workday Cloud Platform development tenant.</li></ul><br>When the app runs in App Preview with no tenant connection, the value is always `WCPDEV`.|
|**wd_gateway_url**|The Workday API gateway for the tenant. When the app runs in App Preview without tenant connection, this value is always blank.|
|**wd_hostname**|The host URL. Example: `api.workday.com`<br>When the app runs in App Preview, this value is populated in the `remote` log, but is always blank in the `access` log.|
|**wd_http_method**|The HTTP method for the API request.|
|**wd_http_protocol**|The HTTP version for the API request.|
|**wd_level**|A label that categorizes log entries by level of urgency for troubleshooting and debugging. Possible values:<ul><li>`ERROR` – Categorizes logs for serious but nonfatal issues in the app.</li><li>`WARN` – Categorizes logs for potentially harmful behavior with the app.</li><li>`INFO` – Categorizes logs that provide information on the normal behavior of the app.</li><li>`DEBUG` – Categorizes logs with more granular information that you can use to debug the app.</li></ul>|
|**wd_log_type**|The type of logs returned:<ul><li>`access` – Log entries with information about client requests.</li><ul>`app` – Log entries with debugging, informational, warning, or error messages related to an app request. For log entries related to Presentation Components, see the log categories in **wd_category**.</li><li>`audit` – Log entries that enable administrators to track changes to code.</li><li>`remote` – Log entries with information about requests to a remote service.</li></ul>|
|**wd_message**|Provides details on events for your app.<br>For the Workday Orchestrate service, this field enables you to view log information about your orchestrations.|
|**wd_page_id**|The ID of a PMD file.|
|**wd_path**|The URL path for the API request.<br>Example: `/common/v1/workers/me/organizations`|
|**wd_referer**|The address of the previous page linked to the requested resource.|
|**wd_request_bytes**|The size of the payload of the API request in bytes.|
|**wd_request_content_type**|The content type of the HTTP request.<br>Example: `application/json`|
|**wd_resource**|The resource of the RESTful API type.<br>Example: `workers`|
|**wd_response_bytes**|The size of the payload of the API response in bytes.|
|**wd_response_code**|The API response code.|
|**wd_response_content_disposition**|The content disposition header of the HTTP response.<br>Example: `attachment; filename=Alerts.json`<br>When the app runs in App Preview, this value is always blank.|
|**wd_response_content_type**|The content type of the HTTP response.|
|**wd_response_time**|The total roundtrip time for the API request and response.<br>When the app runs in App Preview without tenant connection, this value is always 0.|
|**wd_service**|The Workday service for the logs.<br>Available Workday services:<ul><li>API Gateway (`api-gateway`).</li><li>Workday Orchestrate (`orchestrate`).</li></ul><br>**Note:** An additional service, Presentation Components (`vps` or `wcpui`) is available to Extend app developers but not to integration app developers.|
|**wd_session_id**|The unique ID for a session.|
|**wd_sku**|The Workday product associated with the log.|
|**wd_subresource**|The subresource of the RESTful API type.<br>Example: `organizations`|
|**wd_tenant**|The tenant name.<br>When the app runs in App Preview without tenant connection, this value is always blank.|
|**wd_tenant_alias**|The tenant alias name. You provide the tenant alias name during the Onboarding process.<br>When the app runs in App Preview without tenant connection, this value is always blank.|
|**wd_usage_bytes**|The size of the request and response in bytes if the usage ID is present.|
|**wd_usage_id**|The ID used by the **Usage** tab to highlight your app traffic.|
|**wd_user_agent**|The browser type and version in use.|
|**wd_username**|The developer username.|