# 02. Types of Android Layouts

## Definition

A layout is a `ViewGroup` that defines how its child views are positioned and sized on the screen.

| Layout | Defining characteristic | Typical use |
|---|---|---|
| **LinearLayout** | Children in a single row or column (`orientation`) | Simple forms, button rows, login screens |
| **ConstraintLayout** | Views positioned by constraints to the parent or siblings | Complex, flat, responsive screens |
| **FrameLayout** | Children stack on top of each other (last child on top) | A single fragment container, overlays, image + caption |
| **RelativeLayout** | Views positioned relative to siblings or the parent | Older layouts; mostly replaced by ConstraintLayout |
| **TableLayout** | Rows (`TableRow`) and columns like a table | Data entry forms with aligned columns |
| **GridLayout** | Children in rows and columns of a grid (supports spans) | Calculators, icon grids |
| **CoordinatorLayout** | A more powerful FrameLayout that coordinates child *behaviours* (Material components) | App bar + scrolling content + FAB with collapsing/scroll effects |

## Minimal XML for each layout

### LinearLayout

```xml
<!-- orientation decides row vs column -->
<LinearLayout android:layout_width="match_parent" android:layout_height="wrap_content"
    android:orientation="vertical">
    <TextView android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="A"/>
    <TextView android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="B"/>
</LinearLayout>
```

### ConstraintLayout

```xml
<!-- each view needs constraints (here: centred in parent) -->
<androidx.constraintlayout.widget.ConstraintLayout
    android:layout_width="match_parent" android:layout_height="match_parent">
    <Button android:layout_width="wrap_content" android:layout_height="wrap_content"
        android:text="OK"
        app:layout_constraintTop_toTopOf="parent" app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent" app:layout_constraintEnd_toEndOf="parent"/>
</androidx.constraintlayout.widget.ConstraintLayout>
```

### FrameLayout

```xml
<!-- children overlap; the TextView draws on top of the ImageView -->
<FrameLayout android:layout_width="match_parent" android:layout_height="200dp">
    <ImageView android:layout_width="match_parent" android:layout_height="match_parent"
        android:src="@drawable/photo"/>
    <TextView android:layout_width="wrap_content" android:layout_height="wrap_content"
        android:layout_gravity="bottom" android:text="Caption"/>
</FrameLayout>
```

### RelativeLayout

```xml
<!-- position relative to a sibling or parent -->
<RelativeLayout android:layout_width="match_parent" android:layout_height="wrap_content">
    <TextView android:id="@+id/title" android:layout_width="wrap_content"
        android:layout_height="wrap_content" android:text="Title"/>
    <Button android:layout_width="wrap_content" android:layout_height="wrap_content"
        android:layout_below="@id/title" android:layout_alignParentEnd="true" android:text="Go"/>
</RelativeLayout>
```

### TableLayout

```xml
<!-- TableRow = one row; cells = columns -->
<TableLayout android:layout_width="match_parent" android:layout_height="wrap_content">
    <TableRow>
        <TextView android:text="Name"/>
        <EditText android:layout_width="match_parent" android:layout_height="wrap_content"/>
    </TableRow>
</TableLayout>
```

### GridLayout

```xml
<!-- columnCount decides grid width -->
<GridLayout android:layout_width="match_parent" android:layout_height="wrap_content"
    android:columnCount="2">
    <Button android:text="1"/><Button android:text="2"/>
    <Button android:text="3"/><Button android:text="4"/>
</GridLayout>
```

### CoordinatorLayout

```xml
<!-- coordinates behaviours between children (e.g. FAB anchored, scrolling) -->
<androidx.coordinatorlayout.widget.CoordinatorLayout
    android:layout_width="match_parent" android:layout_height="match_parent">
    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:layout_width="wrap_content" android:layout_height="wrap_content"
        android:layout_gravity="bottom|end" android:src="@drawable/ic_add"/>
</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

## Common mistakes

- LinearLayout **does not overlap** children; FrameLayout **does**.
- Deeply nested LinearLayouts hurt performance, because each nesting level adds measure/layout passes. Prefer a flatter ConstraintLayout when the structure gets complicated. ([Android docs: optimize layout hierarchies](https://developer.android.com/develop/ui/views/layout/improving-layouts/optimizing-layouts))
- RelativeLayout still works, but ConstraintLayout is the modern recommendation.

## Quick summary

- Linear = row/column, Constraint = constraints, Frame = stack, Relative = relative to siblings, Table = rows/columns, Grid = grid cells, Coordinator = behaviours between Material views.
