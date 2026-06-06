+++
title = "Borrow vs. AsRef"
date = 2026-06-06
description = "A short explanation of the difference between the Borrow and the AsRef traits in Rust."

[taxonomies]
tags = ["rust"]
+++
The `Borrow` and `AsRef` traits in Rust are very similar.
Both have an associated type and a single required method with the same signature.
The difference between these traits lies entirely in their contract.

<!-- more -->

Although both traits are still widely seen as equivalent, they are not.
{% margin_note(id="so-answer") %}
See for example [this StackOverflow answer](https://stackoverflow.com/a/74757579).
{% end %}

Whereas a reference borrowed via the `Borrow` trait must have the *same semantics* as the value itself,
a reference borrowed via the `AsRef` trait does not have any such requirements.

## Example

A case-insensitive string can be implemented with the *newtype pattern* on a `String`:

```rust
#[derive(Clone, Debug)]
pub struct CIString(String);
```

In order to make `CIString` case-insensitive, we need to implement the `PartialEq` and `Eq` traits,
as well as the `PartialCmp` and `Cmp` traits.
Furthermore, we will likely want to implement the `Hash` trait,
which also depends on the case-insensitive semantics.
{% margin_note(id="common-traits") %}
For brevity, I do not even mention other common methods and traits implementations &ndash; such as `Display`.
{% end %}

It is a good idea to provide an `as_str` method, and to implement the `AsRef<str>` trait accordingly:

```rust
impl CIString {
    #[inline]
    pub fn as_str(&self) -> &str {
        self.0.as_str()
    }
}

impl AsRef<str> for CIString {
    #[inline]
    fn as_ref(&self) -> &str {
        self.as_str()
    }
}
```

This allows borrowing a case-insensitive string as a `&str`.
The borrowed reference will not be case-insensitive, but that is expected.
The `AsRef` trait is in the `std::convert` module after all.

However, we must not implement the `Borrow<str>` trait.
Indeed, the contract of the `Borrow` trait says that the borrowed reference must have the same semantics as the value itself,
which in our case is that it is case-insensitive.
