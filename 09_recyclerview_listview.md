# 09. RecyclerView and ListView

## RecyclerView

### Definition

`RecyclerView` is a `ViewGroup` for efficiently displaying large lists or grids. It **recycles** item views: when an item scrolls off screen, its view is **reused** for a new item scrolling in, instead of being destroyed and created again.

### Components

| Component | Role |
|---|---|
| **RecyclerView** | The container (`ViewGroup`) added to your XML layout |
| **Item layout** (XML) | How **one** row/card looks |
| **ViewHolder** | Holds references to an item's views (`findViewById` is done once) |
| **Adapter** | Creates ViewHolders and **binds** data to them |
| **LayoutManager** | **Arranges** items: `LinearLayoutManager` (list), `GridLayoutManager` (grid), `StaggeredGridLayoutManager` (uneven grid) |
| **ItemAnimator** | Animates add/remove/move of items |

Source: [Create dynamic lists with RecyclerView](https://developer.android.com/develop/ui/views/layout/recyclerview).

### How it works: create → bind → recycle

```
 data list --> Adapter --onCreateViewHolder()--> new ViewHolder (only when needed)
                   |
                   +--onBindViewHolder(holder, position)--> fills holder with data
 item scrolls off screen --> its ViewHolder goes to a pool --> reused (re-bound) for a new item
```

- `onCreateViewHolder()`: creates a ViewHolder and inflates the item layout; it **does not** fill in data.
- `onBindViewHolder()`: puts the data for `position` into the holder's views. It is called for new **and** recycled holders.
- `getItemCount()`: how many items there are.

**Why a ViewHolder?** `findViewById()` is relatively expensive. The holder stores the view references once, so binding a recycled row only calls `setText(...)` and similar.

### Minimal implementation

**XML:**

```xml
<androidx.recyclerview.widget.RecyclerView
    android:id="@+id/recyclerView"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```

**Gradle dependency:** `implementation "androidx.recyclerview:recyclerview:<latest version>"`

**Activity: attach the LayoutManager and Adapter**

```java
RecyclerView recyclerView = findViewById(R.id.recyclerView);
recyclerView.setLayoutManager(new LinearLayoutManager(this));
recyclerView.setAdapter(new MyAdapter(names));   // names is an ArrayList<String>
```

For a grid instead: `new GridLayoutManager(this, 2)` (2 columns).

> Note: the official docs' Java sample shows Kotlin-style `recyclerView.layoutManager = ...`. In Java you must call `setLayoutManager(...)`.

**Adapter + ViewHolder**

```java
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder> {
    private final ArrayList<String> names;
    public MyAdapter(ArrayList<String> names) { this.names = names; }

    public static class ViewHolder extends RecyclerView.ViewHolder {
        TextView tvName;
        public ViewHolder(View itemView) {
            super(itemView);
            tvName = itemView.findViewById(R.id.tvName);
        }
    }

    @Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.item_name, parent, false);
        return new ViewHolder(view);
    }

    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {
        holder.tvName.setText(names.get(position));
    }

    @Override
    public int getItemCount() { return names.size(); }
}
```

**Item layout `item_name.xml`:** a layout containing a `TextView` with `android:id="@+id/tvName"`.

This code depends on: `R.layout.item_name`, `R.id.tvName`, the list `names`, and `R.id.recyclerView`.

### Updating data

```java
names.add("Sara");
adapter.notifyItemInserted(names.size() - 1);   // precise: animates one item
// adapter.notifyDataSetChanged();               // coarse: refreshes everything, no item animation
```

### Different layouts for different items (e.g. chat sent/received)

```java
@Override public int getItemViewType(int position) {
    return messages.get(position).isSent() ? 0 : 1;
}
// onCreateViewHolder(parent, viewType): inflate sent_layout if 0, received_layout if 1
```

## ListView (older)

### Definition

`ListView` is an older `ViewGroup` that shows a **vertically scrolling** list of items supplied by an adapter.

### Minimal setup

```java
ListView listView = findViewById(R.id.listView);
ArrayAdapter<String> adapter =
        new ArrayAdapter<>(this, android.R.layout.simple_list_item_1, names);
listView.setAdapter(adapter);
listView.setOnItemClickListener((parent, view, position, id) -> {
    // handle click on names.get(position)
});
```

## RecyclerView vs ListView

| Point | RecyclerView | ListView |
|---|---|---|
| **View recycling** | Built-in and **enforced** through ViewHolder | Reuses rows through `convertView`, but you must write the code (and ViewHolder is optional) |
| **ViewHolder** | **Mandatory** (`RecyclerView.ViewHolder`) | **Optional** (manual pattern) |
| **Layout flexibility** | `LayoutManager`: list, grid, staggered, horizontal, custom | Vertical list only |
| **Animations** | Built-in `ItemAnimator` (add/remove/move) | No built-in item animations |
| **Updating items** | Fine-grained `notifyItemInserted/Removed/Changed` | Mostly `notifyDataSetChanged()` |
| **Complexity** | More code (Adapter + ViewHolder + LayoutManager) | Simpler for basic lists |
| **Performance on large lists** | Better | Fine for small lists |
| **Use case** | Chats, feeds, grids, large/dynamic lists | Small, simple, static lists |

## Example scenario: a chat app with 10,000 messages and different layouts for sent and received

**RecyclerView is the better choice because:**

1. It creates only enough views for the screen (plus a few spares) and **reuses** them, instead of 10,000 views. This saves memory and keeps scrolling smooth.
2. A **ViewHolder** avoids repeated `findViewById()` calls on each bind.
3. `getItemViewType()` supports **two layouts** cleanly (one for sent, one for received).
4. `notifyItemInserted()` and `ItemAnimator` make adding a new message efficient and animated.

Nuance: ListView can also show multiple row types, but it recycles less cleanly and has no built-in animations.

## Common mistakes

- "ListView doesn't recycle" is wrong: it recycles via `convertView`, just less strictly.
- The component that decides **how items are arranged** is the `LayoutManager`, **not** the Adapter. The Adapter decides **what data goes in each item**.
- `onBindViewHolder()` is the method that binds data. `onCreateViewHolder()` only inflates the layout and builds the holder.

## Quick summary

- RecyclerView = Adapter + ViewHolder + LayoutManager (+ ItemAnimator).
- Create (`onCreateViewHolder`), bind (`onBindViewHolder`), count (`getItemCount`).
- `setLayoutManager()` then `setAdapter()`.
- ListView = old, vertical only, optional ViewHolder, no animations.
