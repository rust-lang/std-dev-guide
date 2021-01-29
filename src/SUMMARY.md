# Summary

[About this guide](./about-this-guide.md)

[Getting started](./getting-started.md)

---

# The feature lifecycle

This section describes the processes around how standard library features are developed and stabilized.

- [Landing new features](./feature-lifecycle/new-unstable-features.md)
- [Using tracking issues](./feature-lifecycle/tracking-issues.md)
- [Stabilizing features](./feature-lifecycle/stabilization.md)
- [Deprecating features](./feature-lifecycle/deprecation.md)

# Code considerations

This section includes things to keep in mind when writing and reviewing standard library code. Most topics are relevant for both contributors and reviewers.

- [Reviewer checklist](./code-considerations/reviewer-checklist.md)
- [Design](./code-considerations/design/summary.md)
    - [Public APIs](./code-considerations/design/public-apis.md)
- [Breaking changes](./code-considerations/breaking-changes/summary.md)
    - [Breaking behavior](./code-considerations/breaking-changes/behavior.md)
    - [New trait impls](./code-considerations/breaking-changes/new-trait-impls.md)
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

This section lists tools and bots that support the development process on the standard library. Most can be interacted with through GitHub mentions.

- [`@bors`](./tools-and-bots/bors.md)
- [`@rust-timer`](./tools-and-bots/timer.md)
- [`@craterbot`](./tools-and-bots/crater.md)
