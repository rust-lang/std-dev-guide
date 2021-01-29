# Getting started

**Status:** Stub

> Everything I wish I knew before somebody gave me `r+`

This guide is an effort to capture some of the context needed to develop and maintain the Rust standard library. It’s goal is to help members of the Libs team share the process and experience they bring to working on the standard library so other members can benefit. It’ll probably accumulate a lot of trivia that might also be interesting to members of the wider Rust community.

## If you’re ever unsure…

Maintaining the standard library can feel like a daunting responsibility! Through [`highfive`], the automated reviewer assignment, you’ll find yourself dropped into a lot of new contexts.

Ping the `@rust-lang/libs-impl` or `@rust-lang/libs` teams on GitHub anytime. We’re all here to help!

## A tour of the standard library

**Status:** Stub

The standard library is made up of three crates that exist in a loose hierarchy:

- `core`: dependency free and makes minimal assumptions about the runtime environment.
- `alloc`: depends on `core`, assumes allocator support. `alloc` doesn't re-export `core`'s public API, so it's not strictly above it in the layering.
- `std`: depends on `core` and `alloc` and re-exports both of their public APIs.
