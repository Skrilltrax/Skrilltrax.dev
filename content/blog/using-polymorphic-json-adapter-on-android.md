+++
content = ""
date = 2020-12-29T18:30:00Z
description = "Learn how to use polymorphic adapter with an abstract class"
draft = true
keywords = []
math = false
slug = "using-polymorphic-adapter-moshi"
tags = ["Moshi", "Polymorphic", "JSON", "Kotlin"]
title = "Using Polymorphic json adapter with Moshi"
toc = true

+++
Hey, today we're gonna learn how to use the Polymorphic Json adapter with Moshi. Let's start with some basics around Moshi and its adapters.

Here's the [code](#code) if you don't have time to read everything.

## What is Moshi?

Quoting Moshi's Github page here,

> Moshi is a modern JSON library for Android and Java. It makes it easy to parse JSON into Java objects.

It is a fast and modern library that can parse JSON using both codegen and reflection methods. It uses Okio and can also use the same buffer as Okhttp for efficiency. If you haven't used Moshi before, I highly* recommend you to try it.

<sub>* Moshi comes with a lesser number of inbuilt adapters so you might have to write some yourself.</sub>

## What are Adapters though?

Moshi uses adapters for parsing the content of JSON into a primitive or a user-defined type. These adapters do all the heavy lifting and return us the data that we expect from Moshi. Moshi comes with inbuilt adapters for primitives like Boolean, String, Integers, etc. However, to parse complex types we can either let Moshi handle it for us or we can take the matter into our own hands and create an adapter. We'll let Moshi handle everything for now but if you're interested in reading more about manually creating adapters, [Harsh has an awesome article for it.](https://msfjarvis.dev/posts/manually-parsing-json-with-moshi/)

### Moshi adapter using code generation

You can let Moshi create an adapter for you by annotating your class with `@JsonClass` and then passing `generateAdapter = true`. There's also an option to add `@Json` and specify the `name` when there's a difference between the JSON and the model. Here's an example below.

```kotlin
@JsonClass(generateAdapter = true)
class Person(
    @Json(name = "full_name") val fullName: String,
    val age: Int,
)
```

There are a lot more things that you can do with Moshi and we're not gonna go into that but this how we generate an adapter with the code generation method.

<sub>* You'll have to enable `kapt` and add `kapt("com.squareup.moshi:moshi-kotlin-codegen:1.11.0")` to your build.gradle so that Moshi can generate code for you</sub>

### Moshi adapter using reflection

In the case of reflection, there's really nothing to do except adding a `KotlinJsonAdapterFactory()` while creating an instance of Moshi.

    var moshi = Moshi.Builder()
        // KotlinJsonAdapterFactoy() should be always added in the end 
        // so that moshi can use other adapters first.
        .add(KotlinJsonAdapterFactory())
        .build()

## Polymorphic Adapters

So you might be wondering what this Polymorphic Adapter is? and why do even need it if Moshi can already generate adapters for us?

Valid point, but, Moshi can only generate adapters when proper types are known, In the case of an abstract class it is difficult for Moshi to know which derived class it should create. That's where `PolymorphicJsonAdapterFactory` comes in, Let's go through an example to understand it properly.

Suppose we have a base class called `Person` like this

```kotlin
abstract class Person {
    abstract val name: String,
    abstract val age: String,
    abstract val occupation: String
}
```

```kotlin
abstract class Person {
    abstract val name: String,
    abstract val age: String,
    abstract val occupation: String
}
```