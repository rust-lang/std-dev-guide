# Relying on generic trait impls for soundness

Be careful of generic types that interact with unsafe code. Unless the generic type is bounded by an unsafe trait that specifies its contract, we can't rely on the results of generic types being reliable or correct.

A place where this commonly comes up is with the `RangeBounds` trait. You might assume that the start and end bounds given by a `RangeBounds` implementation will remain the same since it works through shared references. That's not necessarily the case though, an adversarial implementation may change the bounds between calls:

```rust
struct EvilRange(Cell<bool>);

impl RangeBounds<usize> for EvilRange {
    fn start_bound(&self) -> Bound<&usize> {
        Bound::Included(if self.0.get() {
            &1
        } else {
            self.0.set(true);
            &0
        })
    }
    fn end_bound(&self) -> Bound<&usize> {
        Bound::Unbounded
    }
}
```

This has [caused problems in the past](https://github.com/rust-lang/rust/issues/81138) for code making safety assumptions based on bounds without asserting they stay the same.

Code using generic types to interact with unsafe should try convert them into known types first, then work with those instead of the generic. For our example with `RangeBounds`, this may mean converting into a concrete `Range`, or a tuple of `(Bound, Bound)`.

## For reviewers

Look out for generic functions that also contain unsafe blocks and consider how adversarial implementations of those generics could violate safety.
