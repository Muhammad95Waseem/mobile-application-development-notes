# 01. Android Architecture

Android is an open-source, Linux-based software stack for mobile devices. It is organised into **layers**, where each layer provides services to the layer above it and hides the complexity of the layer below.

## Architecture Diagram

```
+-----------------------------------------------------------+
| LAYER 5   SYSTEM APPS            |        USER APPS       |
|           (Dialer, SMS, Camera,  |  (WhatsApp, Games,     |
|            Contacts, Browser)    |   Banking apps ...)    |
+-----------------------------------------------------------+
| LAYER 4   JAVA API FRAMEWORK                              |
|   Activity Mgr | Content Providers | View System          |
|   Resource Mgr | Notification Mgr  | Location Mgr         |
|   Package Mgr  | Telephony Mgr     | Window Mgr           |
+-----------------------------------------------------------+
| LAYER 3   NATIVE C/C++ LIBRARIES | ANDROID RUNTIME (ART)  |
|   SQLite, OpenGL ES, WebKit,     |  Core Java libraries   |
|   SSL, libc, Media Framework,    |  Ahead-of-time (AOT)   |
|   Surface Manager, FreeType      |  + JIT compilation     |
+-----------------------------------------------------------+
| LAYER 2   HARDWARE ABSTRACTION LAYER (HAL)                |
|   Audio | Bluetooth | Camera | Sensors | Display | WiFi    |
+-----------------------------------------------------------+
| LAYER 1   LINUX KERNEL                                    |
|   Drivers | Memory Mgmt | Process Mgmt | Power Mgmt |      |
|   Security | Binder IPC                                   |
+-----------------------------------------------------------+
                         HARDWARE
```

## Layer 5: System Apps | User Apps
- The topmost layer, which the user directly interacts with.
- **System apps** come pre-installed with the OS (Phone, Contacts, Messages, Camera, Settings, Browser, Email).
- **User apps** are installed by the user from the Play Store or other sources.
- All apps are written using the APIs of Layer 4 and run in their own sandbox/process.
- Any app can be replaced (e.g. a third-party keyboard or launcher can replace the default).

## Layer 4: Java API Framework
The set of Java/Kotlin APIs that app developers use. It exposes the Android features as reusable services and components.

| Component | Purpose |
|---|---|
| Activity Manager | Manages the lifecycle of activities and the back stack |
| Content Providers | Let apps share data with each other (e.g. contacts) |
| View System | Builds the UI: buttons, lists, text boxes, layouts, etc. |
| Resource Manager | Gives access to non-code resources: strings, images, layouts |
| Notification Manager | Lets apps show alerts in the status bar |
| Location Manager | Provides GPS/location updates |
| Package Manager | Knows which apps are installed, their permissions and components |
| Telephony Manager | Information about phone services, calls, SIM |
| Window Manager | Manages windows and screen display |

## Layer 3: Native C/C++ Libraries | Android Runtime
### Native C/C++ Libraries
Performance-critical code written in C/C++, exposed to the framework through Java interfaces (JNI):
- **SQLite** - lightweight database for local storage
- **OpenGL ES** - 2D/3D graphics rendering
- **WebKit** - browser engine used by WebView
- **SSL** - secure communication
- **libc** - standard C library (Bionic)
- **Media Framework** - audio/video playback and recording
- **Surface Manager** - composes the drawing surfaces onto the display
- **FreeType** - font rendering

### Android Runtime (ART)
- Executes app code. Every app runs in its own process with its own instance of the runtime.
- Converts the app's bytecode (DEX files) into machine code.
- **ART** replaced the older **Dalvik VM** (from Android 5.0 Lollipop). It uses **AOT (Ahead-Of-Time)** compilation at install time plus JIT and profile-guided compilation, giving faster startup and better battery life.
- Includes **Core Libraries**: the Java libraries (java.lang, java.util, java.io, etc.) that apps use.
- Also provides garbage collection and debugging support.

## Layer 2: Hardware Abstraction Layer (HAL)
- Provides standard interfaces that expose device hardware capabilities to the higher-level Java API framework.
- Consists of library modules, one for each hardware type (camera, audio, Bluetooth, sensors).
- When the framework asks for a hardware feature, the HAL loads the right module for that hardware.
- Benefit: the upper layers stay the same regardless of the manufacturer's hardware.

## Layer 1: Linux Kernel
- The foundation of the whole platform.
- Provides the **device drivers** (display, camera, keypad, WiFi, audio, Bluetooth, USB, flash memory).
- Handles **memory management**, **process management**, **power management**, **networking** and **security**.
- Includes **Binder IPC** for inter-process communication.
- Android modified the standard Linux kernel (wakelocks, low-memory killer, Binder, etc.).

## Key Points
- Five layers (top to bottom): Apps -> Java API Framework -> Native Libraries + ART -> HAL -> Linux Kernel.
- Developers work mostly at Layers 4 and 5.
- Layer 3 is split side-by-side into native libraries and the runtime.
- The kernel is the only layer that talks directly to hardware (through drivers).
