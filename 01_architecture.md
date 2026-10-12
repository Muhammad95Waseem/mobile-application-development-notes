# 01. Android Architecture

## Definition

Android architecture is a **layered software stack**. Each layer provides services to the layer above it and relies on the layer below it. Our course divides it into five layers (top to bottom):

1. System apps | User apps
2. Java API Framework
3. Native C/C++ Libraries | Android Runtime
4. Hardware Abstraction Layer (HAL)
5. Linux Kernel

## Diagram

```
+----------------------------------------------------------------+
|  LAYER 5                                                       |
|  +-------------------------+  +------------------------------+ |
|  |       SYSTEM APPS       |  |          USER APPS           | |
|  | Dialer, Contacts,       |  | Weather app, WhatsApp,       | |
|  | Camera, Settings,       |  | your own apps ...            | |
|  | Browser                 |  |                              | |
|  +-------------------------+  +------------------------------+ |
+----------------------------------------------------------------+
|  LAYER 4: JAVA API FRAMEWORK                                   |
|  Activity Manager | Window Manager | View System               |
|  Content Providers | Notification Manager | Package Manager    |
|  Resource Manager | Location Manager | Telephony Manager       |
+----------------------------------------------------------------+
|  LAYER 3                                                       |
|  +-------------------------------+ +-------------------------+ |
|  | NATIVE C/C++ LIBRARIES        | | ANDROID RUNTIME         | |
|  | Media Framework, SQLite,      | | ART (Android 5.0+)      | |
|  | OpenGL ES, WebKit, SSL,       | | Core Libraries          | |
|  | libc, Surface Manager         | | (Dalvik before 5.0)     | |
|  +-------------------------------+ +-------------------------+ |
+----------------------------------------------------------------+
|  LAYER 2: HARDWARE ABSTRACTION LAYER (HAL)                     |
|  Audio | Bluetooth | Camera | Sensors | Display modules        |
+----------------------------------------------------------------+
|  LAYER 1: LINUX KERNEL                                         |
|  Display / Camera / Bluetooth / Audio / WiFi / USB drivers     |
|  Binder IPC | Memory mgmt | Process mgmt | Power mgmt | Security|
+----------------------------------------------------------------+
|                           HARDWARE                             |
+----------------------------------------------------------------+
```

Reference: [GeeksforGeeks, Android Architecture](https://www.geeksforgeeks.org/android/android-architecture/) (their diagram groups the layers slightly differently, but the components are the same).

## What each layer does

| Layer | Purpose | Key components |
|---|---|---|
| **System apps / User apps** | Apps the user sees and installs | System apps: Dialer, Contacts, Camera, Settings. User apps: any app installed by the user, including the ones you build |
| **Java API Framework** | The Java/Kotlin APIs that apps call; manages app behaviour and system services | Activity Manager (activity lifecycle), Window Manager, View System (UI widgets), Content Providers (share data between apps), Notification Manager, Package Manager |
| **Native C/C++ Libraries** | Performance-critical native code used by the framework | Media framework, SQLite (database), OpenGL ES (graphics), WebKit (browser engine), SSL, libc |
| **Android Runtime (ART)** | Executes app code | ART + core Java libraries. ART compiles apps ahead of time (AOT) |
| **Hardware Abstraction Layer (HAL)** | Standard interfaces that let the framework talk to hardware without knowing driver details | Audio, Bluetooth, camera, sensor HAL modules |
| **Linux Kernel** | Drivers, memory, process and power management, security | Display, camera, Bluetooth, audio, WiFi drivers; Binder IPC |

## Key points

- **Linux Kernel** is the foundation. Android is built on it.
- **Libraries and Android Runtime** share one layer. Libraries are written in C/C++; the runtime runs the app's compiled code.
- **ART replaced Dalvik** from Android 5.0 (Lollipop).
- **HAL** sits between the kernel and the layers above, so the same framework can work with different manufacturers' hardware.
- Each layer only talks to the layer directly above and below it.

## Example: a weather app

| Task | Layer | Why |
|---|---|---|
| Displaying the temperature on screen | **User apps** | The visible screen is the weather app itself (built using the framework's View System/TextView). |
| Managing the app's Activity lifecycle (start, pause, destroy) | **Java API Framework** | The Activity Manager in this layer creates, pauses and destroys activities and calls their callbacks. |
| Running the app's code with ART | **Native C/C++ Libraries / Android Runtime** | ART executes the compiled app code. |
| Reading the phone's temperature sensor | **HAL** (then Linux Kernel driver) | The HAL gives the framework a standard sensor interface; the kernel driver talks to the hardware. |

## Common confusions

- **ART** belongs to the *Android Runtime* part of Layer 3, not the Java API Framework.
- **Activity Manager** is in the *Java API Framework*, not in the libraries.
- **SQLite** is a native library; **Binder** is part of the kernel layer.
- Android is based on the **Linux kernel**, not Windows, Darwin or BSD.

## Quick summary

- Bottom to top: Linux Kernel → HAL → (Native Libraries | ART) → Java API Framework → (System apps | User apps).
- Drivers → Kernel. Activity Manager → Framework. ART → Runtime. Your app → User apps.
