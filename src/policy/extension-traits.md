# Why the standard library uses extension traits (and not `cfg`-guarded items)

A common pattern in the standard library is to put target-specific methods into
extension traits, rather than providing them as `cfg`-guarded methods directly
on objects themselves. For example, the many extension traits in
[`std::os::unix::prelude`](https://doc.rust-lang.org/std/os/unix/prelude/index.html)
provide UNIX-specific methods on standard types.

The standard library could, instead, provide these methods directly on the
standard types, guarded by `#[cfg(unix)]`. However, it does not do so, and PRs
adding `cfg`-guarded methods are often rejected.

Providing these methods via extension traits forces code to explicitly use
those extension traits in order to access the methods. This, effectively,
requires code to declare whether it depends on target-specific functionality,
either because the code is target-specific, or because it has appropriately
`cfg`-guarded code for different targets. Without these extension traits, code
could more easily use target-specific functionality "accidentally".

This policy may change in the future if Rust develops better mechanisms for
helping code explicitly declare its portability, or lack of portability, before
accessing target-specific functionality.
