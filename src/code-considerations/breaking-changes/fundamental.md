# `#[fundamental]` types

**Status:** Stub

Type annotated with the `#[fundamental]` attribute have different coherence rules. See [RFC 1023](https://rust-lang.github.io/rfcs/1023-rebalancing-coherence.html) for details. That includes:

- `&T`
- `&mut T`
- `Box<T>`
- `Pin<T>`

Typically, the scope of [breakage in new trait impls](./new-trait-impls.md) is limited to inference and deref-coercion. New trait impls on `#[fundamental]` types may overlap with downstream impls and cause other kinds of breakage.

[RFC 1023]: https://rust-lang.github.io/rfcs/1023-rebalancing-coherence.html

## For reviewers

Look out for blanket trait implementations for fundamental types, like:

```rust
impl<'a, T> PublicTrait for &'a T
where
    T: SomeBound,
{

}
```

unless the blanket implementation is being stabilized along with `PublicTrait`. In cases where we really want to do this, a [crater] run can help estimate the scope of the breakage.

[crater]: ../../tools-and-bots/crater.md
