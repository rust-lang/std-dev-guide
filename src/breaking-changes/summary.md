# Breaking changes

Breaking changes should be avoided when possible.
[RFC 1105](https://rust-lang.github.io/rfcs/1105-api-evolution.html) lays the foundations for what constitutes a breaking change.
Breakage may be deemed acceptable or not based on its actual impact,
which can be approximated with a [crater](https://github.com/rust-lang/crater/blob/master/docs/bot-usage.md) run.

If the impact isn't too high, looping in maintainers of broken crates and submitting PRs to fix them can be a valid strategy.
