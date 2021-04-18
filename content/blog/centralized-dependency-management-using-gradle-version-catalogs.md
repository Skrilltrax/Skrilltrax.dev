+++
date = 2021-04-17T18:30:00Z
description = ""
draft = true
keywords = ["Version Catalogs", "Gradle", "Dependency Management"]
math = false
slug = "centralized-dependency-management-gradle-version-catalogs"
tags = ["Gradle", "Android"]
title = "Centralized dependency management using Gradle version catalogs"
toc = false

+++
The Gradle team recently released Gradle 7.0 and it comes with a bunch of exciting new features like [native support for M1 macs](https://docs.gradle.org/7.0/release-notes.html#apple-silicon), [type-safe project accessors](), and [support for running and building Java 16](https://docs.gradle.org/7.0/release-notes.html#support-for-java-16). However, today we're gonna focus on one specific feature, Gradle version catalogs.

## What is a version catalog?

A version catalog is a list of dependencies that the user can pick and use inside their Gradle build scripts to declare dependencies. It allows the users to manage and share their dependencies from a centralized location.

## Why use a version catalog?

Currently, there are multiple ways to declare and use dependencies inside our Gradle projects. Writing dependencies inside build scripts, creating separate `dependencies.gradle` file or even using `buildSrc` to share dependencies, but with all these approaches, there are certain pitfalls and gotcha's that users have to take care of, so the Gradle team created a _standard_ way to work through this.  
  
And without further ado, let's try implementing Gradle version catalogs.

## How to use the Gradle version catalog?

There are a few steps that we have to perform to use the version catalog.

1. First, make sure you are using Gradle 7.0. If you're not, you can update your project's Gradle version using the following command.

       ./gradlew wrapper --gradle-version=7.0
2. Enable the feature by adding the following in your `settings.gradle(.kts)`.  

       enableFeaturePreview("VERSION_CATALOGS")

   This is necessary to do because the version catalog is an experimental feature.
3. Now, we are all set to define our dependencies. To do that, create `libs.versions.toml` inside the root level Gradle directory.

       # gradle/libs.versions.toml
       [versions]
       accompanist = "0.7.1"
       androidx_test = "1.4.0-alpha05"
       compose = "1.0.0-beta04"
       coroutines = "1.4.3"
       lifecycle = "2.4.0-alpha01"
       retrofit = "2.9.0"
       sqldelight = "1.4.4"
       
4. 

       