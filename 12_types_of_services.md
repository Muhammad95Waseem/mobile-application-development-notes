# 12. Types of Services

Android services are commonly classified into **Foreground, Background and Bound** services. Another classification is **Started vs Bound**.

## 1. Foreground Service
- Performs an operation that is **noticeable to the user**.
- **Must show a persistent notification** in the status bar so the user knows it is running.
- Keeps running even when the user is not interacting with the app.
- Started with `startForegroundService()` and then the service calls `startForeground(id, notification)`.
- Example: music player, fitness tracker, navigation, ongoing download.

```java
Notification n = new NotificationCompat.Builder(this, "channel_id")
        .setContentTitle("Playing music")
        .setSmallIcon(R.drawable.ic_music).build();
startForeground(1, n);
```

## 2. Background Service
- Performs an operation that is **not directly noticed** by the user.
- No notification is shown.
- Since Android 8.0 the system strongly restricts background services when the app is not in the foreground; modern apps use **WorkManager** or JobScheduler for such tasks.
- Example: syncing data, compacting storage.

## 3. Bound Service
- Offers a **client-server interface**: components (activities, other services, even other apps) **bind** to it with `bindService()` to interact, send requests and receive results.
- Runs **only as long as at least one component is bound** to it. When all clients unbind, the service is destroyed.
- Implements `onBind()` and returns an `IBinder`.
- Ways to define the interface: extending the `Binder` class (same process), using a `Messenger`, or using **AIDL** (different processes/IPC).
- Example: an activity calling methods on a music-player service.

```java
bindService(new Intent(this, MyService.class), connection, Context.BIND_AUTO_CREATE);
```

## Started vs Bound Service
| Feature | Started Service | Bound Service |
|---|---|---|
| Started by | `startService()` | `bindService()` |
| Lifetime | Runs until `stopSelf()` / `stopService()` | Runs while clients are bound |
| Communication | No direct result to the caller | Two-way interaction through IBinder |
| Key callback | `onStartCommand()` | `onBind()` |
| Stops when | Explicitly stopped | Last client unbinds |

A service can be **both** started and bound.

## Summary Table
| Type | User aware? | Notification | Typical use |
|---|---|---|---|
| Foreground | Yes | Required | Music, navigation, tracking |
| Background | No | None | Sync, cleanup (restricted today) |
| Bound | Depends | None | Client-server interaction with components |

## Note
`IntentService` (a started service that processes requests one by one on a worker thread and stops itself) was used earlier but is **deprecated**; use WorkManager or JobIntentService/coroutines.
