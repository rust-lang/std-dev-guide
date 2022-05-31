# Summary

[About this guide](./about-this-guide.md)

[Getting started](./getting-started.md)

- [The library team](./team.md)
  - [Meetings](./meetings.md)
  - [Membership](./membership.md)
  - [Reviewing](./reviewing.md)

---

- [Building and debugging libraries](./development/building-and-debugging.md)


---

- [The feature lifecycle](./feature-lifecycle/summary.md)
    - [Landing new features](./feature-lifecycle/new-unstable-features.md)
    - [Using tracking issues](./feature-lifecycle/tracking-issues.md)
    - [Stabilizing features](./feature-lifecycle/stabilization.md)
    - [Deprecating features](./feature-lifecycle/deprecation.md)

---

- [Code considerations](./code-considerations/summary.md)
    - [Design](./code-considerations/design/summary.md)
        - [Public APIs](./code-considerations/design/public-apis.md)
        - [When to add `#[must_use]`](./code-considerations/design/must-use.md)
    - [Breaking changes](./code-considerations/breaking-changes/summary.md)
        - [Breakage from changing behavior](./code-considerations/breaking-changes/behavior.md)
        - [Breakage from new trait impls](./code-considerations/breaking-changes/new-trait-impls.md)
        - [`#[fundamental]` types](./code-considerations/breaking-changes/fundamental.md)
        - [Breakage from changing the prelude](./code-considerations/breaking-changes/prelude.md)
    - [Safety and soundness](./code-considerations/safety-and-soundness/summary.md)
        - [Generics and unsafe](./code-considerations/safety-and-soundness/generics-and-unsafe.md)
        - [Drop and `#[may_dangle]`](./code-considerations/safety-and-soundness/may-dangle.md)
        - [`std::mem` and exclusive references](./code-considerations/safety-and-soundness/mem-and-exclusive-refs.md)
    - [Using unstable language features](./code-considerations/using-unstable-lang/summary.md)
        - [Const generics](./code-considerations/using-unstable-lang/const-generics.md)
        - [Specialization](./code-considerations/using-unstable-lang/specialization.md)
    - [Performance](./code-considerations/performance/summary.md)
        - [When to `#[inline]`](./code-considerations/performance/inline.md)

---

- [Documentation](./documentation/summary.md)
    - [doc alias policy](./documentation/doc-alias-policy.md)
    - [safety comments policy](./documentation/safety-comments.md)
    - [how to write documentation](./how-to-write-documentation.md)
    - [reviewing doc changes](./reviewing-doc-changes.md)

---

- [Tools and bots](./tools-and-bots/summary.md)
    - [`@bors`](./tools-and-bots/bors.md)
    - [`@rust-timer`](./tools-and-bots/timer.md)
    - [`@craterbot`](./tools-and-bots/crater.md)
