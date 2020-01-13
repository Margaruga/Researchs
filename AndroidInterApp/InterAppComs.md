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

### Data

### Actions

### Category

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

# Services

![Caution 3](image-20191221225501979.png)

**Note**: To avoid inadvertently running a different app's `Service`, always use an explicit intent to start your own service.

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

`extends ContentProvider `

`public static final Uri CONTENT_URI = Uri.parse("content://"`

## Empty content provider

```java
/*
 * Copyright (C) 2008 The Android Open Source Project
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package com.android.frameworks.coretests.enabled_app;

import android.content.ContentProvider;
import android.content.ContentValues;
import android.database.Cursor;
import android.net.Uri;

/**
 * Empty ContentProvider for testing
 */

public class DisabledProvider extends ContentProvider {

    public boolean onCreate() {
        return false;
    }

    public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs,
                        String sortOrder) {
        return null;
    }

    public String getType(Uri uri) {
        return null;
    }

    public Uri insert(Uri uri, ContentValues values) {
        return null;
    }

    public int delete(Uri uri, String selection, String[] selectionArgs) {
        return 0;
    }

    public int update(Uri uri, ContentValues values, String selection, String[] selectionArgs) {
        return 0;
    }
}
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

> Security tips - https://developer.android.com/training/articles/security-tips

> Interacting with Other Apps - https://developer.android.com/training/basics/intents/index.html

> Sharing simple data - https://developer.android.com/training/sharing/index.html

> Intent spoofing - http://blog.palominolabs.com/2013/05/13/android-security/index.html

https://www.usenix.org/system/files/conference/usenixsecurity15/sec15-paper-ren-chuangang.pdf

> Allow install on sd card - https://developer.android.com/guide/topics/data/install-location

> Content Providers - https://developer.android.com/guide/topics/providers/content-provider-basics