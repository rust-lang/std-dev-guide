# Performance

**Status:** Stub

Changes to hot code might impact performance in consumers, for better or for worse. Appropriate benchmarks should give an idea of how performance characteristics change. For changes that affect `rustc` itself, you can also do a [`rust-timer`] run.

## For reviewers

If a PR is focused on performance then try get some idea of what the impact is. Also consider marking the PR as `rollup=never`.

[`rust-timer`]: https://github.com/rust-lang-nursery/rustc-perf
