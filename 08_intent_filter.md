# 08. Intent Filter

## Definition
An **intent filter** is an expression in the app's **AndroidManifest.xml** that **declares what types of intents a component can receive**. It tells the Android system which implicit intents an activity, service or broadcast receiver is able to handle.

Without an intent filter, a component can only be started with an **explicit** intent.

## Elements of an Intent Filter
| Element | Meaning |
|---|---|
| `<action>` | The action(s) the component can handle, e.g. `android.intent.action.VIEW` |
| `<category>` | The category(ies) of the intent, e.g. `android.intent.category.DEFAULT` |
| `<data>` | The data URI and/or MIME type it can handle (scheme, host, port, path, mimeType) |

## Example 1: The Launcher Activity
```xml
<activity android:name=".MainActivity" android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```
- `MAIN` - this is the main entry point of the app.
- `LAUNCHER` - show it in the home-screen app launcher.

## Example 2: An Activity that Handles Web Links
```xml
<activity android:name=".WebActivity" android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="https" android:host="www.example.com" />
    </intent-filter>
</activity>
```

## Example 3: Receiving Shared Text
```xml
<intent-filter>
    <action android:name="android.intent.action.SEND" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:mimeType="text/plain" />
</intent-filter>
```

## How Matching Works (Intent Resolution)
An implicit intent is delivered to a component only if it **passes all three tests**:
1. **Action test** - the intent's action must match one of the filter's actions.
2. **Category test** - **every** category in the intent must be in the filter. (`startActivity()` adds `CATEGORY_DEFAULT` automatically, so activities that should receive implicit intents must declare `DEFAULT`.)
3. **Data test** - the intent's URI and MIME type must match the filter's `<data>`.

If an intent matches multiple components, the user is shown a chooser.

## Common Categories
| Category | Use |
|---|---|
| `DEFAULT` | Required to receive implicit intents via startActivity |
| `LAUNCHER` | Listed in the home-screen launcher |
| `BROWSABLE` | Can be started from a web browser/link |
| `HOME` | Acts as the home screen |

## Notes
- From Android 12 (API 31), any component with an intent filter **must** explicitly declare `android:exported="true"` or `"false"`.
- Intent filters for **broadcast receivers** can also be registered dynamically in code with `registerReceiver()`.
- Intent filters are never used to resolve **explicit** intents.
- A component can have **multiple** intent filters.
