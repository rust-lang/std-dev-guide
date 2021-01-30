# Safety and soundness

**Status:** Stub

Unsafe code blocks in the standard library need a comment explaining why they're [ok](https://doc.rust-lang.org/nomicon). There's a lint that checks this. The unsafe code also needs to actually be ok.

The rules around what's sound and what's not can be subtle. See the [Unsafe Code Guidelines WG](https://github.com/rust-lang/unsafe-code-guidelines) for current thinking, and consider pinging `@rust-lang/libs-impl`, `@rust-lang/lang`, and/or somebody from the WG if you're in _any_ doubt. We love debating the soundness of unsafe code, and the more eyes on it the better!

## For reviewers

Look out for any unsafe blocks. If they're optimizations consider whether they're actually necessary. If the unsafe code is necessary then always feel free to ping somebody to help review it.

Look at the level of test coverage for the new unsafe code. Tests do catch bugs!
