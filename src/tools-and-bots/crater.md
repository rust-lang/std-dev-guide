# `@craterbot`

**Status:** Stub

[Crater](https://github.com/rust-lang/crater) is a tool that can test PRs against a public subset of the Rust ecosystem to estimate the scale of potential breakage.

You can kick off a crater run by first calling:

```ignore
@bors try
```

Once that finishes, you can then call:

```ignore
@craterbot check
```

to ensure crates compile, or:

```ignore
@craterbot run mode=build-and-test
```
