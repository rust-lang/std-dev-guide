# Getting started

**Status:** Stub

Welcome to the standard library!

This guide is an effort to capture some of the context needed to develop and maintain the Rust standard library. Its goal is to help members of the Libs team share the process and experience they bring to working on the standard library so other members can benefit. It’ll probably accumulate a lot of trivia that might also be interesting to members of the wider Rust community.

## Where to get help

Maintaining the standard library can feel like a daunting responsibility!

Ping the `@rust-lang/libs-impl` or `@rust-lang/libs` teams on GitHub anytime.

You can also reach out in the [`t-libs` stream on Zulip](https://rust-lang.zulipchat.com/#narrow/stream/219381-t-libs/).

## A tour of the standard library

**Status:** Stub

The standard library codebase lives in the [`rust-lang/rust`](https://github.com/rust-lang/rust) repository under the `/library` directory.

The standard library is made up of three crates that exist in a loose hierarchy:

- `core`: dependency free and makes minimal assumptions about the runtime environment.
- `alloc`: depends on `core`, assumes allocator support. `alloc` doesn't re-export `core`'s public API, so it's not strictly above it in the layering.
- `std`: depends on `core` and `alloc` and re-exports both of their public APIs.
