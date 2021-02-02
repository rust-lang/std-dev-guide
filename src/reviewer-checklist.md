# Reviewer checklist

**Status:** Stub

Check the [getting started](./getting-started.md) guide for an introduction to developing in the standard library.

If you'd like to reassign the PR, you can:

```ignore
r? @user
```

## Before you review

- [ ] Is this a [stabilization](./feature-lifecycle/stabilization.md) or [deprecation](./feature-lifecycle/deprecation.md)?
    - [ ] Make sure there's a completed FCP somewhere for it.
    - [ ] Ping `@rust-lang/libs` for input.

## As you review

Look out for code considerations:

- [ ] [Design](./code-considerations/design/summary.md)
- [ ] [Breaking changes](./code-considerations/breaking-changes/summary.md)
- [ ] [Safety and soundness](./code-considerations/safety-and-soundness/summary.md)
- [ ] [Use of unstable language features](./code-considerations/using-unstable-lang/summary.md)
- [ ] [Performance](./code-considerations/performance/summary.md)

## Before you merge

- [ ] Is the commit log tidy? Avoid merge commits, these can be squashed down.
- [ ] Can this change be rolled up?
- [ ] Is this a [new unstable feature](./feature-lifecycle/new-unstable-features.md)?
    - [ ] Create a [tracking issue](./feature-lifecycle/tracking-issues.md).
    - [ ] Update the `#[unstable]` attributes to point to it.

## When you're ready

Use [`@bors`](./tools-and-bots/bors.md) to merge the pull request.

To roll up:

```ignore
@bors r+ rollup
```

or just:

```ignore
@bors r+
```
