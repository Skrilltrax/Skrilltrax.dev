+++
date = 2021-04-17T18:30:00Z
description = "A version catalog is a list of dependencies that the user can pick and use inside their Gradle build scripts to declare dependencies. It allows the users to manage and share their dependencies from a centralized location."
keywords = ["Version Catalogs", "Gradle", "Dependency Management"]
math = false
slug = "centralized-dependency-management-gradle-version-catalogs"
tags = ["Gradle", "Android"]
title = "Centralized dependency management using Gradle version catalogs"
toc = true

+++
The Gradle team recently released Gradle 7.0 and it comes with a bunch of exciting new features like [native support for M1 macs](https://docs.gradle.org/7.0/release-notes.html#apple-silicon), [type-safe project accessors](), and [support for running and building Java 16](https://docs.gradle.org/7.0/release-notes.html#support-for-java-16). However, today we're gonna focus on one specific feature, Gradle version catalogs.

<sub>* Version catalogs is currently an experimental feature so the API may change in the future. </sub>

## What is a version catalog?

A version catalog is a list of dependencies that the user can pick and use inside their Gradle build scripts to declare dependencies. It allows the users to manage and share their dependencies from a centralized location.

The version catalog can be specified using a [TOML](https://toml.io/) file.

### TOML file format

The TOML file consists of 3 major sections:

* `[versions]` section is used to declare versions that can be referenced by dependencies.
* `[libraries]` section is used to declare the dependencies. The dependencies can be declared using multiple notations as found [here](https://docs.gradle.org/7.0/userguide/platforms.html#sub::toml-dependencies-format).
* `[bundles]` section is used to declare dependency bundles that can be used to import multiple dependencies together.

## Why use a version catalog?

Currently, there are multiple ways to declare and use dependencies inside our Gradle projects. Writing dependencies inside build scripts, creating separate `dependencies.gradle` file or even using `buildSrc` to share dependencies, but with all these approaches, there are certain pitfalls and gotcha's that users have to take care of, so the Gradle team created a standard way to work through this.

### Benefits

* Version catalog helps arrange dependencies inside a single file. Dependencies can be used with multi-project builds and even between completely different projects.
* Unlike `buildSrc` changing a dependency inside  the `lib.versions.toml` won't invalidate the complete `buildSrc` cache.
* TOML is a reasonably popular and language-agnostic format, so popular tools like [Dependabot](https://github.com/dependabot) can be updated to work with version catalogs once the feature is stable.

## How to use the Gradle version catalog?

There are a few steps that we have to perform to use the version catalog.

1. First, make sure you are using Gradle 7.0. If you're not, you can update your project's Gradle version using the following command.

   ```shell
   ./gradlew wrapper --gradle-version=7.0
   ```
2. Enable the feature by adding the following in your `settings.gradle(.kts)`.

   ```kotlin
   enableFeaturePreview("VERSION_CATALOGS")
   ```

   This is necessary to do because the version catalog is an experimental feature on Gradle 7.0.
3. Now, we are all set to define our dependencies. To do that, create a file named `libs.versions.toml` inside the `gradle/` directory.

   ```toml
   # gradle/libs.versions.toml
   
   [versions]
   compose = "1.0.0-beta04"
   coroutines = "1.4.3"
   lifecycle = "2.4.0-alpha01"
   
   [libraries]
   # Kotlin dependencies
   kotlin-coroutines-android = { module = "org.jetbrains.kotlinx:kotlinx-coroutines-android", version.ref = "coroutines" }
   kotlin-coroutines-core = { module = "org.jetbrains.kotlinx:kotlinx-coroutines-core", version.ref = "coroutines" }
   kotlin-coroutines-jvm = { module = "org.jetbrains.kotlinx:kotlinx-coroutines-jvm", version.ref = "coroutines" }
   
   # AndroidX dependencies
   androidx-appcompat = "androidx.appcompat:appcompat:1.3.0-rc01"
   androidx-browser = "androidx.browser:browser:1.3.0"
   androidx-coreLibraryDesugaring = "com.android.tools:desugar_jdk_libs:1.0.10"
   androidx-datastore = "androidx.datastore:datastore-preferences:1.0.0-alpha08"
   androidx-lifecycle-runtimeKtx = { module = "androidx.lifecycle:lifecycle-runtime-ktx", version.ref = "lifecycle" }
   androidx-lifecycle-viewmodelKtx = { module = "androidx.lifecycle:lifecycle-viewmodel-ktx", version.ref = "lifecycle" }
   
   [bundles]
   androidxLifecycle = ["androidx-lifecycle-runtimeKtx", "androidx-lifecycle-viewmodelKtx"]
   ```

This will generate an extension in your Gradle build with the name `libs`, which can then be used in your Gradle scripts to access dependencies. Here are a few examples 👇

```kotlin
composeOptions {
  # versions
  kotlinCompilerExtensionVersion libs.versions.compose.get() // The get() is required because the API returns a Provider<String>
}
    
dependencies {
  # libraries
  implementation(libs.kotlin.coroutines.android)
      
  # bundles
  implementation(libs.bundles.androidxLifecycle)
}
```

### Stuck somewhere?

Here's a bunch of PR's that you can look at if you need any help in migrating to version catalogs.

[**Android Password Store**](https://github.com/android-password-store/Android-Password-Store/pull/1381/files)

[**Accompanist**](https://github.com/google/accompanist/pull/345)

[**Claw**](https://github.com/msfjarvis/compose-lobsters/pull/217)

If you still need help with something, feel free to reach out on any of the platforms below.

## Conclusion

Version catalog helps in centralizing common definitions and allows sharing of dependencies and plugins. There are numerous benefits of having a standard way to manage dependencies and hopefully, it will give developers some new, powerful set of tools to work with. Go ahead, give it a try and let me know how you feel about it.

Happy Coding!