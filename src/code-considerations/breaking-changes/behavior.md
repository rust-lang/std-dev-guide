# Breakage from changing behavior

Breaking changes aren't just limited to compilation failures. Behavioral changes to stable functions generally can't be accepted. See [the `home_dir` issue](https://github.com/rust-lang/rust/pull/46799) for an example.

An exception is when a behavior is specified in an RFC (such as IETF specifications for IP addresses). If a behavioral change fixes non-conformance then it can be considered a bug fix. In these cases, `@rust-lang/libs` should still be pinged for input.

## For reviewers

Look out for changes in existing implementations for stable functions, especially if assertions in test cases have been changed.
