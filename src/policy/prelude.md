Policy for inclusion in the prelude
===================================

The Rust standard library provides a "prelude": items that Rust programs can
use without having to import anything. For instance, the Rust prelude includes
[`Vec`](https://doc.rust-lang.org/std/vec/struct.Vec.html), so programs can
write `Vec::with_capacity(4)` without having to first import `Vec` from the
standard library.

Each edition of Rust has its own prelude, so that new editions of Rust may add
items without making those items available to all code in older editions of
Rust.

This policy sets out the requirements for what items we add to the prelude, and
whether we add those items to the prelude for a future edition or to the common
prelude for all Rust editions.

When to use editions
--------------------

Adding a trait to the prelude can break existing Rust code, by creating name
resolution ambiguity where none previously existed. While introducing
resolution ambiguities may be "permitted breakage", doing so is quite
disruptive, and we want to avoid doing so. Thus, we generally never add a trait
to the common prelude; we only add traits to the prelude for a new edition of
Rust.

Adding items *other* than traits to the prelude will never produce conflicts or
other compatibility issues with existing code, since we allow shadowing and
give other sources of names priority over names from the prelude. Thus, if we
choose to add a non-trait item to the prelude, we should typically add it to
the common prelude for all editions of Rust. (Exceptions to this would include
names that form part of an edition transition, such that the same name resolves
to something different in different editions.)

Criteria for including an item
------------------------------

An item included in the prelude can be used by its unqualified name, by any
Rust code. Users can look up the item by name, and easily get documentation for
it. However, in general the name should make sense without any context, such as
within a diff hunk or code sample. Any name we include in the prelude should be
one that will not confuse users if they see it unqualified, without anything
introducing the name.

In particular, the name should not be something users are likely to
misunderstand as coming from the local crate or its dependencies. While any
crate *could* define any name, the names in the prelude should be *unlikely* to
occur unqualified in another crate. (A conflict with a name typically used
qualified or in a different context is not a problem; for instance, a common
method name `Object::name`, commonly called via `expr.name`, does not
necessarily preclude adding a free function `name` to the prelude.)

We should only add an item to the prelude if some reasonable number of crates
are likely to use the item. It need not be an item used by the *majority* of
crates, but it should be reasonably frequent across the ecosystem.
