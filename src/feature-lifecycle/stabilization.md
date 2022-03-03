# Stabilizing features

**Status:** Current
**Last Updated:** 2022/03/03

Feature stabilization involves adding `#[stable]` attributes. They may be introduced alongside new trait impls or replace existing `#[unstable]` attributes.

Stabilization goes through the Libs FCP process, which occurs on the [tracking issue](./tracking-issues.md) for the feature.

## Before writing a PR to stabilize a feature

Check to see if a FCP has completed first. If not, either ping `@rust-lang/libs` or leave a comment asking about the status of the feature.

This will save you from opening a stabilization PR and having it need regular rebasing while the FCP process runs its course.

## Partial Stabilizations

When you only wish to stabilize a subset of an existing feature your first step should be to split the feature into multiple features, each with their own tracking issues. How you split up that feature is situational and depends on the specific feature and how you're splitting it up, so in some cases you may want to create a new tracking issue for the portion being stabilized, and in other situations you may want to use the new tracking issue to track the portion being left unstable. If you're unsure how to split up the issue you can always ask a libs-api team member for guidance in the original tracking issue or in the [libs team zulip](https://rust-lang.zulipchat.com/#narrow/stream/219381-t-libs).

You can see an example of partially stabilizing a feature with tracking issues [#71146](https://github.com/rust-lang/rust/issues/71146) and [XXXXX]() with FCP and the associated implementation PR [XXXXX]().

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

Note that, the version number is updated to be the version number of the stable release where this feature will appear. This can be found by consulting [the forge](https://forge.rust-lang.org/#current-release-versions). Specifically, you'll want to use the version labelled "Nightly". That's two versions higher than the current stable release, as what's currently in beta will be the next stable release, and any change you're making now will be in the one after that.

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

0. (Optional) For partial stabilizations, create a new tracking issue for either the subset being stabilized or the subset being left unstable, whichever makes the most sense based on the situation.
0. Ask a **@rust-lang/libs-api** member to start an FCP on the tracking issue and wait for the FCP to complete (with `disposition-merge`).
0. Change `#[unstable(...)]` to `#[stable(since = "version")]`. `version` should be the *current nightly*, i.e. stable+2. You can see which version is the current nightly [on Forge](https://forge.rust-lang.org/#current-release-versions).
0. Remove `#![feature(...)]` from any test or doc-test for this API. If the feature is used in the compiler or tools, remove it from there as well.
0. If applicable, change `#[rustc_const_unstable(...)]` to `#[rustc_const_stable(since = "version")]`.
0. Open a PR against `rust-lang/rust`.
   - Add the appropriate labels: `@rustbot modify labels: +T-libs-api`.
   - Link to the tracking issue and say "Closes #XXXXX".

You can see an example of stabilizing a feature with [tracking issue #81656 with FCP](https://github.com/rust-lang/rust/issues/81656) and the associated [implementation PR #84642](https://github.com/rust-lang/rust/pull/84642).

