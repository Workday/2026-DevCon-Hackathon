We are excited to release a highly requested enhancement to the `editWizard` widget: native draft saving!

Enabling the `enableSaving` attribute on an `editWizard` adds a native 'Save' button and triggers an automatic background save every 30 seconds. Users can securely pause their work and resume exactly where they left off, without losing progress.

**Consider these guidelines and recommendations:**

- The `enableSaving` attribute is a boolean. By default, this attribute is set to `false`.

- Draft data is preserved for 30 days securely by Workday.

- **Important:** Every tag within the `editWizard` must have a unique `id`. Changing these IDs after deployment will cause users to lose all previously saved progress.

- Automatic saving and Save for Later features are disabled when viewing in App Preview.

**Example:**  
<img width="1918" height="1490" alt="Screen Recording 2026-05-11 at 4 46 34 PM" src="https://github.com/user-attachments/assets/03e46db5-af5f-496c-a8c2-8fe699f12c1e" /> 
  
**When is This Happening?**  
This Update is available, May 15, 2026.

**What do I have to do?**  
If you would like to enable this feature in your editWizard, set the attribute `enableSaving` to true. 

For more information, please see the [release notes](https://forum.developer.workday.com/c/news/release-notes/11) and [developer documentation](https://developer.workday.com/documentation).
