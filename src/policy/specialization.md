# Using specialization

Specialization is currently unstable. You can track its progress [here](https://github.com/rust-lang/rust/issues/31844).

We try to avoid leaning on specialization too heavily, limiting its use to optimizing specific implementations. These specialized optimizations use a private trait to find the correct implementation, rather than specializing the public method itself. Any use of specialization that changes how methods are dispatched for external callers should be carefully considered.

As an example of how to use specialization in the standard library, consider the case of creating an `Rc<[T]>` from a `&[T]`:

```rust,ignore
impl<T: Clone> From<&[T]> for Rc<[T]> {
    #[inline]
    fn from(v: &[T]) -> Rc<[T]> {
        unsafe { Self::from_iter_exact(v.iter().cloned(), v.len()) }
    }
}
```

It would be nice to have an optimized implementation for the case where `T: Copy`:

```rust,ignore
impl<T: Copy> From<&[T]> for Rc<[T]> {
    #[inline]
    fn from(v: &[T]) -> Rc<[T]> {
        unsafe { Self::copy_from_slice(v) }
    }
}
```

Unfortunately we couldn't have both of these impls normally, because they'd overlap. This is where private specialization can be used to choose the right implementation internally. In this case, we use a trait called `RcFromSlice` that switches the implementation:

```rust,ignore
impl<T: Clone> From<&[T]> for Rc<[T]> {
    #[inline]
    fn from(v: &[T]) -> Rc<[T]> {
        <Self as RcFromSlice<T>>::from_slice(v)
    }
}

/// Specialization trait used for `From<&[T]>`.
trait RcFromSlice<T> {
    fn from_slice(slice: &[T]) -> Self;
}

impl<T: Clone> RcFromSlice<T> for Rc<[T]> {
    #[inline]
    default fn from_slice(v: &[T]) -> Self {
        unsafe { Self::from_iter_exact(v.iter().cloned(), v.len()) }
    }
}

impl<T: Copy> RcFromSlice<T> for Rc<[T]> {
    #[inline]
    fn from_slice(v: &[T]) -> Self {
        unsafe { Self::copy_from_slice(v) }
    }
}
```

Only specialization using the `min_specialization` feature should be used. The full `specialization` feature is known to be unsound.

## Specialization attributes

There are two unstable attributes that can be used to allow a trait bound in a specializing implementation that does not appear in the default implementation.

`rustc_specialization_trait` restricts the implementations of a trait to be "always applicable". Implementing traits annotated with `rustc_specialization_trait` is unstable, so this should not be used on any stable traits exported from the standard library. `Sized` is an exception, and can have this attribute because it already cannot be implemented by an `impl` block.
**Note**: `rustc_specialization_trait` only prevents incorrect monomorphizations, it does not prevent a type from being coerced between specialized and unspecialized types which can be important when specialization must be applied consistently. See [rust-lang/rust#85863](https://github.com/rust-lang/rust/issues/85863) for more details.

`rustc_unsafe_specialization_marker` allows specializing on a trait with no associated items. The attribute is `unsafe` because lifetime constraints from the implementations of the trait are not considered when specializing. The following example demonstrates a limitation of `rustc_unsafe_specialization_marker`, the specialized implementation is used for *all* shared reference types, not just those with `'static` lifetime. Because of this, new uses of `rustc_unsafe_specialization_marker` should be avoided.

```rust,ignore
#[rustc_unsafe_specialization_marker]
trait StaticRef {}

impl<T> StaticRef for &'static T {}

trait DoThing: Sized {
    fn do_thing(self);
}

impl<T> DoThing for T {
    default fn do_thing(self) {
        // slow impl
    }
}

impl<T: StaticRef> DoThing for T {
    fn do_thing(self) {
        // fast impl
    }
}
```

`rustc_unsafe_specialization_marker` exists to allow existing specializations that are based on marker traits exported from `std`, such as `Copy`, `FusedIterator` or `Eq`.
