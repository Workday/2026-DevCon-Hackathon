The new `multiStepLayout` tag allows you to partition a single PMD page into sequential steps, creating a modern, carousel-style navigation experience.

Unlike the `editWizard`, which requires separate PMD files for each step, this layout allows you to build rich, interactive forms directly within a single file.

**Consider these guidelines and recommendations:**

- `multiStepLayout` must be used as the top-level tag within the `body` container.

- It is available on both edit and view pages.

- You can specify up to 10 `layoutSection` tags in the `steps[]` array.

- The `multiStepLayout` tag is strictly forbidden inside a `pod`.

**Example:**
<img width="1364" height="1002" alt="Screen Recording 2026-05-14 at 6 01 09 PM" src="https://github.com/user-attachments/assets/13602b0a-aa77-4dae-ae5a-6f4fdb8c75d0" />


**When is This Happening?**  
This is available, May 15, 2026.

**What do I have to do?**  
If you would like to use this feature, `multiStepLayout` must be used as the top-level tag within the `body` container.

For more information and example code, please see the [release notes](https://forum.developer.workday.com/c/news/release-notes/11) and [developer documentation](https://developer.workday.com/documentation).
