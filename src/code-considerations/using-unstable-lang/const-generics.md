# Using const generics

**Status:** Stub

Complete const generics are currently unstable. You can track their progress [here](https://github.com/rust-lang/rust/issues/44580).

Const generics are ok to use in public APIs, so long as they fit in the [`min_const_generics` subset](https://github.com/rust-lang/rust/issues/74878).

See [const evaluation](./const-evaluation.md) for details on what is accepted for the *implementation* of `const` functions/contexts, which may not use const generics.

## For reviewers

Look out for const operations on const generics in public APIs like:

```rust,ignore
pub fn extend_array<T, const N: usize, const M: usize>(arr: [T; N]) -> [T; N + 1] {
    ..
}
```

or for const generics that aren't integers, bools, or chars:

```rust,ignore
pub fn tag<const S: &'static str>() {
    ..
}
```
