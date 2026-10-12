# 04. Responsive Layouts

## Definition

A responsive layout adapts to different screen sizes, densities and orientations without cutting off content or wasting space.

## Tools

| Tool | How it helps |
|---|---|
| `dp` for sizes, `sp` for text | Density-independent units |
| `layout_weight` + `0dp` | Proportional space instead of fixed pixels |
| `match_parent` / `wrap_content` | Flexible sizes |
| **`ScrollView` wrapper** | Content taller than the screen becomes scrollable (stops buttons being cut off) |
| **Resource qualifiers**: `layout-land/`, `layout-sw600dp/`, `values-sw600dp/` | A different layout/dimensions for landscape or tablets |
| `ConstraintLayout` for complex screens | Constraint-based, flatter hierarchy |

## Example problem: a login screen is cut off in landscape

The screen looks fine in portrait on a 6" phone, but in landscape the buttons are cut off. This happens because landscape height is much smaller, so a fixed-size vertical stack overflows.

### Fix 1: ScrollView

Wrap the layout in a `ScrollView` so the buttons can be scrolled into view.

```xml
<ScrollView android:layout_width="match_parent" android:layout_height="match_parent">
    <LinearLayout android:layout_width="match_parent" android:layout_height="wrap_content"
        android:orientation="vertical">
        <!-- EditTexts and buttons -->
    </LinearLayout>
</ScrollView>
```

A `ScrollView` can hold only **one direct child**, so put a single LinearLayout inside it.

### Fix 2: separate landscape layout

Create `res/layout-land/activity_login.xml` (same file name as the portrait one). Android loads it automatically in landscape. Use a horizontal LinearLayout with weights so the form and buttons sit side by side and fit the short height.

### Fix 3: weights

Give the form `0dp` + `layout_weight` so it shares the available space instead of using fixed `dp` sizes.

## Quick summary

- Cut off in landscape → ScrollView + `layout-land` + weights.
- Qualifier folders: `layout-land` (landscape), `layout-sw600dp` (smallest width ≥ 600dp, e.g. tablets).
