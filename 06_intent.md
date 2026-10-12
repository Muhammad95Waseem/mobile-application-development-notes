# 06. Intent

## Definition
An **Intent** is a **messaging object** used to request an action from another app component. It describes an operation to be performed and carries the data needed for it. Intents are the "glue" that connects the components of an app (and different apps) together.

Class: `android.content.Intent`

## Main Uses
1. **Starting an Activity** - `startActivity(intent)`
2. **Starting a Service** - `startService(intent)` / `bindService(intent, ...)`
3. **Delivering a Broadcast** - `sendBroadcast(intent)`

## Parts of an Intent
| Part | Description |
|---|---|
| **Component name** | The exact class to start (used in explicit intents) |
| **Action** | The generic action to perform, e.g. `ACTION_VIEW`, `ACTION_SEND`, `ACTION_DIAL` |
| **Data** | A URI (and/or MIME type) of the data to act on, e.g. `tel:12345`, `https://...` |
| **Category** | Extra info about the kind of component that should handle it, e.g. `CATEGORY_LAUNCHER` |
| **Extras** | Key-value pairs (a Bundle) carrying additional information |
| **Flags** | Instructions on how to launch/handle the activity, e.g. `FLAG_ACTIVITY_NEW_TASK` |

## Examples
### Move from one activity to another
```java
Intent intent = new Intent(MainActivity.this, SecondActivity.class);
startActivity(intent);
```

### Pass data with extras
```java
Intent intent = new Intent(MainActivity.this, SecondActivity.class);
intent.putExtra("username", "Ali");
intent.putExtra("age", 21);
startActivity(intent);
```

### Receive data in the second activity
```java
Intent intent = getIntent();
String name = intent.getStringExtra("username");
int age = intent.getIntExtra("age", 0);
```

### Open a web page / dial a number
```java
Intent web = new Intent(Intent.ACTION_VIEW, Uri.parse("https://www.google.com"));
startActivity(web);

Intent call = new Intent(Intent.ACTION_DIAL, Uri.parse("tel:03001234567"));
startActivity(call);
```

### Share text
```java
Intent share = new Intent(Intent.ACTION_SEND);
share.setType("text/plain");
share.putExtra(Intent.EXTRA_TEXT, "Hello!");
startActivity(Intent.createChooser(share, "Share via"));
```

## Getting a Result Back
Use the Activity Result API (modern) or `startActivityForResult()` (old) to start an activity and receive data when it finishes.

## Key Points
- An Intent can be **explicit** or **implicit** (see `07_types_of_intent.md`).
- Implicit intents are matched using **intent filters** (see `08_intent_filter.md`).
- Extras can hold primitives, Strings, Serializable and Parcelable objects.
- Intent data is limited in size (the transaction buffer is about 1 MB), so do not pass huge objects.
