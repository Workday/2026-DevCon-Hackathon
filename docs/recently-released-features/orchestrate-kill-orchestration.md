# Product Update: Orchestrate - Kill Orchestration

## What's New?

We are excited to introduce the Kill Orchestration capability for Workday Orchestrate. This new emergency operation gives developers the ability to forcefully and immediately terminate a running orchestration, thus bypassing all cancellation checkpoints when a critical incident demands it.

When an orchestration enters a problematic state where the ‘Cancel’ doesn’t suffice such as an infinite loop, runaway resource consumption, or a cascading failure that can't wait for the next graceful cancellation point, Kill Orchestration is your emergency stop. It halts the execution instantly and surfaces a ‘Killed’ status in the Orchestration Activity dashboard giving you the control to contain incidents and protect infrastructure.

https://vimeo.com/1188439706/1cd577c21c
 
## Benefits

* **Immediate termination for critical incidents**: Stop a runaway orchestration on the spot, without waiting for it to reach a developer-defined cancellation checkpoint.

* **Available via API and Dashboard**: Trigger a kill programmatically directly from the Orchestration Management Dashboard or via the Kill Orchestration API endpoint. 

## When is this happening?

Kill Orchestration is now Generally Available as of April 29th, 2026.

## How it Works

1. **Identify the orchestration** : In the Orchestration Activity dashboard, locate the orchestration you need to terminate. Note that the Kill action is only available for RUNNING and CANCEL_REQUESTED states.

2. **Initiate via Dashboard or API** : Click the Kill button. A confirmation dialog will appear with an explicit warning: this is an emergency-only operation; execution will be halted immediately and can cause data inconsistencies between systems. Confirm to proceed. Alternatively, you can invoke the Kill API endpoint with the parameters - appId, orchestrationName and orchestrationId

3. **Monitor** : The orchestration status immediately transitions to KILL_REQUESTED state, confirming the request has been accepted. Orchestrate will terminate all active processes associated with the orchestration run and update the final status once termination is complete.

4. **Reconcile affected systems** : After the kill completes, review what work the orchestration had already performed before it was stopped. Any writes to external systems, business processes or integrations or API calls triggered will not be rolled back. It is your responsibility as the developer to identify and manually reconcile the state of the integrations in the dependent systems.

## Use Cases

**Runaway orchestration containment:** An orchestration has entered an infinite loop and is consuming excessive compute. Cancel is not viable because the orchestration never reaches a safe checkpoint. Kill stops it immediately.

**Stuck CANCEL requested orchestrations:** An orchestration was sent a cancellation request but has not reached its cancellation point and is holding resources. Kill forces immediate termination.

**Cascading failure prevention:** A malfunctioning orchestration is triggering downstream side effects across multiple systems. Kill halts the orchestration before further damage propagates.

**Incident response:** During a production incident where an orchestration is threatening system stability, Kill gives operations teams a fast, decisive action rather than waiting for natural termination boundaries.

## Limitations

**Emergency use only:** Kill operation will leave the orchestration and any downstream systems it has already interacted with in an inconsistent state. Developer reconciliation is required after every kill.

**Best-effort termination:** Kill is a best-effort operation. Orchestrate will make every attempt to stop execution, but, does not guarantee success.

**Orchestration types:** Kill Orchestration action is available only for Workday Business Process, Integration System, and Asynchronous orchestrations.

## FAQ

**Q: What is the difference between Kill and Cancel?**

Cancel is a graceful termination. The developer defines specific checkpoints in the orchestration where cancellation is safe, and the runtime waits for the orchestration to reach one of those points before terminating, ensuring data consistency between systems.

Kill on the other hand is a forceful, immediate stop that bypasses all cancellation checkpoints. Cancel preserves consistency; Kill prioritizes speed at the cost of consistency. Use Cancel whenever possible and Kill only in genuine emergencies.

**Q: Will my orchestration stop instantly when I trigger a Kill?**

The kill operation is a best-effort one. Orchestrate will immediately transition the status to KILL_REQUESTED and begin terminating the active process. Once the kill operation is successful, it will be updated to KILLED state.

**Q: Can I kill an orchestration that is waiting on a callback (async wait state)?**

If the orchestration is in a RUNNING state (including while checkpointed in an asynchronous call), it is eligible for kill. If it has already been sent a cancellation request and is in CANCEL_REQUESTED state, it is also eligible. Any queued resume operations will also be terminated as part of the kill.