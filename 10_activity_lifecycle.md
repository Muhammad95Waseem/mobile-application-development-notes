# 10. Activity Lifecycle

## Concept
An activity moves through different **states** as the user navigates in, out of, and back to the app. The Android system notifies the activity of each change through **callback methods**. Overriding them lets you manage resources properly and avoid crashes, memory leaks and lost data.

## Lifecycle Diagram
```
                 Activity launched
                        |
                        v
                   +----------+
                   | onCreate |   (first creation, set up UI)
                   +----------+
                        |
                        v
      +------------> onStart <---------------+
      |                 |                    |
      |                 v                    |
      |             onResume                 |
      |                 |                    |
      |         ACTIVITY RUNNING             |
      |      (in foreground, interactive)    |
      |                 |                    |
      |     another activity comes to front  |
      |                 |                    |
      |                 v                    |
      |             onPause  --(user returns)-> onResume
      |                 |
      |    activity no longer visible
      |                 |
      |                 v
      |             onStop
      |                 |
      |    +------------+-------------+
      |    |                          |
      |  user returns          activity finished /
      |    |                    killed by system
      |    v                          |
  onRestart                           v
      |                          onDestroy
      +---> onStart                   |
                                      v
                             Activity shut down
```

## Callback Methods
| Method | When called | Typical use |
|---|---|---|
| `onCreate()` | Activity is first created | Set layout, initialise views, variables; restore saved state |
| `onStart()` | Activity becomes **visible** | Prepare to enter the foreground |
| `onResume()` | Activity is in the **foreground** and ready for interaction | Start animations, camera, sensors |
| `onPause()` | Another activity is coming in front; partially visible | Pause animations, save unsaved data, release sensors; keep it fast |
| `onStop()` | Activity is **no longer visible** | Release heavy resources, stop network/location updates |
| `onRestart()` | Called after onStop, before the activity restarts | Re-initialise things released in onStop |
| `onDestroy()` | Activity is being destroyed (finish() or system) | Final cleanup |

## The Three Nested Loops
1. **Entire lifetime**: `onCreate()` -> `onDestroy()`.
2. **Visible lifetime**: `onStart()` -> `onStop()`. The user can see the activity.
3. **Foreground lifetime**: `onResume()` -> `onPause()`. The user can interact with it.

## Common Scenarios
| Scenario | Callbacks sequence |
|---|---|
| App launched | onCreate -> onStart -> onResume |
| Press Home button | onPause -> onStop |
| Return to the app | onRestart -> onStart -> onResume |
| Press Back button (exit) | onPause -> onStop -> onDestroy |
| Phone call / dialog partly covers the screen | onPause (-> onResume when it ends) |
| Screen rotation | onPause -> onStop -> onDestroy -> onCreate -> onStart -> onResume |
| Open another activity B from A | A.onPause -> B.onCreate -> B.onStart -> B.onResume -> A.onStop |

## Example
```java
public class MainActivity extends AppCompatActivity {
    @Override protected void onCreate(Bundle b) { super.onCreate(b); Log.d("LC", "onCreate"); }
    @Override protected void onStart()   { super.onStart();   Log.d("LC", "onStart"); }
    @Override protected void onResume()  { super.onResume();  Log.d("LC", "onResume"); }
    @Override protected void onPause()   { super.onPause();   Log.d("LC", "onPause"); }
    @Override protected void onStop()    { super.onStop();    Log.d("LC", "onStop"); }
    @Override protected void onRestart() { super.onRestart(); Log.d("LC", "onRestart"); }
    @Override protected void onDestroy() { super.onDestroy(); Log.d("LC", "onDestroy"); }
}
```
Always call the `super` method first in each callback.

## Activity States
- **Running/Resumed** - in the foreground with user focus.
- **Paused** - partially visible, lost focus.
- **Stopped** - completely hidden, but still in memory.
- **Destroyed** - removed from memory.

## Notes
- The system may kill a stopped/paused activity to free memory; `onDestroy()` is not guaranteed in that case, so save important data in `onPause()`/`onStop()`.
- `onSaveInstanceState()` is called before the activity may be destroyed, to save UI state.
