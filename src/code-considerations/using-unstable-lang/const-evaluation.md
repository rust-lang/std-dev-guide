# Const evaluation

Rust's compile-time code evaluation story is still in an experimental state,
with a large amount of unstable surface area, including areas where there is
active design work.

The standard library (as of this writing) uses some amount of hacks to work
around language limitations for some implementations: this document provides a
framework for evaluating the cost/benefit analysis of using such workarounds to
enable adding new `const` functions/traits or `const`-ifying existing APIs.

The overall high-level guidance is to evaluate the tradeoff between enabling
experimentation in users of the standard library (or discovery of limitations
to these language features) and the costs to the maintenance and readability of
the standard library. (This is broad guidance applicable to almost any feature,
but const generics are particularly notable in terms of their potentially wide
scope).

These are good areas to consider in such an evaluation. You should think about
whether the given PR is adding value to outweigh its costs along these
dimensions, and add to the PR description what your justification for the usage
is. Consider including future plans for the feature as well: if the feature is
experimental, does it have a clear path toward stabilization in the current
form?

## Maintainability

Does making this API const require that code be hand-written which was previously automatic?

Does the new code require duplicating, for example through `#[cfg(bootstrap)]`,
increasing the risk of diverging between the two implementations (and
increasing work when bumping the bootstrap compiler)?

Will updates/bugfixes to the code require further workarounds to CTFE
limitations?

Are the changes making a decision for many parts of the standard library?  For
example, if a trait is made const, that likely means all implementations in the
standard library have to be const too. It may not be the case that this is easy
to accomplish (particularly if their current implementations are complex and
need const hacks to work around limitations in CTFE).

## Stability risks

When making an internal (or external) API `const`, it becomes harder to
evaluate whether a given standard library function is safe to stabilize as
const. One of its dependencies may take a long time to stabilize for
const-evaluation, even though its API doesn't obviously reference unstable
surface area. 

For example, a function like `[T]::get_many_mut` may rely on
sorting to guarantee distinct indices. If sorting is made unstably const, it is
difficult to know when looking at `get_many_mut` whether it is OK to
const-stabilize it: you have to inspect the full dependency tree with knowledge
of the "fully determined" const surface area.

This doesn't mean that we can't make internal functions const (even if
internally these use unstable const hacks to make that feasible), but it does
mean that when doing so we should be cautious. If the function is in widespread
use, it may be best to either avoid this or keep a non-const wrapper function
for most callsites, with a comment linking to this policy.

Like with specialization, we try to avoid stabilizing APIs that *rely* on
long-term unstable language details, so even if it seems like we will
definitely have some feature eventually, it may not make sense to expose it
from the standard library today.

## Unfamiliar syntax

The standard library is intended to be broadly readable to most Rust users. It
should be possible to drop into `[src]` labels from rustdoc or review PRs to
the standard library without being deeply involved with any language
experiment. Using unstable syntax like `~const Trait` which does not have
obvious meaning to someone who has finished documentation like the Rust book is
an impediment to this goal, so in general, keeping its usage to a minimum is
preferred.

Note that this is not purely about syntax; complex APIs or implementations need
justification too. This is especially the case if it makes it harder to
"uplift" an API out of the standard library into a regular crate (because of
unstable feature use or dragging internal helpers not directly related to the
feature), since that makes it challenging to experiment with improvements to it
in a more isolated setting.

Prefer to make code readable and digestible to 'everyday' Rustaceans. This is
also a win for maintainability and reviewability. The library team and the
library contributor teams need to understand this code too, and aren't up to
speed on every language initiative.

As an initial question to ask in this area, consider how one might find out
what a feature you're using in the standard library does. Is there up to date
documentation? Documentation isn't sufficient to make usage okay, but it's a
useful question to ask yourself when opening a PR making use of that feature.

## Decisions in this space

**Replacing a derive with a manual impl**: generally not permitted, see
[#102371](https://github.com/rust-lang/rust/issues/102371) for context. We are
waiting on const-derive being implemented to avoid an increase in manual
implementations throughout the standard library.
