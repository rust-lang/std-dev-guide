# Reviewer checklist

**Status:** Stub

## Before you review

Check the [getting started](./getting-started.md) guide for an introduction to developing in the standard library.

If you'd like to reassign the PR, you can:

```
r? @user
```

- [ ] Are there any `#[stable]` attributes involved?
    - [ ] Ping `@rust-lang/libs` for input.

## As you review

Look out for:

- [ ] [Breaking changes](./code-considerations/breaking-changes/summary.md)
- [ ] [Safety and soundness](./code-considerations/safety-and-soundness/summary.md)
- [ ] [Quality](./code-considerations/quality/summary.md)
- [ ] [Use of unstable language features](./code-considerations/using-unstable-lang/summary.md)
- [ ] [Performance](./code-considerations/performance/summary.md)

## Before you merge

- [ ] Is the commit log tidy? Avoid merge commits, these can be squashed down.
- [ ] Can this change be rolled up?
- [ ] Is this a new unstable feature?
    - [ ] Create a tracking issue.
    - [ ] Update the `#[unstable]` attributes to point to it.

## When you're ready

For Libs PRs, rolling up is usually fine, in particular if it's only a new unstable addition or if it only touches docs. See the [rollup guidelines] for more details on when to rollup.

To roll up:

```
@bors r+ rollup
```

or just:

```
@bors r+
```

[rollup guidelines]: https://forge.rust-lang/org/compiler/reviews.md#rollups
