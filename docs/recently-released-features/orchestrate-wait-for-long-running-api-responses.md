# Product Update: Orchestrate - Wait for Long Running API Responses

# What's New?
![image](upload://d5XMmI8XdsSPsn0VMDrxJlMxIHi.jpeg)

We are excited to introduce the Long Running Request capability for the [Send Workday API Request](https://developer.workday.com/documentation/GUID-43efaf69-686f-4372-a0a7-5238ead1a2a7?q=send%20workday) component. This feature allows orchestrations to wait for Workday REST and SOAP API requests to complete beyond the standard 5-minute response timeout, supporting up to 1 hour for REST API requests and up to 6 hours for SOAP API requests.

When enabled, the orchestration creates a checkpoint and waits for the API response before resuming. This is a key enabler for the 48-hour runtime capability, as each long-running request creates a new orchestration process, resetting the runtime clock and allowing complex, data-intensive workflows to execute across extended timeframes.

# When is this happening?

Wait for Long Running API Responses is Generally Available Friday January 23rd 2026.

# Benefits

* Extended API Processing: Handle Workday API operations that take longer than 5 minutes to complete, such as large data retrievals or complex business process operations.
* 48-Hour Runtime Enabler: Each long-running request step creates a checkpoint, enabling workflows to run for up to 48 hours by chaining multiple API calls within loops.
* Batch Processing Support: Process large datasets via API calls within loops without hitting timeout limits. Ideal for annual or quarterly batch operations that process hundreds of thousands of records.
* Simplified Architecture: Remove the need for complex workarounds or technology switches when handling long-running batch API operations.

# When is this happening?

Wait for Long Running API Responses is Generally Available January 23rd 2026.

# How it Works

To enable Wait for Long Running API Responses:

![long_running|690x388](upload://tUuDCJ7WzC59vpQSaPFIVBVAAXx.gif)

1. Open an Orchestration in the Orchestration Builder
2. Add or select a Send Workday API Request step
3. Enable Advanced Mode
4. Navigate to the Settings tab
5. Toggle "Enable Wait for Long Running Response" to on
6. Configure your Response Timeout (up to 1 hour for REST, up to 6 hours for SOAP)

When enabled:

* The orchestration creates a checkpoint when the API request is made
* The orchestration goes to sleep while waiting for the response
* When the response is ready, the orchestration resumes with a new process
* All step outputs (response body, headers, status code) are available as normal
* If used in a branch or a loop Concurrency is disabled in that loop or branch. If used in a nested loop concurrency is disabled in all outer loops.

# What happens if I do nothing?

Wait for Long Running API Responses is an optional feature that requires explicit opt-in via Advanced Mode. Your existing orchestrations will continue to work without any changes, using the standard 5-minute response timeout.

# Use Cases

This feature enables several powerful patterns:

* Large Data Exports: Retrieve large datasets from Workday APIs that take longer than 5 minutes to generate and respond.
* Batch API Operations: Process thousands of records via API calls within loops, with each iteration checkpointing to extend total runtime
* Complex Business Operations: Invoke Workday APIs that trigger long-running business processes and wait for completion

**Limitations**

Available only in Integration System Orchestration, Async and Business Process Orchestration template types

* REST API requests support up to 1 hour wait time or the API specific lower limit, whichever is less.
* SOAP API requests support up to 6 hours wait time or the API specific lower limit, whichever is less.

When used in loops, pipelining is suppressed and iterations execute sequentially.

Response Timeout must be set to more than 5 minutes for the feature to have effect. Avoid enabling this feature if you know your API requests will return in less than 5 minutes, you’re simply making the overall Workflow Orchestration take longer.

# FAQ

**Q: What SKU is this feature available under?**

Wait for Long Running API Responses is available for all Orchestrate customers. An Extend SKU is not required.

**Q: Why shouldn't I enable this feature with a low Response Timeout value?**

Response Timeout must be set to more than 5 minutes for the feature to have effect. Avoid enabling this feature if you know your API requests will return in less than 5 minutes, you're simply making the overall workflow take longer. The feature creates a checkpoint and puts the orchestration to sleep, which adds overhead. Only use this when you genuinely need to wait for long-running API operations. Using it for fast API calls will work, it’s just a waste.

**Q: Does this affect performance?**

When enabled inside a loop, iterations process sequentially rather than concurrently. This trades speed for reliability - position this as "correctness over speed" for edge cases that require guaranteed completion.

**Q: When should I enable this feature?**

Only enable this feature when you have API requests that may exceed the standard 5-minute timeout. The vast majority of API requests complete well within this limit. Use this for specific edge cases like large data exports or batch processing scenarios.

**Q: Can I use this inside a loop?**

Yes. This is a powerful pattern for batch processing. Each iteration creates a checkpoint, allowing you to process large volumes of data across extended timeframes. Note that loop iterations will execute sequentially when this feature is enabled.

**Q: How do I know if my API request needs this feature?**

If your orchestration is failing with timeout errors on Send Workday API Request steps, or if you're processing large datasets that may take longer than 5 minutes per API call, consider enabling Wait for Long Running API Responses.

**Q: Does this work for all REST APIs https://developer.workday.com/rest-api-explorer APIs?**

This feature will work for any tenanted API that is exposed through DevSite for App Development. Other APIs such as Prism, ML etc are not supported.

**Q: Does this work for all SOAP APIs https://developer.workday.com/soap-api-explorer APIs?**

This feature will work for any tenanted API that is exposed through DevSite for App Development. If it's not listed as a Public API for your tenant, this feature won't work for the API.