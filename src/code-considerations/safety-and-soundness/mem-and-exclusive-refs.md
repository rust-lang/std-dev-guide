# Using `mem` to break assumptions

## `mem::replace` and `mem::swap`

Any value behind a `&mut` reference can be replaced with a new one using `mem::replace` or `mem::swap`, so code shouldn't assume any reachable mutable references can't have their internals changed by replacing.

## `mem::forget`

Rust doesn't guarantee destructors will run when a value is leaked (which can be done with `mem::forget`), so code should avoid relying on them for maintaining safety. Remember, [everyone poops][Everyone Poops].

It's ok not to run a destructor when a value is leaked because its storage isn't deallocated or repurposed. If the storage is initialized and is being deallocated or repurposed then destructors need to be run first, because [memory may be pinned][Drop guarantee]. Having said that, there can still be exceptions for skipping destructors when deallocating if you can guarantee there's never pinning involved.

[Drop guarantee]: https://doc.rust-lang.org/nightly/std/pin/index.html#drop-guarantee

## For reviewers

If there's a `Drop` impl involved, look out for possible soundness issues that could come from that destructor never running.
