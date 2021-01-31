# When to add `#[must_use]`

The `#[must_use]` attribute can be applied to types or functions when failing to explicitly consider them or their output is almost certainly a bug.

As an example, `Result` is `#[must_use]` because failing to consider it may indicate a caller didn't realise a method was fallible:

```rust
// Is `check_status` infallible? Or did we forget to look at its `Result`?
check_status();
```

Operators like `saturating_add` are also `#[must_use]` because failing to consider their output might indicate a caller didn't realise they don't mutate the left-hand-side:

```rust
// A caller might assume this method mutates `a`
a.saturating_add(b);
```

Combinators produced by the `Iterator` trait are `#[must_use]` because failing to use them might indicate a caller didn't realize `Iterator`s are lazy and won't actually do anything unless you drive them:

```rust
// A caller might not realise this code won't do anything
// unless they call `collect`, `count`, etc.
v.iter().map(|x| println!("{}", x));
```

On the other hand, `thread::JoinHandle` isn't `#[must_use]` because spawning fire-and-forget work is a legitimate pattern and forcing callers to explicitly ignore handles could be a nuisance rather than an indication of a bug:

```rust
thread::spawn(|| {
    // this background work isn't waited on
});
```

## For reviewers

Look for any legitimate use-cases where `#[must_use]` will cause callers to explicitly ignore values. If these are common then `#[must_use]` probably isn't appropriate.

The `#[must_use]` attribute only produces warnings, so it can technically be introduced at any time. To avoid accumulating nuisance warnings though ping `@rust-lang/libs` for input before adding new `#[must_use]` attributes to existing types and functions.
