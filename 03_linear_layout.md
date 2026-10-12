# 03. Linear Layout

## Definition
`LinearLayout` is a `ViewGroup` that arranges all its children **in a single direction: either horizontally (in a row) or vertically (in a column)**. The direction is set with the `android:orientation` attribute.

- `android:orientation="vertical"` - children are stacked top to bottom, one per row.
- `android:orientation="horizontal"` - children are placed left to right, in one row.
- If orientation is not specified, the default is **horizontal**.

## Diagram
```
 Vertical                    Horizontal
+-------------+             +------+------+------+
| Child 1     |             |  C1  |  C2  |  C3  |
+-------------+             +------+------+------+
| Child 2     |
+-------------+
| Child 3     |
+-------------+
```

## Example
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Login" />

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Username" />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Submit" />
</LinearLayout>
```

## Important Attributes
| Attribute | Description |
|---|---|
| `android:orientation` | `vertical` or `horizontal` |
| `android:layout_width` / `layout_height` | `match_parent`, `wrap_content` or exact `dp` |
| `android:layout_weight` | How the extra space is shared among children |
| `android:weightSum` | Total weight of the layout (optional) |
| `android:gravity` | Alignment of the **contents inside** the view/layout |
| `android:layout_gravity` | Alignment of **the view itself** within its parent |
| `android:padding` | Space inside the view |
| `android:layout_margin` | Space outside the view |
| `android:divider`, `android:showDividers` | Divider lines between children |
| `android:baselineAligned` | Aligns baselines of children in horizontal layout |

## gravity vs layout_gravity
- `gravity` is set on a **container** (or text view) and decides how its **children/content** are positioned.
- `layout_gravity` is set on a **child** and decides where **that child** sits inside its parent.

## Layout Weight
`layout_weight` distributes the remaining space in proportion to the weights assigned.
```xml
<LinearLayout android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:weightSum="3">

    <Button android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1" android:text="A" />
    <Button android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="2" android:text="B" />
</LinearLayout>
```
Button A takes 1/3 of the width and B takes 2/3. Set the dimension along the orientation to `0dp` when using weights, for efficiency and exact proportions.

## Nested Linear Layouts
A LinearLayout can contain other LinearLayouts (e.g. a vertical layout containing horizontal rows) to build grid-like UIs. Avoid deep nesting because each level adds measuring/drawing cost.

## Advantages
- Very simple and easy to understand.
- Weights make proportional sizing easy.
- Good for forms, toolbars, button bars and simple lists.

## Disadvantages
- Only one direction per layout; complex designs need nesting.
- Nested layouts and weights slow down rendering.
- Less flexible than ConstraintLayout.
