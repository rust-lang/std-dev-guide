# How to write documentation

This document explains how to write documentation for the std/core public APIs.

Let's start with some general information:

### Contractions

It is common in English to have contractions such as "don't" or "can't". Do not
use these in documentation. Always write their "full form":

 * "do not" instead of "don't"
 * "cannot" instead of "can't"
 * "it would" instead of "it'd"
 * "it will" instead of "it'll"
 * "it is"/"it has" instead of "it's"
 * "you are" instead of "you're"
 * "they are" instead of "they're"
 * etc

The only exception to this rule is "let's" as it is specific/known/common enough.

The reason is simply to make the reading simpler for as many people as possible.

### When to use inline code blocks

Whenever you are talking about a type or anything code related, it should be in a
inline code block. As a reminder, a inline code block is created with backticks
(\`). For example:


```text
This a `Vec` and it has a method `push` which you can call by doing `Vec::push`.
```

### When to use intra-doc links

Intra-doc links (you can see the full explanations for the feature
[here](https://doc.rust-lang.org/rustdoc/write-documentation/linking-to-items-by-name.html))
should be used as much as possible whenever a type is mentioned.

Little note: when you are documenting an item, there is no need to link to it.
So, if you write documentation for the `String::push_str` method, there is
no need to link to the `push_str` method or the `String` type.

If you have cases like `Vec<String>`, you need to use intra-doc links on both
`Vec` and `String` as well. It would look like this:

```text
This is a [`Vec`]`<`[`String`]`>`.
```

Extra explanations: since both `Vec` and `String` are in codeblocks, `<` and `>`
should as well, otherwise it would render badly.

### Code blocks

With rustdoc, code blocks are tested (because they are treated as Rust code
blocks by default). It allows us to know if the documentation is up to date. As
such, please avoid using `ignore` as much as possible on code blocks! If you
want as a language other than Rust, simply set it in the code block tags:

````text
```text
This is not rust code!
```
````

Some special cases:
 * If the code example cannot be run (when documenting a I/O item for example),
   use `no_run`.
 * If it is expected to fail, use `should_panic`.
 * If it is expected to fail compilation (which be quite rare!), use `compile_fail`.

You can find more information about code blocks
[here](https://doc.rust-lang.org/rustdoc/write-documentation/documentation-tests.html).

## How to write documentation for a module

A module is supposed to contain "similar" items. As such, its documentation is
supposed to give an overview and eventually **a base** to understand what the
items it contains are doing.

You can take a look at the
[f32 module](https://doc.rust-lang.org/nightly/std/f32/index.html) or at the
[fmt module](https://doc.rust-lang.org/nightly/std/fmt/index.html) to see
good examples.

## How to write documentation for functions/methods

The basic format of each documented methods/functions should roughly look like this:

```text
[explanations]

[example(s)]
```

### Explanations

By `explanations` we mean that the text should explain what the method and what
each of its arguments are for. Let's take this method for example:

```rust,ignore
pub fn concat_str(&self, s: &str) -> String {
    if s.is_empty() {
        panic!("empty concat string");
    }
    format!("{}{}", self.string, s)
}
```

The explanation should look like this:

```text
Returns a new [`String`] which contains `&self` content with `s` added at the end.
```

### Panic?

If the function/method can panic in certain circumstances, it must to be
mentioned! This explanation needs to be prepended by a `Panics` title:

```text
# Panics

`concat_str` panics if `s` is empty.
```

### Examples

As for the examples, they have to show the usage of the function/method. Just
like the `panic` section, they need to be prepended by a `Examples` title.

It is better if you use `assert*!` macros at the end to ensure that the example
is working as expected. It also allows the readers to understand more easily
what the function is doing (or returning).

````text
# Examples

```
let s = MyType::new("hello ");
assert_eq!("hello Georges", s.concat_str("Georges").as_str());
```
````

## How to write documentation for other items

It is mostly the same as for methods and functions except that the examples
are (strongly) recommended and not mandatory.

A good example often shows how to create the item.
