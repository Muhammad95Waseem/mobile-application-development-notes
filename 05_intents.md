# 05. Intents (Explicit and Implicit)

## Definition

An **Intent** is a messaging object used to request an action from another app component (Activity, Service, or Broadcast Receiver).

## Main uses

1. **Start an Activity**: `startActivity(intent)`
2. **Start or bind a Service**: `startService()` / `bindService()`
3. **Deliver a broadcast**: `sendBroadcast()`
4. Carry small pieces of data between components (extras).

Source: [Intents and intent filters](https://developer.android.com/guide/components/intents-filters).

## What an intent carries

| Part | Role |
|---|---|
| **Component name** | Makes the intent *explicit* (which class to start) |
| **Action** | What to do (`ACTION_VIEW`, `ACTION_SEND`) |
| **Data** | URI and/or MIME type to act on |
| **Category** | Extra info about the handling component (`CATEGORY_LAUNCHER`) |
| **Extras** | Key-value pairs (your data) |
| **Flags** | Launch metadata |

## Explicit intent

An intent that names the exact component (class) that should handle it. It is used mainly **inside your own app**, when you know the target class (open another Activity, start your own Service).

### Example: open `SecondActivity` when a button is pressed

```java
Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
startActivity(intent);
```

- `Intent(Context, Class)` names the target explicitly; `startActivity()` asks the system to start it.
- This code goes inside the button's `onClick` handler in the sending Activity.
- `SecondActivity` must be declared in the manifest: `<activity android:name=".SecondActivity"/>`.

**Common mistakes:** forgetting the manifest entry (the app crashes with `ActivityNotFoundException`), or using `this` inside an anonymous click listener (it refers to the listener, not the Activity, so use `FirstActivity.this`). If the click handler is a lambda (`v -> { ... }`), `this` is fine, but `FirstActivity.this` always works.

## Passing data with extras

### Example: send a student's name to `SecondActivity`

**Sender:**

```java
Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
intent.putExtra("student_name", "Ali");
startActivity(intent);
```

**Receiver, in `onCreate()` of `SecondActivity`:**

```java
String name = getIntent().getStringExtra("student_name");
```

- The key `"student_name"` must be **identical** on both sides.
- The getter must match the type: `putExtra("age", 20)` → `getIntExtra("age", 0)` (the second argument is the default value). Use `getStringExtra`, `getIntExtra`, `getBooleanExtra`, `getDoubleExtra` as appropriate.
- **Common mistake:** a key spelling/case mismatch, or reading with the wrong type, returns `null` or the default.

## Implicit intent

An intent that does **not** name a component. It declares a general **action** (and data), and the system finds a component that can do it, which may be in another app. Used when your app can't do the job itself: open a web page, dial a number, share text, view a map.

### Example: open a web page

```java
Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("https://www.example.com"));
startActivity(intent);
```

### Handling the case where no app can handle it

```java
try {
    startActivity(intent);
} catch (ActivityNotFoundException e) {
    Toast.makeText(this, "No app can handle this", Toast.LENGTH_SHORT).show();
}
```

### Example: share text

```java
Intent send = new Intent(Intent.ACTION_SEND);
send.setType("text/plain");
send.putExtra(Intent.EXTRA_TEXT, "My result: 85%");
startActivity(Intent.createChooser(send, "Share via"));
```

### How the system resolves an implicit intent

The system compares the intent against the **intent filters** declared in other apps' manifests, testing three things: **action**, **data (URI + MIME type)**, and **category**. All three must pass.

- One match → that component opens.
- Several matches → the system shows a **chooser dialog**.
- No match → `ActivityNotFoundException`.

See [06_intent_filters.md](06_intent_filters.md) for how filters are written.

## Example scenario: a "Share Result" button

- Use an **implicit intent** (`ACTION_SEND`), because the app doesn't know which app (WhatsApp, Gmail, Messages...) the user wants. The system lists every app that can handle it and the user chooses.
- Use `Intent.createChooser()` so the user is asked every time, since share targets vary.
- **If no installed app can handle it:** `startActivity()` throws `ActivityNotFoundException` and the app crashes unless you catch it. Catch it and show a Toast or message.
- *Why not explicit?* An explicit intent needs the exact class name of another app's Activity, which you can't know in advance.

## Explicit vs Implicit

| | Explicit | Implicit |
|---|---|---|
| Names the target class? | **Yes** (component name) | **No** (action + data) |
| Typical target | Your own app's Activity/Service | Other apps' components |
| Needs an intent filter on the target? | **No** | **Yes** (the target must declare a matching filter) |
| Example | `new Intent(this, SecondActivity.class)` | `new Intent(Intent.ACTION_VIEW, uri)` |
| Failure | Class missing from the manifest | No matching app → `ActivityNotFoundException` |

## Common mistakes and extra notes

- **"Explicit intents need an intent filter"** is **false**. The docs say an explicit intent is always delivered to its target, regardless of any intent filters. An Activity with **no** filter can be started **only** by explicit intents.
- Always use an **explicit** intent to start a **Service**. Implicit service intents are a security hazard, and `bindService()` with an implicit intent throws an exception since Android 5.0.
- A **PendingIntent** wraps an intent so another app or the system (a notification, an alarm) can run it later. It is not a type of intent like explicit/implicit.
- **Broadcast** is a way of *delivering* an intent, not a type of intent.
- Android 11+ **package visibility:** `resolveActivity()` can return `null` for apps you haven't declared in `<queries>`; catching `ActivityNotFoundException` is simpler. ([Package visibility](https://developer.android.com/training/package-visibility))
- `startActivityForResult()` is the old way to get a result back; modern code uses the Activity Result APIs. Older textbooks still use the old name.

## Quick summary

- Intent = a message to start an Activity/Service or send a broadcast.
- Explicit = class named. Implicit = action named.
- Data: `putExtra(key, value)` → `getIntent().getXxxExtra(key)`, with the same key.
- No handler → `ActivityNotFoundException` → catch it.
