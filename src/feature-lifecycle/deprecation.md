# Deprecating features

**Status:** Stub

Public APIs aren't deleted from the standard library. If something shouldn't be used anymore it gets deprecated by adding a `#[rustc_deprecated]` attribute. Deprecating need to go through a Libs FCP, just like stabilizations do.

To try reduce noise in the docs from deprecated items, they should be moved to the bottom of the module or `impl` block so they're rendered at the bottom of the docs page. The docs should then be cut down to focus on why the item is deprecated rather than how you might use it.
