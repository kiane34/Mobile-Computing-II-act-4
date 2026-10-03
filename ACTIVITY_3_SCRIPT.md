# Activity 3 Presentation Script

## Dynamic Performance Throttle App

### Introduction

Hello everyone. This is Activity 3, the **Network Diagnostic Dashboard**.

The purpose of this feature is to monitor the quality of the user's internet connection in real time. The app measures latency, download speed, upload speed, and packet loss. It then uses those results to decide whether the application should display high-resolution multimedia or lighter placeholder content.

This helps the application remain responsive even when the user has a slow or unstable connection.

### Opening the Dashboard

From the home dashboard, I open **Activity 3: Network Diagnostic Dashboard**.

When the screen opens, the diagnostic service immediately starts a test. The service also repeats the test automatically every minute. I can use the **Run diagnostic now** button to start another test manually.

The current connection category appears at the top of the screen. Before the first successful test, the status is shown as unknown. If the connection has severe problems or the test endpoints cannot be reached, the status becomes **Degraded**.

### Diagnostic Sequence

The diagnostic runs in three stages.

First, it performs the **baseline idle ping**. This sends a small request and measures how long the connection takes to respond before a large transfer begins. The result is displayed in milliseconds. A lower ping generally means a more responsive connection.

Second, it measures **download bandwidth and download ping**. The app downloads a test payload and calculates the approximate number of megabits transferred per second. At the same time, it tracks latency during the download phase.

Third, it measures **upload bandwidth and upload ping**. The app uploads a test payload and calculates the approximate upload speed while monitoring the connection response time.

Each stage is displayed in the diagnostic sequence section. The active stage is highlighted, and completed stages receive a check mark.

### Connection Health Classification

After all three stages finish, the service classifies the connection using the measured throughput and latency:

- **Excellent** means the effective speed is above 10 megabits per second.
- **Fair** means the effective speed is between 2 and 10 megabits per second.
- **Poor** means the effective speed is below 2 megabits per second.
- **Degraded** means there is heavy packet loss, extreme latency, a timeout, or no available diagnostic endpoint.

The dashboard also displays packet loss and the measured idle ping so the result is easier to understand.

### Global State Management

The diagnostic result is not kept only inside this screen. It is stored in the global `AppStateProvider` using the Provider package.

When a new snapshot is published, the Provider notifies the rest of the application. This means other screens or widgets can respond to the current network health without creating their own separate diagnostic process.

The global snapshot contains the current phase, health category, ping measurements, bandwidth measurements, packet loss, completion time, and any error details.

### Dynamic Performance Throttle

The final part of the activity is the adaptive interface.

When the connection is Excellent or Fair, the app selects **high-resolution media mode** because the connection can support heavier content.

When the connection is Poor or Degraded, the app selects **lightweight media mode** and shows placeholders instead. This reduces unnecessary data usage and helps keep the interface responsive.

The important point is that this decision is based on measured connection health rather than a fixed device setting.

### Error Handling

Network tests can fail because of DNS problems, unavailable servers, missing permissions, or a disconnected device. The service handles probe failures independently and tries fallback endpoints for ping, download, and upload tests.

If all endpoints are unavailable, the dashboard completes with a Degraded result and shows the error details instead of crashing the application.

For Android release builds, the application also includes the `INTERNET` permission required for these HTTPS requests.

### Closing

In summary, Activity 3 demonstrates a practical performance-aware Flutter feature. It performs regular network diagnostics, categorizes connection health, broadcasts the result through Provider, and adapts the user interface from high-resolution content to lightweight placeholders.

This approach improves reliability and gives the application a better experience across fast, slow, and unstable networks.
