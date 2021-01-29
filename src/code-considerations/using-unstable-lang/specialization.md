# Using specialization

Specialization is currently unstable. You can track its progress [here](https://github.com/rust-lang/rust/issues/31844).

We try to avoid leaning on specialization too heavily, limiting its use to optimizing specific implementations. These specialized optimizations use a private trait to find the correct implementation, rather than specializing the public method itself. Any use of specialization that changes how methods are dispatched for external callers should be carefully considered.

As an example of how to use specialization in the standard library, consider the case of creating an `Rc<[T]>` from a `&[T]`:

```rust
impl<T: Clone> From<&[T]> for Rc<[T]> {
    #[inline]
    fn from(v: &[T]) -> Rc<[T]> {
        unsafe { Self::from_iter_exact(v.iter().cloned(), v.len()) }
    }
}
```

It would be nice to have an optimized implementation for the case where `T: Copy`:

```rust
impl<T: Copy> From<&[T]> for Rc<[T]> {
    #[inline]
    fn from(v: &[T]) -> Rc<[T]> {
        unsafe { Self::copy_from_slice(v) }
    }
}
```

Unfortunately we couldn't have both of these impls normally, because they'd overlap. This is where private specialization can be used to choose the right implementation internally. In this case, we use a trait called `RcFromSlice` that switches the implementation:

```rust
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

## For reviewers

Look out for any `default` annotations on public trait implementations. These will need to be refactored into a private dispatch trait. Also look out for uses of specialization that do more than pick a more optimized implementation.
