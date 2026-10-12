# 14. RecyclerView

## Definition
`RecyclerView` is a flexible and efficient `ViewGroup` for displaying **large lists or grids of data**. Instead of creating a view for every item, it **recycles (reuses)** the views that scroll off the screen to display new items, which saves memory and improves scrolling performance.

Package: `androidx.recyclerview.widget.RecyclerView`

## Core Components
| Component | Role |
|---|---|
| **RecyclerView** | The container that displays the items |
| **Adapter** | Binds the data to the views; creates ViewHolders |
| **ViewHolder** | Holds references to the views of one list item (avoids repeated `findViewById`) |
| **LayoutManager** | Decides how items are positioned (list, grid, staggered) |
| **ItemAnimator** | Animations for add/remove/move |
| **ItemDecoration** | Dividers/spacing between items |

## How Recycling Works
```
 +--------+   scrolls off the top
 | Item 1 | ------------------+
 | Item 2 |                   |  ViewHolder is reused ("recycled")
 | Item 3 |                   v
 | Item 4 |            +-------------+
 +--------+            | Item 5 data | bound to the same view
                       +-------------+
```
Only the visible items (plus a few extra) exist in memory.

## Adapter Methods
| Method | Purpose |
|---|---|
| `onCreateViewHolder()` | Inflates the item layout and creates a ViewHolder (called only as many times as needed) |
| `onBindViewHolder()` | Binds the data at a position to the ViewHolder (called often) |
| `getItemCount()` | Returns the total number of items |

## LayoutManagers
- `LinearLayoutManager` - vertical or horizontal list
- `GridLayoutManager` - grid with a number of columns
- `StaggeredGridLayoutManager` - staggered grid with uneven item sizes

## Steps to Use
1. Add RecyclerView to the layout XML.
```xml
<androidx.recyclerview.widget.RecyclerView
    android:id="@+id/recyclerView"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```
2. Create the item layout (`item_row.xml`).
3. Create the Adapter and ViewHolder.
```java
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.MyViewHolder> {
    private List<String> data;

    public MyAdapter(List<String> data) { this.data = data; }

    static class MyViewHolder extends RecyclerView.ViewHolder {
        TextView textView;
        MyViewHolder(View itemView) {
            super(itemView);
            textView = itemView.findViewById(R.id.textView);
        }
    }

    @NonNull @Override
    public MyViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View v = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.item_row, parent, false);
        return new MyViewHolder(v);
    }

    @Override
    public void onBindViewHolder(@NonNull MyViewHolder holder, int position) {
        holder.textView.setText(data.get(position));
    }

    @Override
    public int getItemCount() { return data.size(); }
}
```
4. Set it up in the Activity.
```java
RecyclerView rv = findViewById(R.id.recyclerView);
rv.setLayoutManager(new LinearLayoutManager(this));
rv.setAdapter(new MyAdapter(list));
```

## Advantages
- Efficient memory use through view recycling.
- Mandatory ViewHolder pattern.
- Supports list, grid and staggered layouts, and horizontal scrolling.
- Built-in animations and item decorations.
- `DiffUtil` / `ListAdapter` updates only the changed items.
- Item click handling is flexible (set in the adapter).

## Dependency (Gradle)
```
implementation "androidx.recyclerview:recyclerview:1.3.2"
```
