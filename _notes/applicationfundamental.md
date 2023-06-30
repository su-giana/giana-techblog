---
title: "Application Fundamentals"
---

> The Android SDK tools compile your code along with any data and resource files into an APK or an Android App Bundle(.aab).

- Difference of APK and Android App Bundle -> Android App Bundle including some additional metadata that isn't required at runtime

## App components

### Activities
> The entry point for interacting with user
- represents a single screen wigh a user interface
-> what is on-screen

### Service
> A service is a general-purpose entry point for keeping an app running in **background** for all kinds of reasons
- Such as business logic which does not appeared on screen
- **Started services** tell the system to keep them running until their work is completed
- **Bound services** run because some other app has said that it wants to make use of the service

### Broadcast receivers
> A component that lets the system deliver events to the app outside of a regular user flow so the app can repond to system-wide broadcast announcements
- So, for example, an app can schedule an alarm to post a notification to tell the user about an upcoming event. Because the alarm is delivered to a BroadcastReceiver in the app, there is no need for the app to remain running until the alarm goes off.
- Although broadcast receivers don't display a user interface, they can create a status bar notification to alert the user when a broadcast event occurs
- A broadcast receiver is implemented as a subclass of BroadcastReceiver, and each broadcast is delivered as an Intent object

### Content providers
> A content provider manages a shared set of app data that you can store in the file system, in a SQLite database, on the web, or on any other persistent storage location that your app can access

