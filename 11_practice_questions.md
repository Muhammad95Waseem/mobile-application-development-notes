# 11. Practice Questions

Try these without looking at the notes first. Answers are at the bottom.

## Multiple choice

**Q1.** A horizontal LinearLayout is 300dp wide. Two children both use `layout_width="match_parent"` with `layout_weight` 1 and 2. What are their final widths (first, second)?
a) 100dp, 200dp  b) 150dp, 150dp  c) 200dp, 100dp  d) 300dp, 300dp

**Q2.** A 300dp-wide horizontal LinearLayout holds two `wrap_content` buttons, naturally 60dp and 140dp wide, both with `layout_weight="1"`. Final widths?
a) 150dp, 150dp  b) 110dp, 190dp  c) 100dp, 200dp  d) 60dp, 140dp

**Q3.** Which attribute positions a **single child** within its parent LinearLayout?
a) `android:gravity`  b) `android:orientation`  c) `android:layout_gravity`  d) `android:weightSum`

**Q4.** Activity A starts a full-screen Activity B in the same app. Which sequence is correct?
a) A.onPause → B.onCreate → B.onStart → B.onResume → A.onStop
b) A.onStop → B.onCreate → B.onResume → A.onPause
c) B.onCreate → A.onPause → A.onStop → B.onResume
d) A.onDestroy → B.onCreate → B.onStart → B.onResume

**Q5.** A semi-transparent activity opens over yours, so yours is still partly visible. Which callback does **your** activity receive?
a) `onStop()`  b) `onPause()`  c) `onDestroy()`  d) `onRestart()`

**Q6.** Which statement about explicit intents is true?
a) They always require an intent filter on the target.
b) They cannot carry extras.
c) They are delivered to the named target regardless of its intent filters.
d) They can only start Services.

**Q7.** An implicit intent is sent but no installed app matches, and the code has no try/catch. What happens?
a) It silently returns null  b) The Play Store opens automatically  c) Your own Activity starts instead  d) `ActivityNotFoundException` is thrown

**Q8.** For an Activity to receive implicit intents sent with `startActivity()`, its intent filter must include:
a) `android.intent.action.MAIN`  b) `android.intent.category.LAUNCHER`  c) `android.intent.category.DEFAULT`  d) `android.intent.category.BROWSABLE`

**Q9.** A service was started with `startService()` and an Activity is also bound to it. Another component calls `stopService()`. What happens?
a) It keeps running until the last client unbinds, then is destroyed  b) It is destroyed immediately  c) `onStop()` is called on the service  d) Binding is cancelled and the service restarts

**Q10.** A foreground service must:
a) show an AlertDialog  b) call `startForeground()` with a Notification  c) extend `Activity`  d) run in a separate process

**Q11.** RecyclerView needs a brand-new ViewHolder (nothing to recycle). Which Adapter method is called?
a) `onBindViewHolder()`  b) `getItemCount()`  c) `onCreateViewHolder()`  d) `getItemViewType()`

**Q12.** A chat RecyclerView needs different layouts for sent and received messages. Which Adapter method should be overridden?
a) `getItemViewType()`  b) `getItemId()` only  c) `onViewRecycled()`  d) `notifyDataSetChanged()`

**Q13.** Which `scaleType` fills the ImageView completely, keeps the aspect ratio and crops the overflow?
a) `fitXY`  b) `fitCenter`  c) `centerInside`  d) `centerCrop`

**Q14.** Which belongs to the **Java API Framework** layer?
a) SQLite  b) ART  c) Activity Manager  d) Binder driver

**Q15.** Which statement about ListView is true?
a) It cannot recycle rows at all.
b) It has a built-in ItemAnimator.
c) It recycles rows via `convertView`, but the ViewHolder pattern is optional.
d) It supports staggered grid layouts.

## Short answers

**S1.** Define a Service. Differentiate Started and Bound services on **three** points.

**S2.** Why does RecyclerView use a ViewHolder, and what does it save?

**S3.** Differentiate `android:gravity` and `android:layout_gravity` with a one-line example each.

**S4.** Is an intent filter required for an explicit intent? Explain how the system resolves an implicit intent.

**S5.** Differentiate `onPause()` and `onStop()`, naming one thing you would do in each.

## Scenarios

**SC1.** A horizontal LinearLayout is 360dp wide with two TextViews, both `layout_width="match_parent"`, weights 1 and 3. Calculate the final widths and explain the result. How do you fix it to get a 1:3 split?

**SC2.** A camera preview should run only while your activity is in front and focused. Which callbacks should start and stop it, and why? What changes if the preview should keep running when the activity is visible but unfocused (multi-window)?

**SC3.** Your app opens a PDF with another app using an implicit intent. A user has no PDF viewer installed. What happens, and how do you handle it?

**SC4.** A step-counting app must keep counting when the screen is off, and a "Progress" screen must call methods on the same service only while it is visible. Which service types are involved and why?

**SC5.** A square profile photo (500×500) appears stretched inside an ImageView of 200dp × 100dp with `scaleType="fitXY"`. Why, and what are two fixes?

## Diagrams

**D1.** Draw and label the five Android architecture layers. Place these items in the correct layer: Activity Manager, SQLite, ART, camera driver, camera HAL module.

**D2.** Draw the Service lifecycle for both a started and a bound service. Mark where `onStartCommand()` and `onBind()` occur.

## Code

**C1.** Write the XML for a horizontal LinearLayout (width `match_parent`) with three Buttons: the first takes 50% of the width, the others 25% each.

**C2.** An `EditText` (`etUrl`) holds a web address. Write the button-click code that opens it in a browser and handles the case where nothing can open it.

**C3.** A RecyclerView shows an `ArrayList<String> names`. Assuming the ViewHolder has a `TextView tvName` and the item layout is `R.layout.item_name`, write the three Adapter methods, and the two lines that attach the adapter in the Activity.

---

# Answers

## Multiple choice

| Q | Answer | Explanation |
|---|---|---|
| 1 | **c** 200, 100 | Leftover = 300 − 600 = −300, shared 1:2 → A = 300 − 100 = 200, B = 300 − 200 = 100. With `match_parent`, the bigger weight shrinks more. |
| 2 | **b** 110, 190 | Leftover = 300 − 200 = 100, +50 each: 60+50, 140+50. Equal weights don't mean equal widths. |
| 3 | **c** | `layout_gravity` = the child positions itself; `gravity` positions the **contents** of a view. |
| 4 | **a** | A pauses, B is created/started/resumed, then A stops once it is no longer visible. A is only destroyed if it finishes. |
| 5 | **b** | Still partly visible, so it is only paused, not stopped. |
| 6 | **c** | Explicit intents are delivered regardless of filters. Needing a filter is true for implicit intents. |
| 7 | **d** | Catch it with try/catch. |
| 8 | **c** | `startActivity()` adds `CATEGORY_DEFAULT` automatically; filters without it won't match. `LAUNCHER` is only for the launcher icon. |
| 9 | **a** | A stopped-but-bound service waits until all clients unbind. |
| 10 | **b** | A foreground service shows a notification via `startForeground()`. |
| 11 | **c** | The holder is created first, then bound with `onBindViewHolder()`. |
| 12 | **a** | `getItemViewType()` returns the type; `onCreateViewHolder()` inflates per type. |
| 13 | **d** | `centerCrop` fills and crops. `fitCenter` shows the whole image. |
| 14 | **c** | Activity Manager is in the framework; SQLite is a native library; Binder is in the kernel layer; ART is the runtime. |
| 15 | **c** | ListView recycles through `convertView`; ViewHolder is optional. |

## Short answers

**S1.** A Service is a background component without a UI, used for long-running work.
- *Started:* begun by `startService()`; runs until `stopSelf()` / `stopService()`; independent of the starter.
- *Bound:* begun by `bindService()`; client–server interaction through `IBinder`; lives only while clients are bound.

**S2.** A ViewHolder keeps references to an item's child views, so `findViewById()` is called once per holder instead of on every bind. With recycling, a scrolled-out row's holder is reused, saving CPU and memory. It is mandatory in RecyclerView.

**S3.** `gravity` aligns an element's **contents** (a LinearLayout's children, a TextView's text), e.g. `android:gravity="center"` on the parent centres its children. `layout_gravity` positions the **view itself** in its parent, e.g. `android:layout_gravity="end"` on a Button.

**S4.** No; explicit intents go to the named class regardless of filters. For implicit intents, the system compares the intent's **action, category and data** with the filters in installed apps' manifests: one match → started, several → chooser, none → `ActivityNotFoundException`.

**S5.** `onPause()`: the activity is losing focus but may still be visible; do light work (pause an animation). `onStop()`: the activity is no longer visible; do heavier work (save data, release resources).

## Scenarios

**SC1.** Leftover = 360 − 720 = −360 → A = 360 − 90 = **270dp**, B = 360 − 270 = **90dp**. The heavier weight shrinks more because the children overflowed. Fix: `layout_width="0dp"` on both → A = 90dp, B = 270dp.

**SC2.** Start in `onResume()`, release in `onPause()` (pair set-up and tear-down callbacks). For visible-but-unfocused (multi-window), start in `onStart()` and release in `onStop()`, because the activity can be fully visible while paused. Trade-off: holding the camera while paused can block another resumed app.

**SC3.** `startActivity()` throws `ActivityNotFoundException` and crashes if uncaught. Wrap it in try/catch and show a message (or offer to install a viewer).

**SC4.** The counter is a **started, foreground** service (it survives screen-off and shows a notification); the Progress screen **binds** to it to call methods. It is destroyed only after `stopSelf()` / `stopService()` **and** unbinding.

**SC5.** `fitXY` stretches the 1:1 image into a 2:1 box, distorting it. Fixes: `fitCenter` (whole image, side bars) or `centerCrop` (fills, crops); or make the view square (e.g. 100dp × 100dp).

## Diagrams

**D1.** Five stacked boxes (top to bottom):
1. System apps | User apps
2. Java API Framework (**Activity Manager**)
3. Native C/C++ Libraries (**SQLite**) | Android Runtime (**ART**)
4. HAL (**camera HAL module**)
5. Linux Kernel (**camera driver**)

**D2.**
- Started: `startService() → onCreate() → onStartCommand() → running → stopSelf()/stopService() → onDestroy()`
- Bound: `bindService() → onCreate() → onBind() → clients bound → unbindService() → onUnbind() → onDestroy()`

## Code

**C1.**

```xml
<LinearLayout android:layout_width="match_parent" android:layout_height="wrap_content"
    android:orientation="horizontal">
    <Button android:layout_width="0dp" android:layout_height="wrap_content"
        android:layout_weight="2" android:text="A"/>
    <Button android:layout_width="0dp" android:layout_height="wrap_content"
        android:layout_weight="1" android:text="B"/>
    <Button android:layout_width="0dp" android:layout_height="wrap_content"
        android:layout_weight="1" android:text="C"/>
</LinearLayout>
```

Weights 2:1:1 = 50% / 25% / 25% (or `weightSum="4"` with 2, 1, 1). The width must be `0dp`.

**C2.**

```java
String url = etUrl.getText().toString();
Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse(url));
try {
    startActivity(intent);
} catch (ActivityNotFoundException e) {
    Toast.makeText(MainActivity.this, "No app can open this", Toast.LENGTH_SHORT).show();
}
```

The URL needs a scheme such as `https://`; otherwise add it first.

**C3.**

```java
@Override
public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
    View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_name, parent, false);
    return new ViewHolder(v);
}
@Override
public void onBindViewHolder(ViewHolder holder, int position) {
    holder.tvName.setText(names.get(position));
}
@Override
public int getItemCount() { return names.size(); }
```

In the Activity:

```java
recyclerView.setLayoutManager(new LinearLayoutManager(this));
recyclerView.setAdapter(new MyAdapter(names));
```
