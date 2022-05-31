# Reviewing doc changes

Most of the time, it is mostly reviewing that the documentation isn't wrong
in any way and that it follows the
[how to write documentation](./how-to-write-documentation.md) guideline.

There is however something where we need to be extra careful: stability
guarantees.

## Stability guarantees

First, short explanation about what a stability guarantee is: a statement in
the document which explains what the item is doing in a precise case. For
example:

 * Showing precisely how a function on floats handles `NaN`.
 * Saying that a sort method has a particular running-time bound.

So if a doc change updates/adds/removes a stability guarantee, it has to be
**very** carefully handled and needs to go through the
[libs API team FCP](https://rustc-dev-guide.rust-lang.org/stabilization_guide.html?highlight=fcp#fcp).

It can be circumvented by adding a `# Current Implementation` section
[like done here](https://github.com/rust-lang/rust/blob/4a8d2e3856c0c75c71998b6c85937203839b946d/library/alloc/src/slice.rs#L250).
