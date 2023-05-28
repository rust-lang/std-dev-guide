# Criteria for inclusion in `std`

**Note**: *The process for deciding what does and does not get added to the standard
library is too complex to reduce to a set of concrete rules. Ultimately we rely
upon the expertise of members of the library API team, but here we attempt to
document the reasoning that has been used historically to guide contributors
and team members and to ensure long term consistency in how these decisions are
made even as the team membership naturally changes over time.*

## Reasons For Inclusion

* APIs where there is a clear best implementation
* APIs where there is a strong benefit to having only one, such as common
  traits (e.g. `Error`, the `Fail` trait from `failure` crate showed the cost
  of competing error traits and how it causes ecosystem fractures)
* Core value propositions: APIs that we want to use in examples for our
  learning materials to demonstrate rust's core value propositions. (e.g.
  scoped threads simplifying examples demonstrating thread and memory safety)
* Unsafe primitives: APIs that reduce the need of or centralize difficult to
  implement unsafe code
* Portability: APIs that are not dependent upon any platform's abstractions or
  features

## Reasons Against Inclusion

* APIs where there are multiple legitimate alternatives with different tradeoffs
* APIs that are excessively complex or difficult to maintain (e.g. Needle api
  or `rand` crate)
* API instability
    * All new APIs will be subject to `std`'s perma 1.0 stability policy, so
      certain classes of APIs that are more likely to require breaking changes
      such as cryptography generally face a higher bar

## Reasons that may seem like criteria but aren't

* APIs that need access to the host operating system
* APIs that are in other languages' standard libraries
* APIs in crates with high download numbers

## Case Studies

### [`Mutex`](https://doc.rust-lang.org/stable/std/sync/struct.Mutex.html)

We would likely still add `Mutex` to `std` if it were proposed today.

#### For

- Core value proposition: mutex is one of the fundamental primitives for thread
  safe interior mutability and is frequently used in examples.

#### Against

### [`mpsc`](https://doc.rust-lang.org/stable/std/sync/mpsc/index.html)

We likely would not add `mpsc` to `std` if it were proposed today.

#### For

* It helped demonstrate rust's core value proposition showing of rust's
  "fearless concurrency" in examples and learning materials

#### Against

* There are many different legitimate alternative approaches with tradeoffs, it
  is not a one size fits all solution.
* It's a complex API with multiple issues and has been a large maintenance burden.
* It's currently being replaced with a completely different implementation to
  solve some of these maint issues ([93563](https://github.com/rust-lang/rust/pull/93563)).

### [`scoped threads`](https://doc.rust-lang.org/stable/std/thread/fn.scope.html)
