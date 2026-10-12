# 08. Android Services

## Definition

A **Service** is an application component that performs **long-running operations in the background** and provides **no user interface**.

## Important facts

- A service runs on the app's **main thread** by default. Use a separate thread for blocking work, or you get an ANR (Application Not Responding).
- It must be **declared in the manifest** (`android:name` is the required attribute).
- Use an **explicit** intent to start it.

Source: [Services overview](https://developer.android.com/develop/background-work/services).

## Types of services

| | **Started** | **Bound** | **Foreground** |
|---|---|---|---|
| Started by | `startService()` | `bindService()` | `startForegroundService()` then `startForeground()` |
| Lifetime | Runs until it calls `stopSelf()` or someone calls `stopService()` | Runs **only while a component is bound**; destroyed when all clients unbind | Same as started, with **higher priority** |
| Linked to the starter's lifecycle? | **No** (independent) | **Yes** (lives while a client is bound) | No |
| Communication | One-way (intent) | **Client–server** (`IBinder`, call methods, get results) | Via intent/bind |
| User awareness | Not visible | Not visible | **Must show a notification** |
| Example | Downloading a file | An Activity calling methods on a service | Music player |

*Foreground* describes **how noticeable and protected** the service is (notification + high priority), not how it was started. A foreground service is normally a **started** service that has called `startForeground()`.

A service can also be started **and** bound. Then it is destroyed only after it has been **stopped** *and* **unbound**.

## Service lifecycle

```
 STARTED SERVICE                          BOUND SERVICE
 startService()                           bindService()
      |                                        |
      v                                        v
  onCreate()                               onCreate()
      |                                        |
      v                                        v
 onStartCommand()                           onBind()
      |                                        |
      v                                        v
 SERVICE RUNNING                         CLIENTS BOUND (running)
      |                                        |
 stopSelf() / stopService()               all clients call unbindService()
      |                                        |
      |                                        v
      |                                    onUnbind()
      |                                        |
      v                                        v
  onDestroy()                              onDestroy()
      |                                        |
      v                                        v
 Service shut down                        Service shut down
```

`onRebind()` exists for re-binding after `onUnbind()`. There is **no `onStop()`** for services; `onDestroy()` is the last callback.

## Started service skeleton

```java
public class MyService extends Service {
    @Override public void onCreate() { /* one-time setup */ }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        // do (background-thread) work here
        return START_STICKY;     // how to behave if the system kills it
    }

    @Override public IBinder onBind(Intent intent) { return null; }  // not a bound service

    @Override public void onDestroy() { /* cleanup */ }
}
```

`onStartCommand` return values:

- `START_NOT_STICKY`: don't recreate the service after it is killed.
- `START_STICKY`: recreate it (with a null intent); fits media players.
- `START_REDELIVER_INTENT`: recreate it with the last intent; fits downloads.

**Manifest:**

```xml
<service android:name=".MyService" android:exported="false"/>
```

## Starting and stopping

From an **Activity**:

```java
// Start
startService(new Intent(MainActivity.this, MyService.class));
// Stop from outside
stopService(new Intent(MainActivity.this, MyService.class));
```

From inside the **Service**:

```java
stopSelf();
```

**Which code goes where:** `startService` / `stopService` / `bindService` → Activity. `onCreate` / `onStartCommand` / `onBind` / `onDestroy` / `stopSelf` → Service.

## Bound service

**In the Service:**

```java
public class MyService extends Service {
    private final IBinder binder = new LocalBinder();
    public class LocalBinder extends Binder {
        MyService getService() { return MyService.this; }
    }
    @Override public IBinder onBind(Intent intent) { return binder; }
    public int getProgress() { return 42; }       // a method clients can call
}
```

**In the Activity:**

```java
MyService service; boolean bound = false;

ServiceConnection conn = new ServiceConnection() {
    @Override public void onServiceConnected(ComponentName name, IBinder b) {
        service = ((MyService.LocalBinder) b).getService();
        bound = true;
    }
    @Override public void onServiceDisconnected(ComponentName name) { bound = false; }
};

// bind (e.g. in onStart) and unbind (e.g. in onStop)
bindService(new Intent(this, MyService.class), conn, Context.BIND_AUTO_CREATE);
if (bound) { unbindService(conn); bound = false; }
```

The key idea: `onBind` returns an `IBinder`, the `ServiceConnection` receives it, and `unbindService` ends the binding. More: [Bound services overview](https://developer.android.com/guide/components/bound-services).

## Foreground service

```java
// Activity side
ContextCompat.startForegroundService(this, new Intent(this, MusicService.class));

// Service side, inside onStartCommand(); must be called within ~5 seconds
startForeground(1, notification);   // notification built with a NotificationChannel (API 26+)
```

Manifest requirements on modern Android: `<uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>` and, when targeting API 34+, an `android:foregroundServiceType` on the `<service>` (for example `mediaPlayback`) plus the matching type permission. ([Declare foreground services](https://developer.android.com/develop/background-work/services/fgs/declare))

A foreground service **must show a notification**.

## Choosing a service type

- **Music player that keeps playing when the user switches apps → Foreground service** (a started service with a notification). It must continue after the user leaves, and the notification keeps the user aware and helps stop the system from killing it. It can also be bound so the UI can control playback.
- **File uploader that runs only while the app is open → Bound service** (or just a thread). Its lifetime is tied to the Activity that binds it, so it stops when the client unbinds and doesn't waste resources.

## Modern restrictions

- Android 8.0+ restricts **background services** when the app isn't in the foreground. Use `startForegroundService()` and then call `startForeground()` within five seconds.
- `IntentService` is **deprecated** (API 30). For deferrable background work, the docs recommend **WorkManager**. IntentService may still appear in older courses and textbooks.

## Common mistakes

- "Which service type is tied to the lifecycle of the component that binds to it?" → **Bound**.
- `stopService()` or `stopSelf()` **doesn't** destroy a service while clients are still bound.
- A started service does **not** stop itself automatically; you must call `stopSelf()` / `stopService()`.
- A service is **not** a separate thread or process by default.
- Don't confuse Activity callbacks (`onStart`, `onResume`) with Service callbacks (`onStartCommand`, `onBind`).

## Quick summary

- Service = background, no UI, main thread by default, declared in the manifest, started with an explicit intent.
- Started: `startService` → runs until stopped. Bound: `bindService` → lives while bound. Foreground: notification + high priority.
- Callbacks: `onCreate`, `onStartCommand`, `onBind`, `onUnbind`, `onDestroy`.
