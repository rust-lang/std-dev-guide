# Landing new features

**Status:** Stub

New unstable features can be added and approved without going through a Libs FCP. There should be some buy-in from Libs that a feature is desirable and likely to be stabilized at some point before landing though.

If you're not sure, open an issue against `rust-lang/rust` first suggesting the feature before developing it.

All public items in the standard library need a `#[stable]` or `#[unstable]` attribute on them. When a feature is first added, it gets a `#[unstable]` attribute.

Before a new feature is merged, those `#[unstable]` attributes need to be linked to a [tracking issue](./tracking-issues.md).
