# 02. Layout Types in Android

## What is a Layout?
A **layout** defines the structure of the user interface in an app. It is a container (a `ViewGroup`) that holds child `View` objects (buttons, text, images) and decides **how they are arranged** on the screen.

- `View` - a basic UI element (TextView, Button, ImageView).
- `ViewGroup` - an invisible container that holds Views and other ViewGroups. Layouts are ViewGroups.

Layouts can be declared in two ways:
1. **In XML** (preferred) - stored in `res/layout/`.
2. **In Java/Kotlin code** at runtime.

## Common Layout Types

### 1. LinearLayout
Arranges children in a **single row (horizontal) or a single column (vertical)**. Supports weights. (See `03_linear_layout.md`.)

### 2. RelativeLayout
Positions children **relative to each other or to the parent**, e.g. "below the title", "align to the right of the parent".
```xml
<RelativeLayout ...>
    <TextView android:id="@+id/title" ... />
    <Button android:layout_below="@id/title"
            android:layout_alignParentRight="true" ... />
</RelativeLayout>
```
Good for flat hierarchies. Largely replaced by ConstraintLayout.

### 3. ConstraintLayout
Positions children using **constraints** (connections) to other views, the parent, or guidelines. Gives a flat view hierarchy, so it is faster for complex UIs, and is well supported by the Layout Editor. It is the **default layout** in modern Android Studio templates.

### 4. FrameLayout
Designed to hold **a single child**; if there are multiple children they are **stacked on top of each other** (the last one on top). Often used as a container for Fragments.

### 5. TableLayout
Arranges children in **rows and columns** like an HTML table, using `TableRow`.

### 6. GridLayout
Places children in a **grid of rows and columns** with flexible spanning (`rowSpan`, `columnSpan`).

### 7. CoordinatorLayout
A powerful FrameLayout used with Material components, for behaviours like collapsing toolbars and FAB movement on scroll.

### 8. AbsoluteLayout (deprecated)
Positions children using exact x/y coordinates. Deprecated because it does not adapt to different screen sizes.

## Comparison

| Layout | Arrangement | Best for |
|---|---|---|
| LinearLayout | One row or column | Simple lists of items, forms |
| RelativeLayout | Relative to siblings/parent | Medium-complexity screens |
| ConstraintLayout | Constraints between views | Complex, responsive, flat UIs |
| FrameLayout | Stacked children | One view / overlays / fragment holder |
| TableLayout | Rows and columns | Tabular data |
| GridLayout | Grid cells | Calculators, galleries |

## Common Attributes (all layouts)
| Attribute | Meaning |
|---|---|
| `layout_width`, `layout_height` | `match_parent`, `wrap_content`, or a fixed value in `dp` |
| `padding` | Space inside the view, between border and content |
| `layout_margin` | Space outside the view, between it and others |
| `id` | Unique identifier, `@+id/name` |
| `background` | Colour or drawable |
