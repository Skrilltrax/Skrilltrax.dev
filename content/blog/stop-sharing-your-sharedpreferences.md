+++
date = ""
description = "SharedPreferences is one of the easiest way to share a key-value pair inside your application. However, it does not work reliably when the values are added or removed from different processes in your application."
draft = true
keywords = []
math = false
slug = ""
tags = []
title = "Stop sharing your SharedPreferences"
toc = true

+++
`SharedPreferences` is one of the most commonly used APIs in Android. It allows developers to save and retrieve key-value pairs inside their applications.

WIP  
  
Harmony, MMKV, others + Context flags

[https://cs.android.com/androidx/platform/frameworks/support/+/androidx-main:datastore/datastore-core/src/main/java/androidx/datastore/core/SingleProcessDataStore.kt](https://cs.android.com/androidx/platform/frameworks/support/+/androidx-main:datastore/datastore-core/src/main/java/androidx/datastore/core/SingleProcessDataStore.kt "https://cs.android.com/androidx/platform/frameworks/support/+/androidx-main:datastore/datastore-core/src/main/java/androidx/datastore/core/SingleProcessDataStore.kt")