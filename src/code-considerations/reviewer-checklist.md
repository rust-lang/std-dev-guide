# Reviewer checklist

**Status:** Stub

Check the [getting started](../getting-started.md) guide for an introduction to developing in the standard library.

If you'd like to reassign the PR, you can:

```
r? @user
```

## Before you review

- [ ] Is this a [stabilization](../feature-lifecycle/stabilization.md) or [deprecation](../feature-lifecycle/deprecation.md)?
    - [ ] Make sure there's a completed FCP somewhere for it.
    - [ ] Ping `@rust-lang/libs` for input.

## As you review

Look out for code considerations:

- [ ] [Design](./design/summary.md)
- [ ] [Breaking changes](./breaking-changes/summary.md)
- [ ] [Safety and soundness](./safety-and-soundness/summary.md)
- [ ] [Use of unstable language features](./using-unstable-lang/summary.md)
- [ ] [Performance](./performance/summary.md)

## Before you merge

- [ ] Is the commit log tidy? Avoid merge commits, these can be squashed down.
- [ ] Can this change be rolled up?
- [ ] Is this a [new unstable feature](../feature-lifecycle/new-unstable-features.md)?
    - [ ] Create a [tracking issue](../feature-lifecycle/tracking-issues.md).
    - [ ] Update the `#[unstable]` attributes to point to it.

## When you're ready

For Libs PRs, rolling up is usually fine, in particular if it's only a new unstable addition or if it only touches docs. See the [rollup guidelines](https://forge.rust-lang/org/compiler/reviews.md#rollups) for more details on when to rollup.

To roll up:

```
@bors r+ rollup
```

or just:

```
@bors r+
```
