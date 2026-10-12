# 06. Intent Filters

## Definition

An **intent filter** is a declaration in `AndroidManifest.xml` that tells the system which implicit intents a component can handle.

## Where it is declared

Inside the component's manifest element (`<activity>`, `<service>`, `<receiver>`), as `<intent-filter>`. For Activities, filters **must** be declared in the manifest.

## The three elements

| Element | Meaning | Rule |
|---|---|---|
| `<action android:name="..."/>` | Accepted action | The intent's action must match one listed |
| `<category android:name="..."/>` | Accepted categories | Every category in the intent must be in the filter |
| `<data android:mimeType="..." android:scheme="..."/>` | Accepted URI/MIME type | Compared to the intent's data |

An intent must pass **all three** tests to be delivered through that filter. For activities that receive implicit intents, the filter **must include** `android.intent.category.DEFAULT`, because `startActivity()` adds it automatically.

## Example: manifest declaration and a matching intent

```xml
<activity android:name=".ShareActivity" android:exported="false">
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="text/plain"/>
    </intent-filter>
</activity>
```

A matching intent (sent from any component):

```java
Intent i = new Intent(Intent.ACTION_SEND);
i.setType("text/plain");
i.putExtra(Intent.EXTRA_TEXT, "Hello");
startActivity(i);   // resolves to ShareActivity
```

The `exported` value above follows the official example. If other apps should be able to send text to you, set `exported="true"`.

## The launcher activity

The launcher activity is also declared with an intent filter: `MAIN` + `LAUNCHER`.

```xml
<intent-filter>
    <action android:name="android.intent.action.MAIN"/>
    <category android:name="android.intent.category.LAUNCHER"/>
</intent-filter>
```

## Common mistakes

- **Android 12+:** any component with an `<intent-filter>` must explicitly set `android:exported`, or the app won't install.
- A filter is **not** a security barrier: another app can still start your component with an explicit intent if it knows the class name. To keep a component private, set `android:exported="false"` and don't add a filter.
- Missing `CATEGORY_DEFAULT` → implicit intents won't resolve to your Activity.
- Filters for **broadcast receivers** can also be registered in code (`registerReceiver`); **Activities** can't.

## Quick summary

- A filter is a manifest declaration of which implicit intents a component accepts: action + category + data (all must match).
- Needed for implicit intents, **not** needed for explicit ones.
- Include the `DEFAULT` category and set `exported`.
