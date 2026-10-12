# 15. RecyclerView vs ListView

Both display scrollable collections of items, but **RecyclerView** is the modern, more flexible and efficient replacement of **ListView**.

## Comparison

| Feature | ListView | RecyclerView |
|---|---|---|
| Introduced | API level 1 (old) | Android 5.0 / support library (newer) |
| ViewHolder pattern | **Optional** (must be implemented manually for performance) | **Mandatory** and built in |
| View recycling | Basic (via `convertView`) | Efficient, built-in recycling |
| Layouts | Vertical list only | Linear (vertical/horizontal), grid, staggered grid via LayoutManager |
| Animations | Not supported by default | `ItemAnimator` for add/remove/move |
| Item decoration (dividers) | `divider` attribute only | Flexible `ItemDecoration` |
| Item click listener | Built-in `setOnItemClickListener()` | Not built in; implemented in the adapter/ViewHolder |
| Partial updates | Mostly `notifyDataSetChanged()` (refreshes everything) | `notifyItemInserted/Removed/Changed()` and `DiffUtil` |
| Horizontal scrolling | Not directly | Yes |
| Performance with large data | Lower | Higher |
| Customisation | Limited | Highly customisable |
| Header/Footer | Built-in support | Must be implemented via view types |
| Adapter | `ArrayAdapter`, `BaseAdapter` | `RecyclerView.Adapter` |
| Status | Legacy, rarely used in new code | Recommended standard |

## Summary
- **ListView**: simple and easy for small, plain vertical lists, but limited and less efficient.
- **RecyclerView**: more code to set up, but better performance, flexibility, animations and layout choices; the **preferred** choice for modern apps.

## Code Difference at a Glance
```java
// ListView
ListView lv = findViewById(R.id.listView);
lv.setAdapter(new ArrayAdapter<>(this, android.R.layout.simple_list_item_1, data));

// RecyclerView
RecyclerView rv = findViewById(R.id.recyclerView);
rv.setLayoutManager(new LinearLayoutManager(this));
rv.setAdapter(new MyAdapter(data));
```
