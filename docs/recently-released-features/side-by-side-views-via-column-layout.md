Make better use of horizontal desktop screen real estate with the new `columnLayout` tag. This responsive layout allows you to logically group independent but related information side-by-side by specifying how many columns to occupy within the body container.

**Consider these guidelines and recommendations:**

- `columnLayout` must be used as the top-level tag within the `body` container.

- It is available on both edit and view pages.

- The `columns[]` array supports 1 to 3 `layoutSection` columns that automatically size to fit the screen.

- If you only configure 1 column, it will automatically center and occupy ⅔ of the page width.

- The `columnLayout` tag is strictly forbidden inside a `pod`.

**Example:**  
3 Column:  
<img width="1321" height="817" alt="image" src="https://github.com/user-attachments/assets/298b3b13-6674-42a7-a300-9769c92409f1" />

2 Column:  
<img width="1321" height="795" alt="image" src="https://github.com/user-attachments/assets/9676c81b-0615-4034-8598-ef9b1c9ec559" />

1 Column:  
<img width="1321" height="802" alt="image" src="https://github.com/user-attachments/assets/e6c41b7b-215f-4b98-b8f3-0a7c6862a032" />

**When is This Happening?**  
This is available, May 15, 2026.

**What do I have to do?**  
If you would like to use this feature, `columnLayout` must be used as the top-level tag within the `body` container.

For more information and example code, please see the [release notes](https://forum.developer.workday.com/c/news/release-notes/11) and [developer documentation](https://developer.workday.com/documentation).
