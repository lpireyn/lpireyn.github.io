+++
title = "Rust vs. Java: String"
date = 2026-03-09
description = "A comparison between the String type in Rust and Java."

[taxonomies]
tags = ["java", "rust"]
+++
As a Java developer learning Rust, my first intuition was to think of Rust's `String` type as the equivalent of Java's `String` class.
Though legitimate, this intuition is deceitful:
there are important differences between the two types.

<!-- more -->

The most important difference is that Rust's `String` is *mutable* whereas Java's `String` is not.
In that regard, it is closer to Java's `StringBuilder`:
a dynamically resizable array of characters.
{% margin_note(id="string-builder") %}
For example, the `String::push` method is equivalent to the `StringBuilder.append` method.
{% end %}

What is the Rust equivalent of Java's immutable `String` then?
Is it `&str`?

Again, there are a few important differences between the two types.
Java's instances of `String` are allocated on the heap
&ndash; as are all non-primitive types in Java &ndash;,
while Rust's `&str` is just a reference to an existing slice of characters.
{% margin_note(id="array-of-chars") %}
Actually, `&str` is a reference to an array of bytes that are guaranteed to represent valid UTF-8 characters.
{% end %}
In Java, when the `StringBuilder.toString()` method is invoked,
a new `String` must necessarily be instantiated.
In Rust, `String` (and other types) can be borrowed as `&str`,
which does not require any allocation.

Actually, `&str` is similar to the `CharSequence` interface in Java:
both provide immutable operations on a sequence of characters,
although the number of methods available in Rust far exceeds those available in Java.
`CharSequence` is implemented by `String` &ndash; obviously &ndash;
but also by classes such as `StringBuilder` or `Reader`.
{% margin_note(id="reader-interface") %}
`Reader` should really have been an interface rather than an abstract class,
but default interface method implementations were not part of the JLS at the time.
{% end %}
If its methods are sufficient,
using `CharSequence` rather than `String` as type for a parameter allows to pass instances of those classes,
which can avoid the unnecessary creation of `String` instances.

Lastly, it might be tempting to compare Rust's `&str` to Java's `char[]`,
but there again there is a catch:
all arrays are mutable in Java, while shared references in Rust are not.

## A few use cases

Let's consider a few common use cases and compare the approaches in the two languages.

### Read a string

In Java, use `CharSequence` if its methods are sufficient,
{% margin_note(id="array-char-sequence") %}
Note that there is no straightforward way to obtain a `CharSequence` for a `char[]`.
{% end %}
or `String` if you need more specific methods.

In Rust, use `&str`
or `impl AsRef<Str>` if you want your API to be more flexible.

### Modify a string without changing its length

In Java, use `char[]` or `StringBuilder`.

In Rust, use `&mut str`
or `impl AsMut<str>` if you want your API to be more flexible.

### Modify a string, possibly changing its length

In Java, use `StringBuilder`.

In Rust, use `&mut String`
or `impl AsMut<String>` if you want your API to be more flexible.

### Create a string

In Java, use string concatenation
(i.e., the `+` operator)
or `StringBuilder` if you need more complex logic such as conditions and loops.

In Rust, use `format!(...)`
or `String` if you need more complex logic such as conditions and loops.
