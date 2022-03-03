# Stabilizing features

**Status:** Current
**Last Updated:** 2022/03/03

Feature stabilization involves adding `#[stable]` attributes. They may be introduced alongside new trait impls or replace existing `#[unstable]` attributes.

Stabilization goes through the Libs FCP process, which occurs on the [tracking issue](./tracking-issues.md) for the feature.

## Before writing a PR to stabilize a feature

Check to see if a FCP has completed first. If not, either ping `@rust-lang/libs` or leave a comment asking about the status of the feature.

This will save you from opening a stabilization PR and having it need regular rebasing while the FCP process runs its course.

## Partial Stabilizations

When you only wish to stabilize a subset of an existing feature your first step should be to split the feature into multiple features, each with their own tracking issues.

You can see an example of partially stabilizing a feature with tracking issues [#71146](https://github.com/rust-lang/rust/issues/71146) and [XXXXX]() with FCP and the associated implementation PR [XXXXX]().

## When there's `const` involved

Const functions can be stabilized in a PR that replaces `#[rustc_const_unstable]` attributes with `#[rustc_const_stable]` ones. The [Constant Evaluation WG](https://github.com/rust-lang/const-eval) should be pinged for input on whether or not the `const`-ness is something we want to commit to. If it is an intrinsic being exposed that is const-stabilized then `@rust-lang/lang` should also be included in the FCP.

Check whether the function internally depends on other unstable `const` functions through `#[allow_internal_unstable]` attributes and consider how the function could be implemented if its internally unstable calls were removed. See the _Stability attributes_ page for more details on `#[allow_internal_unstable]`.

Where `unsafe` and `const` is involved, e.g., for operations which are "unconst", that the const safety argument for the usage also be documented. That is, a `const fn` has additional determinism (e.g. run-time/compile-time results must correspond and the function's output only depends on its inputs...) restrictions that must be preserved, and those should be argued when `unsafe` is used.

## Writing a stabilization PR

To stabilize a feature, follow these steps:

0. (Optional) For partial stabilizations, create a new tracking issue for either the subset being stabilized or the subset being left unstable as preferred. If you're unsure which way is best ask a libs-api team member.
0. Ask a **@rust-lang/libs-api** member to start an FCP on the tracking issue and wait for the FCP to complete (with `disposition-merge`).
0. Change `#[unstable(...)]` to `#[stable(since = "version")]`. `version` should be the *current nightly*, i.e. stable+2. You can see which version is the current nightly [on Forge](https://forge.rust-lang.org/#current-release-versions).
0. Remove `#![feature(...)]` from any test or doc-test for this API. If the feature is used in the compiler or tools, remove it from there as well.
0. If applicable, change `#[rustc_const_unstable(...)]` to `#[rustc_const_stable(since = "version")]`.
0. Open a PR against `rust-lang/rust`.
   - Add the appropriate labels: `@rustbot modify labels: +T-libs-api`.
   - Link to the tracking issue and say "Closes #XXXXX".

You can see an example of stabilizing a feature with [tracking issue #81656 with FCP](https://github.com/rust-lang/rust/issues/81656) and the associated [implementation PR #84642](https://github.com/rust-lang/rust/pull/84642).

