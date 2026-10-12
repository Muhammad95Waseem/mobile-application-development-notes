# 03. LinearLayout

## Definition

`LinearLayout` is a `ViewGroup` that arranges its children in a **single direction**, either horizontally or vertically, as set by `android:orientation`.

## Parent-level vs child-level attributes

| Attribute | Set on | Meaning |
|---|---|---|
| `android:orientation` | **Parent** LinearLayout | `horizontal` (side by side) or `vertical` (stacked). Default is `horizontal`. |
| `android:gravity` | **Parent** (or a view for its *own content*) | How the **contents** are aligned inside it. On a LinearLayout: where its children sit inside the layout. On a TextView: where the text sits in the TextView. |
| `android:weightSum` | **Parent** | Total weight to distribute against. If omitted, it equals the sum of the children's weights. |
| `android:layout_width` / `layout_height` | **Child** | Size: `match_parent`, `wrap_content`, or fixed (`dp`). |
| `android:layout_gravity` | **Child** | Where the **child itself** sits inside its parent. |
| `android:layout_weight` | **Child** | Share of the **leftover** space along the orientation axis. |

**Easy way to remember:** `gravity` = "where do my contents go?" (inside me). `layout_gravity` = "where do I go?" (inside my parent).

## Size values

- `match_parent`: fill the parent.
- `wrap_content`: just big enough for the content.
- `0dp`: "ignore my own size; the weight decides" (used together with `layout_weight`).

## Basic example

```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal">

    <Button android:layout_width="0dp" android:layout_height="wrap_content"
        android:layout_weight="1" android:text="Cancel"/>
    <Button android:layout_width="0dp" android:layout_height="wrap_content"
        android:layout_weight="1" android:text="OK"/>
</LinearLayout>
```

Each button gets 50% of the width.

## How layout_weight works

The system first measures every child at its requested size. Then it takes the **leftover space** = parent size − sum of the children's sizes, and splits it in the ratio of the weights, **adding** each share to that child's own size.

### Case 1: `0dp` + weights (clean proportions)

Parent width 300dp, weights 1 : 2.

- Child sizes are 0dp, so leftover = 300dp.
- A gets 300 × 1/3 = **100dp**, B gets 300 × 2/3 = **200dp**.

### Case 2: equal weights, different final widths

Parent 300dp, `wrap_content` children of 60dp and 140dp, weights 1 : 1.

- Leftover = 300 − (60 + 140) = 100dp, so each gets +50dp.
- A = 60 + 50 = **110dp**, B = 140 + 50 = **190dp**.
- Equal weights do not mean equal widths: the *extra* space is split equally, not the total.

### Case 3 (classic trap): `match_parent` + weights

Parent 300dp, both children `match_parent`, weights 1 : 2.

- Sum of requested sizes = 600dp, so leftover = **−300dp** (negative).
- Negative space is also split by weight: A = 300 − 300 × 1/3 = **200dp**, B = 300 − 300 × 2/3 = **100dp**.
- The child with the **bigger weight ends up smaller**.
- Fix: use `0dp` for the weighted dimension.

### weightSum

Parent width 400dp, `weightSum="4"`, two `0dp` children with weights 1 and 1: each gets 400 × 1/4 = 100dp, and 200dp stays empty.

### Why 0dp?

It gives clean ratios (Case 1) and avoids measuring children at their natural size first, which is slightly more efficient. Use `0dp` on the **width** in a horizontal layout and on the **height** in a vertical layout, i.e. the axis the weight acts on.

## gravity vs layout_gravity in code

```xml
<!-- Parent: centre all children horizontally and vertically -->
<LinearLayout android:layout_width="match_parent" android:layout_height="match_parent"
    android:orientation="vertical" android:gravity="center">
    <Button android:layout_width="wrap_content" android:layout_height="wrap_content"
        android:text="A"/>
    <!-- Child: this button alone goes to the right edge (works in a vertical layout) -->
    <Button android:layout_width="wrap_content" android:layout_height="wrap_content"
        android:layout_gravity="end" android:text="B"/>
</LinearLayout>
```

## Common mistakes

1. In a **vertical** LinearLayout, `layout_gravity` only affects **horizontal** positions (`start`, `end`, `center_horizontal`); vertical positioning is controlled by weights and order. The reverse holds in a horizontal LinearLayout.
2. `gravity` on the parent affects its children; `gravity` on a TextView/Button affects its **text**.
3. The default `orientation` is **horizontal**. If no `orientation` is given, children are laid out side by side.
4. A child with `layout_weight` and `match_parent` causes Case 3.
5. Weight only distributes space along the orientation axis.

## Nested LinearLayouts

Each nested level makes Android measure the subtree more than once; weights inside nested layouts are especially costly. Flatten the hierarchy or switch to ConstraintLayout.

## Quick summary

- `orientation` = direction (parent). `gravity` = contents (parent). `layout_gravity` = self (child). `layout_weight` = share of leftover (child).
- Weighted dimension → `0dp`. Leftover = parent − children's sizes.
