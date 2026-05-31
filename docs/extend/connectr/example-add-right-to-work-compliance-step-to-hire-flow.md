## Scenario

A company wants to add a "Right to Work" form to the **Hire** flow, but only for workers hired in the UK. You need to follow the configuration.

1. An **Extend** Developer builds an edit task that captures Right to Work document details. This task includes a blocking validation that prevents submission if the document type is not selected.
2. A Workday Admin uses the **Configure ConnectR** task to add the **Extend** task as a section in the Hire flow.
3. The section is positioned after the worker's personal details step.

## Results
Conditional display based on location is handled at the Extend task level.