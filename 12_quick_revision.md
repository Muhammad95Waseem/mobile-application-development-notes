# 12. Quick Revision Sheet

A one-page summary of all topics.

## Definitions

- **Android architecture:** a layered stack: Linux Kernel → HAL → Native Libraries | Android Runtime → Java API Framework → System apps | User apps.
- **Intent:** a messaging object to request an action from a component (start an Activity/Service, send a broadcast).
- **Explicit intent:** names the exact target class. **Implicit intent:** names an action; the system finds a handler.
- **Intent filter:** a manifest declaration of the implicit intents a component accepts (action, category, data).
- **Activity:** a single screen with a UI. **Service:** a background component with no UI.
- **LinearLayout:** arranges children in one row or column.
- **RecyclerView:** an efficient list/grid that recycles item views via Adapter + ViewHolder + LayoutManager.
- **ImageView:** displays an image; `scaleType` controls how it fits.

## Architecture layers (top → bottom)

1. **System apps | User apps**
2. **Java API Framework:** Activity Manager, View System, Content Providers, Notification Manager
3. **Native C/C++ Libraries | Android Runtime:** SQLite, OpenGL ES, Media, SSL, WebKit | ART, core libraries
4. **HAL:** audio, Bluetooth, camera, sensors
5. **Linux Kernel:** drivers, memory, process, power, security, Binder

## Lifecycles

- **Activity:** `onCreate → onStart → onResume → onPause → onStop → onDestroy`; `onRestart` after stop. A→B: A.onPause, B created/started/resumed, A.onStop.
- **Started service:** `startService → onCreate → onStartCommand → running → stopSelf/stopService → onDestroy`.
- **Bound service:** `bindService → onCreate → onBind → bound → unbindService → onUnbind → onDestroy`.

## LinearLayout rules

- `orientation` (parent, default horizontal), `gravity` (contents), `layout_gravity` (self), `layout_weight` (share of leftover), `weightSum` (parent total).
- Leftover = parent − children's sizes; weighted dimension → `0dp`.
- Equal weights ≠ equal width with `wrap_content`; `match_parent` + weights → the bigger weight shrinks more.
- Landscape cut-off → `ScrollView`, `layout-land/`, weights.

## Comparison tables

| | Started | Bound | Foreground |
|---|---|---|---|
| Starts with | `startService()` | `bindService()` | `startForeground()` after start |
| Ends when | `stopSelf/stopService` | all clients unbind | stopped / removed from foreground |
| Notification | No | No | **Yes** |

| | Explicit | Implicit |
|---|---|---|
| Target | named class | action/data |
| Filter needed | No | Yes |

| | RecyclerView | ListView |
|---|---|---|
| ViewHolder | Mandatory | Optional |
| Layouts | Linear/Grid/Staggered | Vertical only |
| Animations | Built-in | None |

## Handy snippets

```java
// explicit intent + data
Intent i = new Intent(FirstActivity.this, SecondActivity.class);
i.putExtra("student_name", "Ali");
startActivity(i);
String name = getIntent().getStringExtra("student_name");   // in the receiver

// implicit intent with failure handling
try { startActivity(new Intent(Intent.ACTION_VIEW, Uri.parse("https://example.com"))); }
catch (ActivityNotFoundException e) { /* show message */ }

// services
startService(new Intent(MainActivity.this, MyService.class));
stopService(new Intent(MainActivity.this, MyService.class));

// RecyclerView
recyclerView.setLayoutManager(new LinearLayoutManager(this));
recyclerView.setAdapter(adapter);

// ImageView
img.setImageResource(R.drawable.photo);
```

```xml
<Button android:layout_width="0dp" android:layout_height="wrap_content" android:layout_weight="1"/>
<ImageView android:layout_width="200dp" android:layout_height="200dp"
    android:src="@drawable/photo" android:scaleType="centerCrop"
    android:contentDescription="Photo"/>
```

## Points that are easy to mix up

- Explicit intents need **no** filter. Implicit intents need a matching filter that includes `DEFAULT`.
- No handler → `ActivityNotFoundException`; always catch it.
- `layout_gravity` vs `gravity`; a vertical layout ignores vertical `layout_gravity`.
- `match_parent` + weights inverts expectations; use `0dp`.
- Paused ≠ stopped; activities don't all follow one callback order; `onDestroy` may be skipped if the process is killed.
- Plain dialogs/toasts don't pause the activity; a translucent *activity* does.
- A bound service is tied to its client's lifecycle; stopping a still-bound service waits for unbind.
- Foreground = notification (not a start/bind mode).
- LayoutManager arranges; Adapter supplies data; `onBindViewHolder` binds.
- `fitXY` distorts; `centerCrop` crops; the default is `fitCenter`.
- ART lives in Android Runtime (Layer 3); Activity Manager in the Java API Framework.

## Self-check

- [ ] Draw the 5-layer architecture and place five components correctly.
- [ ] Draw the Activity lifecycle and the A→B sequence.
- [ ] Draw started and bound Service lifecycles.
- [ ] Differentiate started / bound / foreground services and pick one for a scenario.
- [ ] Write an explicit intent with extras, and read them in the receiver.
- [ ] Write an implicit intent with try/catch; write a matching intent filter.
- [ ] Calculate weight distributions, including the `match_parent` case.
- [ ] Give two fixes for a layout cut off in landscape.
- [ ] Write the Adapter's three methods and attach LayoutManager + Adapter.
- [ ] Compare RecyclerView and ListView on four points.
- [ ] Choose a `scaleType` for crop / whole image / no distortion.
