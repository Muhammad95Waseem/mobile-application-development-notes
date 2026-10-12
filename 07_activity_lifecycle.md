# 07. Activity and the Activity Lifecycle

## Definition

An **Activity** is an application component that provides **a single screen** with a user interface. An app usually has several activities; one of them is the launcher (entry point).

## Activity vs Service

| | Activity | Service |
|---|---|---|
| UI | **Yes** (one screen) | **No** |
| Purpose | Interact with the user | Long-running work in the background |
| Started by | `startActivity(intent)` | `startService()` / `bindService()` |

## Lifecycle diagram

```
                 Activity launched
                        |
                        v
                   onCreate()
                        |
                        v
        +------->   onStart()  <------------------+
        |               |                         |
        |               v                         |
        |  +---->   onResume()                    |
        |  |            |                         |
        |  |     ACTIVITY RUNNING                 |
        |  |     (user can interact)              |
        |  |            |                         |
        |  |   another activity comes in front    |
        |  |            v                         |
        |  +------  onPause()                     |
        |  user returns |                         |
        |   (still      | no longer visible       |
        |   visible)    v                         |
        |            onStop()                     |
        |             |    |                      |
        |             |    +---> onRestart() -----+
        |             |        (user navigates back)
        |             | finish() / Back / system destroys
        |             v
        |         onDestroy()
        |             |
        |             v
        |      Activity shut down
        |
        (if the process was killed while paused/stopped,
         returning creates a NEW instance: onCreate() again)
```

## The core callbacks

| Callback | When it is called | Typical work |
|---|---|---|
| `onCreate(Bundle)` | Activity is first created (once per instance) | `setContentView()`, `findViewById()`, set listeners, bind data |
| `onStart()` | Activity becomes **visible** | Prepare the UI |
| `onResume()` | Activity is in the **foreground** and interactive | Start things that run only while in the foreground (e.g. a camera preview) |
| `onPause()` | Activity is losing focus; the **first sign** the user is leaving | Pause light things; **don't** save heavy data here |
| `onStop()` | Activity is **no longer visible** | Save data, release heavy resources |
| `onDestroy()` | Just before the instance is destroyed | Final cleanup |

`onRestart()` is called after `onStop()` when the user returns, just before `onStart()`.

Source: [The activity lifecycle](https://developer.android.com/guide/components/activities/activity-lifecycle).

## Code skeleton

```java
@Override protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
}
@Override protected void onStart()   { super.onStart();   /* visible */ }
@Override protected void onResume()  { super.onResume();  /* interactive */ }
@Override protected void onPause()   { super.onPause();   /* losing focus */ }
@Override protected void onStop()    { super.onStop();    /* hidden */ }
@Override protected void onDestroy() { super.onDestroy(); /* cleanup */ }
```

Always call `super.xxx()` first in Activity callbacks.

## Common sequences

The order depends on the situation, so don't memorise one fixed sequence; know the conditions.

| Situation | Callbacks |
|---|---|
| App launched | `onCreate → onStart → onResume` |
| Press **Home** / switch app | `onPause → onStop` |
| Come back from Home | `onRestart → onStart → onResume` |
| Press **Back** (activity finishes) | `onPause → onStop → onDestroy` |
| **Activity A starts Activity B** | A: `onPause`; B: `onCreate → onStart → onResume`; then A: `onStop` (only if A is no longer visible) |
| Another **partially covering/transparent** activity | Only `onPause` (and back via `onResume`) |
| **Rotation** (default config change) | `onPause → onStop → onDestroy → onCreate → onStart → onResume` (a new instance) |
| System kills the process while stopped | **No `onDestroy()`** is called for that instance |

## Things that are easy to get wrong

1. **Paused vs stopped:** *Paused* = still partly visible but not focused (multi-window, a translucent activity on top). *Stopped* = completely invisible. A paused activity can even be fully visible in multi-window mode.
2. **Not every activity runs every callback in the same order.** A→B, rotation, process death and `finish()` all differ.
3. **Dialogs:** a new semi-transparent *activity* (such as a dialog-themed one) pauses the activity underneath. A plain `AlertDialog` or `Toast` is a window, not an activity, so it normally does **not** call `onPause()`.
4. `onPause()` is brief; don't save data or make network calls there. Use `onStop()`.
5. `onDestroy()` can come from `finish()` **or** from a configuration change; use `isFinishing()` to tell them apart.
6. `onCreate()` receives `savedInstanceState`; `null` means a brand-new activity.

## Quick summary

- Lifecycle: `onCreate → onStart → onResume → [running] → onPause → onStop → onDestroy`; `onRestart` on return from stopped.
- Visible but unfocused = paused; invisible = stopped; finishing = destroyed.
