# Activities

* All activities are declared on the **Manifest**.

* You can invoke any activity from another App through an **intent** using its **explicit name** (unless they are defined with `exported="false"`)

* Activities might define **intent-filters** in order to be eligible to perform an action when another App launches an **implicit intent**. An activity with an intent-filter is **auto-exported** (unless `exported="false"`)

## Activity declaration

```xml
<activity android:name=".ExampleActivity" android:icon="@drawable/app_icon">
    <intent-filter>
        <action android:name="android.intent.action.SEND" />
        <category android:name="android.intent.category.DEFAULT" />
        <data android:mimeType="text/plain" />
    </intent-filter>
</activity>
```

## Invoking the activity with an implicit intent

```java
// Create the text message with a string
Intent sendIntent = new Intent();
sendIntent.setAction(Intent.ACTION_SEND);
sendIntent.setType("text/plain");
sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
// Start the activity
startActivity(sendIntent);
```

**Activity permissions** - https://developer.android.com/guide/components/activities/intro-activities.html#dp

## Security Recommendations

### You start an activity

* Be careful with **implicit intents**.
* Always use **explicit intents for your own components.**
* Do not put sensitive data on **implicit intents**.

**Relevant methods**

`startActivity()`

### Your activity is started

* Exported activities can be started by other apps on the device which may break user
  workflow assumptions, including ways that break security boundaries.
* The activity uses **the data from the intent** to perform some sensitive action.

**Relevant methods**

`getIntent()`

`intent.getExtra()`

# Services

A *service* is a general-purpose entry point for keeping an app running in the background for all kinds of reasons. 

There are actually **two very distinct semantics services** tell the system about how to manage an app:

- Started services tell the system to keep them running until their work is completed.
- **Bound services** run because some other app (or the system) has said that it wants to make use of the service.  This is basically the service providing an API to another process.  The system thus knows there is a dependency between these processes, so if process A is bound to a service in process B, it knows that it needs to keep process B (and its service) running for A.

## Security Recommendations

![Caution 3](image-20191221225501979.png)

### You start a service

* Always use an **explicit intent**.

**Relevant methods**

`bindService()`

`startService()`

### Your service is started

* Do not declare **intent filters** for your services. 
* Declare your services with `exported="false"`.

# Broadcast Receivers

# Content Providers

# Intents and intent filters

## 	Invoking components through intents

**Intents** are used to invoke *activities*, *services* and *send broadcasts* to another app. 

![Intent types](image-20191221225317413.png)

### Explicit

```java
Intent downloadIntent = new Intent(this, DownloadService.class);
downloadIntent.setData(Uri.parse(fileUrl));
startService(downloadIntent);
```

> **Note:** An explicit intent is always delivered to its target, regardless of any intent filters the component declares.

> Using an intent filter isn't a secure way to prevent other apps from starting your components. Although intent filters restrict a component to respond to only certain kinds of implicit intents, another app can potentially start your app component by using an explicit intent if the developer determines your component names. If it's important that *only your own app* is able to start one of your components, do not declare intent filters in your manifest. Instead, set the [`exported`](https://developer.android.com/guide/topics/manifest/activity-element.html#exported) attribute to `"false"` for that component.

### Implicit

An implicit intent specifies an action that can invoke any app on the device able to perform the action. 

```java
// Create the text message with a string
Intent sendIntent = new Intent();
sendIntent.setAction(Intent.ACTION_SEND);
sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
sendIntent.setType("text/plain");

// Verify that the intent will resolve to an activity
if (sendIntent.resolveActivity(getPackageManager()) != null) {
    startActivity(sendIntent);
}
```

![Caution 1](image-20191221230351316.png)

![Caution 2](image-20200107221142576.png)

* Data
* Actions
* Category
* Type

## Defining intent-filters

Each app can define **intent-filters** for its *activities*, *services*, *broadcast receivers*.

### Manifest

By default any activity that does not have the attribute `exported="false"` explicitly could be invoked **implicitly** if it defines any action filter.

`android.intent.action.*`

`android.intent.extra`

### Programmatically

```JAVA

```

## References

> Intents filters - https://developer.android.com/guide/components/intents-filters

> https://developer.android.com/guide/components/intents-filters#Receiving

> Intents common - https://developer.android.com/guide/components/intents-common

> Android intent hijacking - https://attack.mitre.org/techniques/T1416/

# Broadcasts

https://developer.android.com/guide/components/broadcasts

## Sending a broadcast

`sendBroadcast`

`sendOrderedBroadcast`

`LocalBroadcastManager.sendBroadcast`

## Local broadcast

-  You know that the data you are broadcasting won't leave your app, so don't need to worry about leaking private data. 
-  It is not possible for other applications to send these broadcasts to your app, so you don't need to worry about having security holes they can exploit. 
-  It is more efficient than sending a global broadcast through the system. 

## Listening for broadcasts

# 	Activities

Allowing an App to invoke you activities - https://developer.android.com/training/basics/intents/filters.html

Setup an activity's intent-filter to receive simple data from other Apps - https://developer.android.com/training/sharing/receive.html

# Content Provider

## Permissions

A provider's application can specify permissions that other applications must have in order to    access the provider's data. These permissions ensure that the user knows what data    an application will try to access. Based on the provider's requirements, other applications    request the permissions they need in order to access the provider. End users see the requested    permissions when they install the application.

​    If a provider's application doesn't specify any permissions, then other applications have no    access to the provider's data. However, components in the provider's application always have    full read and write access, regardless of the specified permissions.

------

**Note:** To access a provider, your application usually has to request specific    permissions in its manifest file. This development pattern is described in more detail in the  section [Content Provider Permissions](https://developer.android.com/guide/topics/providers/content-provider-basics#Permissions).

------

## Finding APPs Content Providers

**Content Providers implements the backend that provides the data.**

> Content Providers on base framework - https://github.com/aosp-mirror/platform_frameworks_base/tree/6bebb8418ceecf44d2af40033870f3aabacfe36e/core/java/android/provider

`extends ContentProvider `

`"content://"`

`public static final Uri CONTENT_URI = Uri.parse("content://"`

`import android.content.ContentProvider;`

## Finding APPs Content Resolvers

**Content Resolvers implements a client for a Content Provider.**

> Content Resolvers on base framework - https://github.com/aosp-mirror/platform_frameworks_base/tree/6bebb8418ceecf44d2af40033870f3aabacfe36e/core/java/android/content

`import android.content.ContentResolver;`

`import android.provider.`

`extends ContentResolver`

`Cursor query(`

## Finding APPs queries to Content Providers

`ActivityCompat.shouldShowRequestPermissionRationale`

`ActivityCompat.requestPermissions`

`onRequestPermissionsResult`

`getContentResolver();`

## Testing a content provider

```bash
adb shell 'content write --uri content://settings/system/ringtone_cache' < host.ogg
```

# Media Store

https://developer.android.com/reference/android/provider/MediaStore.html

`MediaStore`

# External Storage

```
https://developer.android.com/training/articles/security-tips#ExternalStorage
```

# SD card

![SD Card 1](image-20191222161735680.png)

**So, any app with READ|WRITE_EXTERNAL_STORAGE can tamper with data stored on the sd card. Data in shared storage can be accessed by any app without special permissions.**

# Shared storage

https://developer.android.com/training/data-storage

https://developer.android.com/training/data-storage/shared/documents-files

# References

https://developer.android.com/guide/components/fundamentals

> Security tips - https://developer.android.com/training/articles/security-tips

> Interacting with Other Apps - https://developer.android.com/training/basics/intents/index.html

> Sharing simple data - https://developer.android.com/training/sharing/index.html

> Intent spoofing - http://blog.palominolabs.com/2013/05/13/android-security/index.html

https://www.usenix.org/system/files/conference/usenixsecurity15/sec15-paper-ren-chuangang.pdf

> Allow install on sd card - https://developer.android.com/guide/topics/data/install-location

> Content Providers - https://developer.android.com/guide/topics/providers/content-provider-basics

> Android Open Source Project - https://github.com/aosp-mirror/platform_frameworks_base