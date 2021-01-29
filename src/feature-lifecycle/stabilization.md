# Stabilizing features

**Status:** Stub

Feature stabilization involves adding `#[stable]` attributes. They may be introduced alongside new trait impls or replace existing `#[unstable]` attributes.

Stabilization goes through the Libs FCP process, which occurs on the [tracking issue](./tracking-issues.md) for the feature.

## Before writing a PR to stabilize a feature

Check to see if a FCP has completed first. If not, either ping `@rust-lang/libs` or leave a comment asking about the status of the feature.

This will save you from opening a stabilization PR and having it need regular rebasing while the FCP process runs its course.

## Writing a stabilization PR

- Replace any `#[unstable]` attributes for the given feature with stable ones. The value of the `since` field is usually the current `nightly` version.
- Remove any `#![feature()]` attributes that were previously required.
- Submit a PR with a stabilization report.

## When there's `const` involved

Const functions can be stabilized in a PR that replaces `#[rustc_const_unstable]` attributes with `#[rustc_const_stable]` ones. The [Constant Evaluation WG](https://github.com/rust-lang/const-eval) should be pinged for input on whether or not the `const`-ness is something we want to commit to. If it is an intrinsic being exposed that is const-stabilized then `@rust-lang/lang` should also be included in the FCP.

Check whether the function internally depends on other unstable `const` functions through `#[allow_internal_unstable]` attributes and consider how the function could be implemented if its internally unstable calls were removed. See the _Stability attributes_ page for more details on `#[allow_internal_unstable]`.

Where `unsafe` and `const` is involved, e.g., for operations which are "unconst", that the const safety argument for the usage also be documented. That is, a `const fn` has additional determinism (e.g. run-time/compile-time results must correspond and the function's output only depends on its inputs...) restrictions that must be preserved, and those should be argued when `unsafe` is used.
