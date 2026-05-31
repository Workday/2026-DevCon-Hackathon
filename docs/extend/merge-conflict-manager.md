# Concept: Merge Conflict Manager

When you update the same application in both your active App Builder session and in App Hub, App Builder's Merge Conflict Manager detects and resolves updates and conflicts.

When you save your app to App Hub or open a new App Builder session, App Builder detects if the Extend components of the app you are editing have also been updated in App Hub and notifies you if updates are available.

Merge Conflict Manager in App Builder applies to Extend components only. Changes made to Orchestrations in App Hub won't trigger a merge conflict notification. You should save those changes and edit them separately in Orchestration Builder. When editing from a Local Folder, App Builder defers to the local disk’s latest changes, so Merge Conflict Manager isn't available with Local Disk Sync enabled.

# Resolve Merge Conflicts in App Builder

### Context

App Builder's Merge Conflict Manager detects and resolves updates and conflicts when the same application has been updated in both your active App Builder session and in App Hub. When you save your app to App Hub or open a new App Builder session, App Builder checks whether the Extend components of the app you are editing have also been updated in App Hub. If updates are available, App Builder notifies you.

### Steps

* **Saving to App Hub with updates available in App Hub**: If updates are available in App Hub while you are saving to App Hub, App Hub displays a **Cannot Save to App Hub** message. You must either apply those updates to your session, or overwrite App Hub with your current session changes before saving to App Hub.
* **Opening a new session with updates available in App Hub**: If updates are available in App Hub while opening a new session in App Builder, App Builder displays an **Updates Available in App Hub** notification in the top App Builder toolbar. This notification may take a few seconds to display after the session loads. You can apply updates at any time during your session by clicking on the notification button. You must either apply updates or overwrite App Hub before saving to App Hub.**Note:** The **Updates Available in App Hub** notification won't appear in the top App Builder toolbar in the middle of your session development. It will only show up if you reload the browser in App Builder or open a new App Builder session.
* **Viewing changes in App Hub**: To review what was changed in App Hub before applying changes, select **Updates Available in App Hub**, then click **View Changes**. App Hub displays a read-only code comparison between the latest App Hub version and the previous version of the app.
* **Applying updates from App Hub**: Click **Apply Updates** to incorporate the latest changes from App Hub into your session automatically.
* **Resolving merge conflicts**: Merge conflicts will occur after you click **Apply Updates** if the same lines of code within the components were modified in both App Hub and your session. In that case, Extend will prompt you to resolve merge conflicts. The **Apply** button remains disabled until you resolve all conflicts across every affected component.For each conflict, you may choose how to resolve it:
  * **Use App Hub**: Overwrites the conflicted code with the latest App Hub version.
  * **Use Your Session**: Overwrites the conflicted code with your current session.
  * **Use Both**: Overwrites the conflicted code with a combination of the code from both App Hub and your session.
* **Completing merge conflict resolution**: Once all conflicts are resolved, click **Apply** to finalize your merge conflict resolutions.

### Next Steps

After applying updates and resolving any merge conflicts, continue editing your app in your session as usual. Applying updates via Merge Conflict Manager does not modify your app in App Hub unless you selected *Overwrite App Hub* in the **Cannot Save to App Hub** window.