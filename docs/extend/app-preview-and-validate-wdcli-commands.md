## App

### preview
Opens a local preview of your Workday Extend app. Extend refreshes this preview automatically when you save changes to the underlying code.
`wdcli app preview charitableDonationsUserBuild_cqbvyp`
To stop your local app preview, use **CTRL + c** so you can restart again after making app changes.

### validate
Runs validation on your Extend app source code and returns any validation errors that may be found. 
`wdcli validate charitableDonationsUserBuild_cqbvyp`
**Note:** This command currently doesn't validate your Orchestrations or Agent file types. This command also doesn't save a version to App Hub, but simply runs validation and returns a success message or a table with the validation errors