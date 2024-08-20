# Breaking changes

Breaking changes should be avoided when possible.
[RFC 1105](https://rust-lang.github.io/rfcs/1105-api-evolution.html) lays the foundations for what constitutes a breaking change.
Breakage may be deemed acceptable or not based on its actual impact,
which can be approximated with a [crater](https://github.com/rust-lang/crater/blob/master/docs/bot-usage.md) run.
Running crater should be done if nontrivial breakage is expected, so the information is
available during the final comment period.

If the impact isn't too high, looping in maintainers of to-be-broken crates and submitting PRs
to fix them can be a valid strategy. However, this can only affect the crates in question, and
it does not automatically affect their dependents. Binary dependents may have already locked-in
a different, older version.

## Breaking and the trains
If a PR is merged and it turns out to have caused code to not compile during the nightly or beta release cycle,
unless there is a trivial fix, the PR should be reverted and a crater run should assess the impact.
