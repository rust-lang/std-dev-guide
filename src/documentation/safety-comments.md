# Safety comments

Using [`unsafe`] blocks in often required in the Rust compiler or standard
library, but this is not done without rules: each `unsafe` block should have
a `SAFETY:` comment explaining why the block is safe, which invariants are
used and must be respected. Below are some examples taken from the standard
library:

[`unsafe`]: https://doc.rust-lang.org/stable/std/keyword.unsafe.html

## Inside `unsafe` elements

This one shows how an `unsafe` function can pass the requirements through to its
caller with the use of documentation in a `# Safety` section while still having
more invariants needed that are not required from callers. `clippy` has a
lint for `# Safety` sections by the way.

```rust
/// Converts a mutable string slice to a mutable byte slice.
///
/// # Safety
///
/// The caller must ensure that the content of the slice is valid UTF-8
/// before the borrow ends and the underlying `str` is used.
///
/// Use of a `str` whose contents are not valid UTF-8 is undefined behavior.
///
/// ...
pub unsafe fn as_bytes_mut(&mut self) -> &mut [u8] {
    // SAFETY: the cast from `&str` to `&[u8]` is safe since `str`
    // has the same layout as `&[u8]` (only libstd can make this guarantee).
    // The pointer dereference is safe since it comes from a mutable reference which
    // is guaranteed to be valid for writes.
    unsafe { &mut *(self as *mut str as *mut [u8]) }
}
```

[See the function on github][as_bytes_mut]

This example is for a function but the same principle applies to `unsafe trait`s
like [`Send`] or [`Sync`] for example, though they have no `# Safety` section
since their entire documentation is about why they are `unsafe`.

Note that in the Rust standard library, [`unsafe_op_in_unsafe_fn`] is active
and so each `unsafe` operation in an `unsafe` function must be enclosed in an
`unsafe` block. This makes it easier to review such functions and to document
their `unsafe` parts.

[`Send`]: https://doc.rust-lang.org/stable/std/marker/trait.Send.html
[`Sync`]: https://doc.rust-lang.org/stable/std/marker/trait.Sync.html
[as_bytes_mut]: https://github.com/rust-lang/rust/blob/a08f25a7ef2800af5525762e981c24d96c14febe/library/core/src/str/mod.rs#L278
[`unsafe_op_in_unsafe_fn`]: https://doc.rust-lang.org/rustc/lints/listing/allowed-by-default.html#unsafe-op-in-unsafe-fn

## Inside *safe* elements

Inside safe elements, a `SAFETY:` comment must not depend on anything from the
caller beside properly contruscted types and values (i.e, if your function
receive a reference that is unaligned or null, it's the caller fault if it fails
and not yours).

`SAFETY` comments in *safe* elements often rely on checks that are done before
the `unsafe` block or on type invariants, like a division by `NonZeroU8` would
not check for `0` before dividing.

```rust
pub fn split_at(&self, mid: usize) -> (&str, &str) {
    // is_char_boundary checks that the index is in [0, .len()]
    if self.is_char_boundary(mid) {
        // SAFETY: just checked that `mid` is on a char boundary.
        unsafe { (self.get_unchecked(0..mid), self.get_unchecked(mid..self.len())) }
    } else {
        slice_error_fail(self, 0, mid)
    }
}
```

[See the function on github][split_at]

[split_at]: https://github.com/rust-lang/rust/blob/a08f25a7ef2800af5525762e981c24d96c14febe/library/core/src/str/mod.rs#L570
