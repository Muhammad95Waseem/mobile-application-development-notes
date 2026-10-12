# 11. Services

## Definition
A **Service** is an Android application component that performs **long-running operations in the background** and **does not provide a user interface**. It can keep running even when the user switches to another app.

Class: `android.app.Service`

Examples: playing music in the background, downloading a file, syncing data, tracking location, handling network transactions.

## Key Facts
- A service has **no UI**.
- By default a service runs on the **main (UI) thread** of the app's process. Heavy work must be moved to a background thread (Thread, Executor, Coroutine) or it will block the UI (ANR).
- Another component (e.g. an activity) can **start** it or **bind** to it.
- It must be declared in the manifest.
- Since Android 8.0 (API 26), background services are restricted; long-running tasks should use **foreground services** or **WorkManager**.

## Creating a Service
```java
public class MyService extends Service {

    @Override
    public void onCreate() {
        super.onCreate();
        // one-time setup
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        // do the work (on a background thread)
        return START_STICKY;
    }

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return null;   // null for a started (non-bound) service
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        // cleanup
    }
}
```

## Declaring in the Manifest
```xml
<service android:name=".MyService" android:exported="false" />
```

## Starting and Stopping
```java
Intent intent = new Intent(this, MyService.class);
startService(intent);        // start
stopService(intent);         // stop from outside
// inside the service: stopSelf();
```

## Return Values of onStartCommand()
| Value | Meaning |
|---|---|
| `START_STICKY` | If killed, system recreates the service (intent is null) |
| `START_NOT_STICKY` | If killed, do not recreate it |
| `START_REDELIVER_INTENT` | If killed, recreate it and redeliver the last intent |

## Service vs Activity vs Thread
| | Service | Activity | Thread |
|---|---|---|---|
| UI | No | Yes | No |
| Purpose | Background work for the app/system | User interaction | Run code concurrently |
| Lives in | App process (main thread by default) | App process | Inside a component |
| Component? | Yes (declared in manifest) | Yes | No |

See `12_types_of_services.md` and `13_service_lifecycle.md`.
