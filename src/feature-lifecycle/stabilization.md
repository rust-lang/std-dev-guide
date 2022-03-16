# Stabilizing features

* **Status:** Current
* **Last Updated:** 2022-03-10

Feature stabilization involves adding `#[stable]` attributes. They may be introduced alongside new trait impls or replace existing `#[unstable]` attributes.

Stabilization goes through the Libs FCP[^1] process, which occurs on the [tracking issue](./tracking-issues.md) for the feature.

## When is an FCP appropriate?

Once an unstable feature's API design space (e.g. alternative APIs) has been fully explored with no outstanding concerns, anyone may push for its stabilization.

If you're unsure if a feature is ready for stabilization the first step should be to ask in the relevant tracking issue and get assistance from other participants in that discussion. In some cases the tracking issue may not have many other active participants, so if you're ever having trouble getting any feedback please ping one of the [libs team reviewers](https://github.com/rust-lang/highfive/blob/master/highfive/configs/rust-lang/rust.json) directly to request assistance.

## Stabilization Report

Once a feature is ready for stabilization the first step of the FCP process is writing a stabilization report. Stabilization reports are not mandatory but they are heavily encouraged, and may be mandated by library API team members if they feel it necessary. The purpose of stabilization reports is to help reviewers more quickly make decisions and to simplify the process of documenting stabilized APIs in release notes. Stabilization reports consist of three primary sections, a implementation history, an API summary, and an experience report.

The **Implementation History** section should summarize the initial discussion during the implementation PR, every change that has been made to the feature since the initial implementation, all issues that were raised during the lifetime of the feature, and how they were resolved.

The **API Summary** section should include a precise description of what APIs are being introduced to the standard libraries. This can often be a simple link back to the top level comment if it's up to date, but in situations it may not be possible to edit the original tracking issue to fix outdated information, such as when the author of the stabilization report is not the author of the tracking issue itself. The libs team maintains a tool for this called [`cargo unstable-api`](https://github.com/rust-lang/libs-team/tree/main/tools/unstable-api) that can be used to generate these API summaries in some cases. *Note* the current implementation of this tool is fragile and does not work in all cases. We hope to have a more permanent version of this tool in the future that is built ontop of either rustdoc or rustc's own APIs.

The **Experience Report** section should include concrete usecases of users who have wanted to use the feature and who have tested that it works for their needs. The experience report should include a brief summary of the experience of using that feature. Ideally this would include links to commits or branches where the feature was integrated with their project, but this is not a requirement. Alternatively, users can provide usage examples of crates that export an identical API to the one being stabilized.

You can see examples of stabilization reports in tracking issues [#88581](https://github.com/rust-lang/rust/issues/88581#issuecomment-1054642118) and [#29553](https://github.com/rust-lang/rust/issues/29553).

## Before writing a PR to stabilize a feature

Check to see if a FCP has completed first. If not, either ping `@rust-lang/libs` or leave a comment asking about the status of the feature.

This will save you from opening a stabilization PR and having it need regular rebasing while the FCP process runs its course.

## Partial Stabilizations

When you only wish to stabilize a subset of an existing feature you should skip creating a new tracking issue and instead create a partial stabilization PR for the subset of the feature being stabilized.

If you're unsure if a feature is ready for partial stabilization the first step should be to ask in the relevant tracking issue and get assistance from other participants in that discussion. In some cases the tracking issue may not have many other active participants, so if you're ever having trouble getting any feedback please ping one of the [libs team reviewers](https://github.com/rust-lang/highfive/blob/master/highfive/configs/rust-lang/rust.json) directly to request assistance.

You can see an example of partially stabilizing a feature with tracking issue [#71146](https://github.com/rust-lang/rust/issues/71146) and partial stabilization PR [#94640](https://github.com/rust-lang/rust/pull/94640).

## When there's `const` involved

Const functions can be stabilized in a PR that replaces `#[rustc_const_unstable]` attributes with `#[rustc_const_stable]` ones. The [Constant Evaluation WG](https://github.com/rust-lang/const-eval) should be pinged for input on whether or not the `const`-ness is something we want to commit to. If it is an intrinsic being exposed that is const-stabilized then `@rust-lang/lang` should also be included in the FCP.

Check whether the function internally depends on other unstable `const` functions through `#[allow_internal_unstable]` attributes and consider how the function could be implemented if its internally unstable calls were removed. See the _Stability attributes_ page for more details on `#[allow_internal_unstable]`.

Where `unsafe` and `const` is involved, e.g., for operations which are "unconst", that the const safety argument for the usage also be documented. That is, a `const fn` has additional determinism (e.g. run-time/compile-time results must correspond and the function's output only depends on its inputs...) restrictions that must be preserved, and those should be argued when `unsafe` is used.

## Stabilization PR for Library Features

Once we have decided to stabilize a feature, we need to have a PR that actually makes that stabilization happen. These kinds of PRs are a great way to get involved in Rust, as they're typically small -- just updating attributes.

Here is a general guide to how to stabilize a feature -- every feature is different, of course, so some features may require steps beyond what this guide talks about.

### Update the stability attributes on the items

Library items are marked unstable via the `#[unstable]` attribute, like this:

```rust,ignore
#[unstable(feature = "total_cmp", issue = "72599")]
pub fn total_cmp(&self, other: &Self) -> crate::cmp::Ordering { ... }
```

You'll need to change that to a `#[stable]` attribute with a version:

```rust,ignore
#[stable(feature = "total_cmp", since = "1.61.0")]
```

Note that, the version number is updated to be the version number of the stable release where this feature will appear. This can be found by consulting [`src/version`](https://github.com/rust-lang/rust/blob/master/src/version) on the current master branch of `rust-lang/rust`. Specifically, you'll want to use the version labelled "Nightly". That's two versions higher than the current stable release, as what's currently in beta will be the next stable release, and any change you're making now will be in the one after that.

### Remove feature gates from doctests

All the doctests on the items being stabilized will be enabling the unstable feature, so now that it's stable those attributes are no longer needed and should be removed.

`````diff
 /// # Examples
 ///
 /// ```
-/// #![feature(total_cmp)]
-///
 /// assert_eq!(0.0_f32.total_cmp(&-0.0), std::cmp::Ordering::Greater);
 /// ```
`````

The most obvious place to find these is on the item itself, but it's worth searching the whole library.  Often you'll find other unstable methods that were also using it in their tests.

### Remove feature gates from the compiler

The compiler builds with nightly features allowed, so you may find uses of the feature there as well.  These also need to be removed.

```diff
 #![feature(once_cell)]
 #![feature(never_type)]
-#![feature(total_cmp)]
 #![feature(trusted_step)]
 #![feature(try_blocks)]
```

## Stabilization PR Checklist

To stabilize a feature, follow these steps:

0. Create a stabiliation report in the tracking issue for the feature being stabilized.
0. (Optional) For partial stabilizations, create a new partial stabilization PR for the subset of the issue being stabilized.
0. Ask a **@rust-lang/libs-api** member to start an FCP on the tracking issue and wait for the FCP to complete (with `disposition-merge`).
0. Change `#[unstable(...)]` to `#[stable(since = "version")]`. `version` should be the *current nightly*, i.e. stable+2. You can see which version is the current nightly in [`src/version`](https://github.com/rust-lang/rust/blob/master/src/version) on the master branch of `rust-lang/rust`.
0. Remove `#![feature(...)]` from any test or doc-test for this API. If the feature is used in the compiler or tools, remove it from there as well.
0. If applicable, change `#[rustc_const_unstable(...)]` to `#[rustc_const_stable(since = "version")]`.
0. Open a PR against `rust-lang/rust`.
   - Add the appropriate labels: `@rustbot modify labels: +T-libs-api`.
   - Link to the tracking issue by adding "Closes #XXXXX".

You can see an example of stabilizing a feature with [tracking issue #81656 with FCP](https://github.com/rust-lang/rust/issues/81656) and the associated [implementation PR #84642](https://github.com/rust-lang/rust/pull/84642).

[^1]: FCP stands for final comment period

