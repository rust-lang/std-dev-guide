# Drop and `#[may_dangle]`

A generic `Type<T>` that manually implements `Drop` should consider whether a `#[may_dangle]` attribute is appropriate on `T`. The [Nomicon][dropck] has some details on what `#[may_dangle]` is all about.

If a generic `Type<T>` has a manual drop implementation that may also involve dropping `T` then dropck needs to know about it. If `Type<T>`'s ownership of `T` is expressed through types that don't drop `T` themselves such as `ManuallyDrop<T>`, `*mut T`, or `MaybeUninit<T>` then `Type<T>` also [needs a `PhantomData<T>` field][RFC 0769 PhantomData] to tell dropck that `T` may be dropped. Types in the standard library that use the internal `Unique<T>` pointer type don't need a `PhantomData<T>` marker field. That's taken care of for them by `Unique<T>`.

As a real-world example of where this can go wrong, consider an `OptionCell<T>` that looks something like this:

```rust
struct OptionCell<T> {
    is_init: bool,
    value: MaybeUninit<T>,
}

impl<T> Drop for OptionCell<T> {
    fn drop(&mut self) {
        if self.is_init {
            // Safety: `value` is guaranteed to be fully initialized when `is_init` is true.
            // Safety: The cell is being dropped, so it can't be accessed again.
            unsafe { self.value.assume_init_drop() };
        }
    }
}
```

Adding a `#[may_dangle]` attribute to this `OptionCell<T>` that didn't have a `PhantomData<T>` marker field opened up [a soundness hole](https://github.com/rust-lang/rust/issues/76367) for `T`'s that didn't strictly outlive the `OptionCell<T>`, and so could be accessed after being dropped in their own `Drop` implementations. The correct application of `#[may_dangle]` also required a `PhantomData<T>` field:

```diff
struct OptionCell<T> {
    is_init: bool,
    value: MaybeUninit<T>,
+   _marker: PhantomData<T>,
}

- impl<T> Drop for OptionCell<T> {
+ unsafe impl<#[may_dangle] T> Drop for OptionCell<T> {
```

[dropck]: https://doc.rust-lang.org/nomicon/dropck.html
[RFC 0769 PhantomData]: https://github.com/rust-lang/rfcs/blob/master/text/0769-sound-generic-drop.md#phantom-data

## For reviewers

If there's a manual `Drop` implementation, consider whether `#[may_dangle]` is appropriate. If it is, make sure there's a `PhantomData<T>` too either through `Unique<T>` or manually.
