# Using unstable language features

The standard library codebase is a great place to try unstable language features, but we have to be careful about exposing them publicly. The following is a list of unstable language features that are ok to use within the standard library itself along with any caveats:

- [Const generics](./code-considerations/using-unstable-lang/const-generics.md)
- [Specialization](./code-considerations/using-unstable-lang/specialization.md)
- _Something missing?_ Please submit a PR to keep this list up-to-date!

## For reviewers

Look out for any use of unstable language features in PRs, especially if any new `#![feature]` attributes have been added.
