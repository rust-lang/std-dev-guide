# When to `#[inline]`

Inlining is a trade-off between potential execution speed, compile time and code size. There's some discussion about it in [this PR to the `hashbrown` crate](https://github.com/rust-lang/hashbrown/pull/119). From the thread:

> `#[inline]` is very different than simply just an inline hint. As I mentioned before, there's no equivalent in C++ for what `#[inline]` does. In debug mode rustc basically ignores `#[inline]`, pretending you didn't even write it. In release mode the compiler will, by default, codegen an `#[inline]` function into every single referencing codegen unit, and then it will also add `inlinehint`. This means that if you have 16 CGUs and they all reference an item, every single one is getting the entire item's implementation inlined into it.

You can add `#[inline]`:

- To public, small, non-generic functions.

You shouldn't need `#[inline]`:

- On methods that have any generics in scope.
- On methods on traits that don't have a default implementation.

`#[inline]` can always be introduced later, so if you're in doubt they can just be removed.

## What about `#[inline(always)]`?

You should just about never need `#[inline(always)]`. It may be beneficial for private helper methods that are used in a limited number of places or for trivial operators. A micro benchmark should justify the attribute.

## For reviewers

`#[inline]` can always be added later, so if there's any debate about whether it's appropriate feel free to defer it by removing the annotations for a start.
