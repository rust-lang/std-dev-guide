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

### Model: A Trivial Fix

On 2024, March 9th, [`Context::ext`] was added in [#123203]. It adds a `&mut dyn Any` field to Context.
On 2024, May 16th regression [#125193] appeared in beta crater: `Context` was no longer `UnwindSafe`.
Some code depended on it being so, but `&mut T` is `!UnwindSafe`, so Context also became `!UnwindSafe`.

The PR to add `Context::ext` could have been reverted, but as the function is a nightly feature,
its implications for unwind safety are limited to those actually using that nightly feature.
Since nightly features are, by definition, permitted to have effects we may not want to stabilize,
the question of whether the unwind safety regression should be accepted was deferred by [#125392]
wrapping the field in `AssertUnwindSafe`. This allowed continued experimentation with the API.

[#123203]: https://github.com/rust-lang/rust/pull/123203
[#125193]: https://github.com/rust-lang/rust/pull/125193
[#125392]: https://github.com/rust-lang/rust/pull/125392
