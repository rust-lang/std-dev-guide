# Summary

[About this guide](./about-this-guide.md)

[Getting started](./getting-started.md)

---

# The feature lifecycle

- [Landing new features](./feature-lifecycle/new-unstable-features.md)
- [Using tracking issues](./feature-lifecycle/tracking-issues.md)
- [Stabilizing features](./feature-lifecycle/stabilization.md)
- [Deprecating features](./feature-lifecycle/deprecation.md)

# Code considerations

- [Reviewer checklist](./code-considerations/reviewer-checklist.md)
- [Design](./code-considerations/design/summary.md)
    - [Public APIs](./code-considerations/design/public-apis.md)
- [Breaking changes](./code-considerations/breaking-changes/summary.md)
    - [Breakage from changing behavior](./code-considerations/breaking-changes/behavior.md)
    - [Breakage from new trait impls](./code-considerations/breaking-changes/new-trait-impls.md)
    - [`#[fundamental]` types](./code-considerations/breaking-changes/fundamental.md)
- [Safety and soundness](./code-considerations/safety-and-soundness/summary.md)
    - [Generics and unsafe](./code-considerations/safety-and-soundness/generics-and-unsafe.md)
    - [Drop and `#[may_dangle]`](./code-considerations/safety-and-soundness/may-dangle.md)
    - [`std::mem` and exclusive references](./code-considerations/safety-and-soundness/mem-and-exclusive-refs.md)
- [Using unstable language features](./code-considerations/using-unstable-lang/summary.md)
    - [Const generics](./code-considerations/using-unstable-lang/const-generics.md)
    - [Specialization](./code-considerations/using-unstable-lang/specialization.md)
- [Performance](./code-considerations/performance/summary.md)
    - [When to `#[inline]`](./code-considerations/performance/inline.md)

# Tools and bots

- [`@bors`](./tools-and-bots/bors.md)
- [`@rust-timer`](./tools-and-bots/timer.md)
- [`@craterbot`](./tools-and-bots/crater.md)
