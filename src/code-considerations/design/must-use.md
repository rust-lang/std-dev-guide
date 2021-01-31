# When to add `#[must_use]`

The `#[must_use]` attribute can be applied to types or functions where failing to explicitly consider them or their output is almost certainly a bug.

As an example, `Result` is `#[must_use]` because failing to consider it may indicate a caller didn't realise a method was fallible:

```rust
// If `check_status` returns a `Result`, we might assume this
// call was infallible
check_status();
```

Operators like `saturating_add` are also `#[must_use]` because failing to consider its output might indicate a caller didn't realise they don't mutate the left hand side:

```rust
let a = 42;
let b = 13;

// A caller might assume this method mutates `a`
a.saturating_add(b);
```

Combinators produced by the `Iterator` trait are `#[must_use]` because failing to consider them might indicate a caller didn't realize `Iterator`s are lazy and won't actually do anything unless you drive them:

```rust
// A caller might not realise this code won't do anything
// unless they call `collect`, `count`, etc.
slice.iter().filter(|v| v > 10).map(|v| v + 2);
```

On the other hand, `thread::JoinHandle` isn't `#[must_use]` because spawning and forgetting background work is a legitimate pattern and forcing callers to ignore it is less likely to catch bugs:

```rust
thread::spawn(|| {
    // this background work isn't waited on
});
```

## For reviewers

The `#[must_use]` attribute only produces warnings, so it can technically be introduced at any time. To avoid accumulating nuisance warnings though ping `@rust-lang/libs` for input before adding new `#[must_use]` attributes to existing types and functions.
