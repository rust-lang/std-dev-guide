# Breakage from new trait impls

A lot of PRs to the standard library are adding new impls for already stable traits,
which can break consumers in many weird and wonderful ways.
Below are some examples of breakage from new trait impls that
may not be obvious just from the change made to the standard library.

## Inference breaks when a second generic impl is introduced

Rust will use the fact that there's only a single impl for a generic trait during inference.
This breaks once a second impl makes the type of that generic ambiguous.
Say we have:

```rust,ignore
// in `std`
impl From<&str> for Arc<str> { .. }
```

```rust,ignore
// in an external `lib`
let b = Arc::from("a");
```

then we add:

```diff
impl From<&str> for Arc<str> { .. }
+ impl From<&str> for Arc<String> { .. }
```

then

```rust,ignore
let b = Arc::from("a");
```

will no longer compile, because we've previously been relying on inference to figure out the `T` in `Box<T>`.

This kind of breakage can be ok, but a [crater](https://github.com/rust-lang/crater/blob/master/docs/bot-usage.md) run should estimate the scope.
When implementing traits known to have this problem, crater should be run before initiating FCP,
so information on the scope of the breakage is available before deciding to accept the change.
This can include, but is not limited to,

- From
- FromIterator

## Deref coercion breaks when a new impl is introduced

Rust will use deref coercion to find a valid trait impl if the arguments don't type check directly.
This only seems to occur if there's a single impl so introducing a new one may break consumers relying on deref coercion.
Say we have:

```rust,ignore
// in `std`
impl Add<&str> for String { .. }

impl Deref for String { type Target = str; .. }
```

```rust,ignore
// in an external `lib`
let a = String::from("a");
let b = String::from("b");

let c = a + &b;
```

then we add:

```diff,ignore
  impl Add<&str> for String { .. }
+ impl Add<char> for String { .. }
```

then

```rust,ignore
let c = a + &b;
```

will no longer compile, because we won't attempt to use deref to coerce the `&String` into `&str`.

This kind of breakage can be ok, but a [crater](https://github.com/rust-lang/crater/blob/master/docs/bot-usage.md) run should estimate the scope.

## `#[fundamental]` types

Type annotated with the `#[fundamental]` attribute have different coherence rules.
See [RFC 1023](https://rust-lang.github.io/rfcs/1023-rebalancing-coherence.html) for details.
That includes:

- `&T`
- `&mut T`
- `Box<T>`
- `Pin<T>`

Typically, the scope of breakage in new trait impls is limited to inference and deref-coercion.
New trait impls on `#[fundamental]` types may overlap with downstream impls and cause other kinds of breakage.

[RFC 1023]: https://rust-lang.github.io/rfcs/1023-rebalancing-coherence.html
