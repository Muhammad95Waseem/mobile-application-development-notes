# 13. Service Lifecycle

The lifecycle of a service depends on **how it is created**: by `startService()` (**started service**) or by `bindService()` (**bound service**).

## Lifecycle Diagram

```
   Started Service                         Bound Service
   ---------------                         -------------
   startService()                          bindService()
        |                                       |
        v                                       v
    onCreate()                              onCreate()
        |                                       |
        v                                       v
  onStartCommand()                          onBind()
        |                                       |
        v                                       v
  SERVICE RUNNING                        CLIENTS BOUND / RUNNING
        |                                       |
  stopSelf() or                           all clients call
  stopService()                           unbindService()
        |                                       |
        v                                       v
   onDestroy()                             onUnbind()
        |                                       |
        v                                       v
 Service shut down                          onDestroy()
                                                |
                                                v
                                         Service shut down
```

(If a client binds again after `onUnbind()` returned true, `onRebind()` is called.)

## Callback Methods
| Method | Description |
|---|---|
| `onCreate()` | Called **once** when the service is first created, before `onStartCommand()` or `onBind()`. Do one-time setup. |
| `onStartCommand(Intent, flags, startId)` | Called every time another component starts it with `startService()`. The work begins here. Returns how the system should behave if the service is killed. |
| `onBind(Intent)` | Called when a component calls `bindService()`. Returns an `IBinder` (or `null` if binding is not allowed). |
| `onUnbind(Intent)` | Called when **all** clients have disconnected. |
| `onRebind(Intent)` | Called when new clients bind after `onUnbind()` had returned true. |
| `onDestroy()` | Called when the service is no longer used and is being destroyed. Release threads, receivers, etc. |

## Started Service Flow
1. `startService()` -> `onCreate()` (only if not already running) -> `onStartCommand()`.
2. Calling `startService()` again does **not** create another instance; it just calls `onStartCommand()` again.
3. The service keeps running until `stopSelf()` or `stopService()` is called -> `onDestroy()`.

## Bound Service Flow
1. `bindService()` -> `onCreate()` (if needed) -> `onBind()`.
2. Clients interact through the returned `IBinder`.
3. When the last client calls `unbindService()` -> `onUnbind()` -> `onDestroy()`.

## Started + Bound
If a service is both started and bound, it is destroyed only after it has been stopped **and** all clients have unbound.

## Lifetimes
- **Entire lifetime**: from `onCreate()` to `onDestroy()`.
- **Active lifetime**: starts with `onStartCommand()` or `onBind()`; ends with the end of `onStartCommand()` processing or `onUnbind()`.

## Important Points
- A service runs on the main thread, so spawn a worker thread for long tasks.
- The system can kill a service under low memory; `onStartCommand()` return values (`START_STICKY`, etc.) decide if it is restarted.
- A service should stop itself when its work is done, to save resources.
