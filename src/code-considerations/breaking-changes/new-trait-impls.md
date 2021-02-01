# Breakage from new trait impls

A lot of PRs to the standard library are adding new impls for already stable traits, which can break consumers in many weird and wonderful ways. The following sections gives some examples of breakage from new trait impls that may not be obvious just from the change made to the standard library.

Also see [`#[fundamental]` types](./fundamental.md) for special considerations for types like `&T`, `&mut T`, `Box<T>`, and other core smart pointers.

## Inference breaks when a second generic impl is introduced

Rust will use the fact that there's only a single impl for a generic trait during inference. This breaks once a second impl makes the type of that generic ambiguous. Say we have:

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

This kind of breakage can be ok, but a [crater] run should estimate the scope.

## Deref coercion breaks when a new impl is introduced

Rust will use deref coercion to find a valid trait impl if the arguments don't type check directly. This only seems to occur if there's a single impl so introducing a new one may break consumers relying on deref coercion. Say we have:

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

This kind of breakage can be ok, but a [crater](../../tools-and-bots/crater.md) run should estimate the scope.

## For reviewers

Look out for new `#[stable]` trait implementations for existing stable traits.
