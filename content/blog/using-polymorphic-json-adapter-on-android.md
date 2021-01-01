+++
content = ""
date = 2020-12-29T18:30:00Z
description = "Learn how to use polymorphic adapter with an abstract class"
draft = true
keywords = []
math = false
slug = "using-polymorphic-adapter-moshi"
tags = ["Moshi", "Polymorphic", "JSON", "Kotlin"]
title = "Using polymorphic json adapter with Moshi"
toc = true

+++
Hey, today we're gonna learn how to use the polymorphic json adapter with Moshi. Let's start with some basics around Moshi and its adapters.

Here's the [code](#creating-a-polymorphic-adapter) if you don't have time to read everything.

## What is Moshi?

Quoting Moshi's Github page here,

> Moshi is a modern JSON library for Android and Java. It makes it easy to parse JSON into Java objects.

It is a fast and modern library that can parse JSON using both codegen and reflection. It uses Okio and can also use the same buffer as Okhttp for efficiency. If you haven't used Moshi before, I highly* recommend you to try it.

<sub>* Moshi comes with a lesser number of inbuilt adapters so you might have to write some yourself.</sub>

## What are Adapters?

Moshi uses adapters for parsing the content of JSON into a primitive or a user-defined type. These adapters do all the heavy lifting and return us the data that we expect. Moshi comes with inbuilt adapters for primitives like Boolean, String, Integers, etc. However, to parse complex types we can either let Moshi handle it for us or we can take the matter into our own hands and create an adapter. We'll let Moshi handle everything for now but if you're interested in reading more about manually creating adapters, [Harsh has an awesome article for it.](https://msfjarvis.dev/posts/manually-parsing-json-with-moshi/)

### Moshi adapter using code generation

You can let Moshi create an adapter for you by annotating your class with `@JsonClass` and then passing `generateAdapter = true`. There's also an option to add `@Json` and specify the `name` when there's a difference between the JSON and the variable. Here's an example below.

```kotlin
@JsonClass(generateAdapter = true)
class Person(
    @Json(name = "full_name") val fullName: String,
    val age: Int,
)
```

There are a lot more things that you can do with Moshi and we're not gonna go into that but this how we generate an adapter with the code generation method.

<sub>* You'll have to enable `kapt` and add the [moshi-kotlin-codegen](https://github.com/square/moshi#codegen) dependency so that Moshi can generate an adapter for this class for you</sub>

### Moshi adapter using reflection

In the case of reflection, there's really nothing to do except adding a `KotlinJsonAdapterFactory()` while creating an instance of Moshi.

    var moshi = Moshi.Builder()
        // KotlinJsonAdapterFactoy() should be always added in the end 
        // so that moshi can try other adapters first.
        .add(KotlinJsonAdapterFactory())
        .build()

## Polymorphic Adapters

So, if Moshi can generate adapters for you both at build time and at runtime, why do you need a polymorphic adapter? And what even is it?

Good questions! The problem we're trying solve is that Moshi can only generate adapters when exact types are known. In the case of an abstract class, it is impossible for Moshi to know which implementation of the abstract class it should create and that's when polymorphic adapters come in.

Learning by seeing is best, so let's start dabbling in code. Suppose we have a base class called `Person` like this:

```kotlin
abstract class Person {
    abstract val name: String
    abstract val age: String
    abstract val occupation: String
}
```

and two subclasses of `Person`, `Doctor` and `Engineer`:

```kotlin
class Doctor(
    override val name: String,
    override val age: String
) : Person() {
    override val occupation: String = "Doctor"
}
```

```kotlin
class Engineer(
    override val name: String,
    override val age: String
) : Person() {
    override val occupation: String = "Engineer"
}
```

Now, if you try to parse a JSON body where the `occupation` is Doctor or Engineer, we want Moshi to create an instance of `Doctor` or `Engineer` class respectively. However, Moshi cannot automatically determine this. Let's help it along.

### Creating a polymorphic adapter

1. First, add the `moshi-adapters` dependency.

   ```kotlin
   implementation("com.squareup.moshi:moshi-adapters:<latest version>")
   ```
2. Create adapters for your subclasses (`Doctor` and `Engineer`) by either one of the methods mentioned [above 👆](#what-are-adapters).
3. Now create an adapter for your abstract class (`Person` class in this example)

   ```kotlin
   val adapterFactory = PolymorphicJsonAdapterFactory
        .of(Person::class.java, "occupation")
        .withSubtype(Doctor::class.java, "Doctor")
        .withSubtype(Engineer::class.java, "Engineer")
   ```

   Here's the main part of our polymorphic adapter. We create an instance of `PolymorphicJsonAdapterFactory` for the type `Person`, and then we specify which key Moshi should use to determine the class it should instantiate. In our case, that's `occupation`.

   After that, we add all the subclasses of `Person` and specify the values of `occupation` on which those classes should be selected. In this case, a JSON body like this:

   ```json
   {
       name: "Dr. Who",
       age: 18,
       occupation: "Doctor"
   }
   ```

   will generate an instance of the `Doctor` class.
4. The final step is to add this adapter to the builder of your `Moshi` instance.

   ```kotlin
   val moshi = Moshi.Builder()
   	    .add(adapterFactory)
   	    .build()
   ```

## Ending notes

Moshi is a powerful and fast JSON library that can perform some really cool stuff. It is built and maintained by some of the smartest developers in the Android space so try it out and have fun playing around with it :smiley: