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
<img width="824" height="556" alt="density-386353630f775b9860e62bf1c9f28f8f" src="https://github.com/user-attachments/assets/b999c23d-0e87-400d-ac72-af71060ab8f7" />

Alignment:  
<img width="1412" height="622" alt="image" src="https://github.com/user-attachments/assets/8fa74917-e87b-4bc3-964f-4367ca935f60" />

Horizontal Sizing:  
<img width="2368" height="910" alt="image" src="https://github.com/user-attachments/assets/ee805ce0-47e1-483d-a685-6405ce1efbcf" />

Label Position:  
<img width="1966" height="676" alt="image" src="https://github.com/user-attachments/assets/ed83d924-d751-4240-be90-54c1eb84208b" />

**When is This Happening?**  
This is available, May 15, 2026.

**What do I have to do?**  
To add these layout properties to an app, you need to include the new `layout` attribute within a `section` or `fieldSet` container tag in your PMD file.

For more information and example code, please see the [release notes](https://forum.developer.workday.com/c/news/release-notes/11) and [developer documentation](https://developer.workday.com/documentation).
__
