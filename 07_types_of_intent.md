# 07. Types of Intent

Android has **two main types** of intents: **Explicit** and **Implicit**.

## 1. Explicit Intent
An intent that **names the exact component** (class) that should handle it. It is usually used to start a component **within your own app**, because you know the class name.

```java
Intent intent = new Intent(MainActivity.this, SecondActivity.class);
startActivity(intent);
```
- The target is specified using the **component name**.
- The system delivers it directly to that component; no intent filter matching is required.
- Used to start activities, services within the same app.

## 2. Implicit Intent
An intent that **does not name a specific component**. Instead it declares a **general action** to perform, and the Android system finds a suitable component (in any app) to handle it.

```java
Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("https://www.example.com"));
startActivity(intent);
```
- Specifies an **action**, **data**/MIME type and optionally **category**.
- The system compares it with the **intent filters** declared in the manifests of installed apps.
- If **one** match -> that component starts.
- If **several** match -> a chooser dialog lets the user pick an app.
- If **none** match -> `ActivityNotFoundException`. Check first with `intent.resolveActivity(getPackageManager()) != null`.

Common implicit actions: `ACTION_VIEW`, `ACTION_SEND`, `ACTION_DIAL`, `ACTION_CALL`, `ACTION_SENDTO`, `ACTION_PICK`, `ACTION_IMAGE_CAPTURE`.

## Comparison
| Feature | Explicit Intent | Implicit Intent |
|---|---|---|
| Target | Specific class named | Only action/data described |
| Typical use | Inside your own app | Using other apps' features |
| Intent filter needed | No | Yes (in the receiving component) |
| Who decides the receiver | Developer | Android system / user |
| Example | Open SecondActivity | Open a URL, share text, dial |

## Other Related Types
- **PendingIntent**: a token wrapping an Intent that another app/the system (e.g. notifications, alarms) can execute later on behalf of your app.
- **Broadcast Intent**: an intent sent with `sendBroadcast()` to notify any interested receivers (e.g. battery low, boot completed).
