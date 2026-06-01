Make better use of horizontal desktop screen real estate with the new `columnLayout` tag. This responsive layout allows you to logically group independent but related information side-by-side by specifying how many columns to occupy within the body container.

**Consider these guidelines and recommendations:**

- `columnLayout` must be used as the top-level tag within the `body` container.

- It is available on both edit and view pages.

- The `columns[]` array supports 1 to 3 `layoutSection` columns that automatically size to fit the screen.

- If you only configure 1 column, it will automatically center and occupy ⅔ of the page width.

- The `columnLayout` tag is strictly forbidden inside a `pod`.

**Example:**
3 Column:
![3 Column|690x426, 75%](upload://3pfQEOs4RIc7lr2a3rXqiE1faTZ.jpeg)
2 Column:
![2 Column|690x415, 75%](upload://9lAxrAWc9XFzsImQEHNHycLvzuL.jpeg)
1 Column:
![1 Column|690x418, 75%](upload://aufr1RzA1n849JlTkH0qNtzlnPA.jpeg)

**When is This Happening?**
This is available, [date=2026-05-15 timezone="America/Los_Angeles"].

**What do I have to do?**
If you would like to use this feature, `columnLayout` must be used as the top-level tag within the `body` container.

For more information and example code, please see the [release notes](https://forum.developer.workday.com/c/news/release-notes/11) and [developer documentation](https://developer.workday.com/documentation).