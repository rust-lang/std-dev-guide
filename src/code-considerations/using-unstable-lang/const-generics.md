# Using const generics

**Status:** Stub

Const generics are ok to use in public APIs, so long as they fit in the [`min_const_generics` subset](https://github.com/rust-lang/rust/pull/79135).

## For reviewers

Look out for const operations on const generics in public APIs like:

```rust
pub fn extend_array<T, const N: usize, const M: usize>(arr: [T; N]) -> [T; N + 1] {
    ..
}
```

or for const generics that aren't integers, bools, or chars:

```rust
pub fn tag<const S: &'static str>() {
    ..
}
```
