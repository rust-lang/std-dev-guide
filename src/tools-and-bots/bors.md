# `@bors`

**Status:** Stub

PRs to the standard library aren’t merged manually using GitHub’s UI or by pushing remote branches. Everything goes through [`@bors`](https://github.com/rust-lang/homu).

You can approve a PR with:

```ignore
@bors r+
```

## Rolling up

For Libs PRs, rolling up is usually fine, in particular if it's only a new unstable addition or if it only touches docs. See the [rollup guidelines](https://forge.rust-lang/org/compiler/reviews.md#rollups) for more details on when to rollup.
