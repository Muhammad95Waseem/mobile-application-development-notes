# 09. Activity

## Definition
An **Activity** is a single, focused screen of an app with which the user can interact. It provides the **window** in which the app draws its user interface. An app usually has several activities (e.g. a login screen, a home screen, a settings screen). One of them is marked as the **main/launcher** activity.

Class: `android.app.Activity` (usually `AppCompatActivity` in practice)

## Creating an Activity
1. Create a class that extends `AppCompatActivity`.
2. Override `onCreate()` and attach a layout with `setContentView()`.
3. Declare it in `AndroidManifest.xml`.

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

## Declaring in the Manifest
```xml
<application ...>
    <activity android:name=".MainActivity" android:exported="true">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>
    <activity android:name=".SecondActivity" />
</application>
```
Every activity **must** be declared in the manifest, or the app will crash when starting it.

## Starting and Finishing
```java
startActivity(new Intent(this, SecondActivity.class));   // start
finish();                                                 // close the current one
```

## Back Stack
Activities are kept in a **stack** (last in, first out) called the **back stack**. Starting a new activity pushes it on top; pressing Back pops it and resumes the one beneath.

```
 |  Activity C  |  <- top (visible)
 |  Activity B  |
 |  Activity A  |
```

## Launch Modes (brief)
| Mode | Behaviour |
|---|---|
| `standard` | New instance every time (default) |
| `singleTop` | Reuse if already on top of the stack |
| `singleTask` | Only one instance in the task; clears the activities above it |
| `singleInstance` | Only activity in its own task |

## Role of the Activity
- Hosts the UI (views and layouts) and handles user events.
- Responds to lifecycle changes (see `10_activity_lifecycle.md`).
- Starts other components using Intents.
- Can host **Fragments** to build flexible UIs.

## Saving State
Configuration changes (e.g. rotation) destroy and recreate the activity. Use `onSaveInstanceState(Bundle)` to save temporary data, or a `ViewModel` for UI data.
