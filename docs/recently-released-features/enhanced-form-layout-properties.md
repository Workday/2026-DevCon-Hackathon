We have introduced a new `layout` property to the `section` and `fieldSet` tags, allowing you to apply granular UX controls to container tags to perfectly tailor your page structure.

The `layout` object introduces 4 new configuration attributes:

- `density`: Controls vertical spacing (`high`, `medium`, `low`).

- `alignment`: Controls horizontal positioning (`left`, `center`).

- `horizontalSizing`: Controls how widgets stretch (`stretch`, `fixed`).

- `labelPosition`: Controls label placement (`top`, `side_align_left`, `side_align_right`).

Nested `section` or `fieldSet` tags automatically inherit the layout attributes of their parent. You can easily override these by defining a specific layout attribute on the child container. 
Note: Form layout properties are **not allowed** within `grid` tags.

**Example:**

Density:
![density-386353630f775b9860e62bf1c9f28f8f|690x465, 50%](upload://ceREl2JHoTHWmLI8RU3pF1FVKRi.gif)

Alignment:
![Screenshot 2026-05-14 at 6.25.38 PM|690x303, 50%](upload://ncqva7ACvJw9oCwFuR7Naeu032t.png)

Horizontal Sizing:
![Screenshot 2026-05-14 at 6.26.27 PM|690x265, 75%](upload://iSmLUdi6mh7QloWUiS9dS9a8HGi.png)
Label Position:
![Screenshot 2026-05-14 at 6.27.40 PM|690x237, 75%](upload://33arU1VrleffzJjJsgs1zScaZOi.png)

**When is This Happening?**
This is available, [date=2026-05-15 timezone="America/Los_Angeles"].

**What do I have to do?**
To add these layout properties to an app, you need to include the new `layout` attribute within a `section` or `fieldSet` container tag in your PMD file.

For more information and example code, please see the [release notes](https://forum.developer.workday.com/c/news/release-notes/11) and [developer documentation](https://developer.workday.com/documentation).
__