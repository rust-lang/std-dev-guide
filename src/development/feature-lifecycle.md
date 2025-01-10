# The feature lifecycle

## Identifying the problem

The first step before proposing any change to the standard library is to properly identify the problem that is trying to be solved. This helps to identify cases of the [XY problem] where a better solution exists without needing any changes to the standard library.

For this reason it is helpful to focus on real problems that people are encountering, rather than theoretical concerns about API design.

[XY problem]: https://en.wikipedia.org/wiki/XY_problem

## Suitability for the standard library

Unlike third party crates on crates.io, the Rust standard library is not versioned. This means that any stable API that is added can *never* be removed or modified in a backwards-incompatible way. For this reason, the standard library maintainers place a high bar on any change to the standard library API.

APIs that are well suited to the standard library are things that require language and/or compiler support, or that extend existing standard library types. Complex APIs that are expected to evolve over time (e.g. GUI frameworks) are a poor fit due to the lack of versioning.

The API Change Proposal process is intended to be a lightweight first step to
getting new APIs added to the standard library. The goal of this process is to
make sure proposed API changes have the best chance of success. The ACP process
accomplishes this by ensuring all changes are reviewed by the library API team,
who will evaluate the proposal and accept it if they are optimistic that the proposal will
be merged and pass its eventual FCP.

You can create an ACP in the `rust-lang/libs-team` repo using [this issue template](https://github.com/rust-lang/libs-team/issues/new?assignees=&labels=api-change-proposal%2C+T-libs-api&template=api-change-proposal.md&title=%28My+API+Change+Proposal%29). This should include a sketch of the proposed API, but does not have to be the final design that will be implemented.

Note that an ACP is not strictly required: you can just go ahead and submit a pull request with an implementation of your proposed API, with the risk of wasted effort if the library team ends up rejecting this feature. However do note that this risk is always present even if an ACP is accepted, as the library team can end up rejecting a feature in the later parts of the stabilization process.

Additionally, if you submit a pull request without an ACP, you should include all of the information that would have been in the ACP in the pull request description. In particular this should include clear motivating examples and a discussion of possible alternative APIs.

## API design exploration

Once a feature is deemed suitable for inclusion in the standard library, the exact design should be iterated on to find the best way to express it as a Rust API. This iteration should happen in community forums such as [Rust internals](https://internals.rust-lang.org/) where all members of the community can comment and propose improvements.

Keep the following points in mind during the discussion:
- Try to achieve a balance between generality and specificity:
  - An overly general API tends to be difficult to use for common use cases, and has a complex API surface. This makes it difficult to review and maintain, and it may be a better fit for an external crate.
  - An overly specific API does not cover all common use cases, and may require further API changes in the future to accomodate these use cases.
- An alternative that should *always* be considered is simply adding this feature via a third party crate. This is even possible when adding new methods to standard library types by using extension traits.
- In the case of "convenience" functions which are simply shorthands for something that is already possible with existing APIs, the cost of extending the standard library surface should be weighed against the ergonomic impact of the new functions.
  - For example, too many convenience methods on a type makes navigating the documentation more difficult.
  - Additionally, consider whether this method is likely to be deprecated in the future if a language-level improvement makes it unnecessary.

The library team itself is not directly involved in this discussion, but individual members may comment to provide feedback. If significant changes have occurred since the ACP, another one may be proposed at this point to have the design validated by the library API team.

## Implementation

Once the API design space has been explored, an implementation based on the favored solution should be proposed as a pull request to the `rust-lang/rust` repository.

The pull request should include a summary of the alternatives that were considered. This is helpful for reviewers since it avoids duplicating this exploration work as part of the review. A PR submitted without this may be closed with a request to explore more alternatives.

If an ACP has not been filed for the proposed feature, the PR will need to be reviewed by the library API team to determine its suitability for the standard library.

## Tracking and stabilization

Before a PR is merged, you will be asked to open a tracking issue which will track the progress of the feature until its [stabilization](stabilization.md).

There are two exceptions to this:
- Modifications of an existing unstable API can re-use the existing tracking issue for this API.
- Changes that are instantly stable (e.g. trait implementations on stable types) do not need a tracking issue. However, such changes need extra scrutiny as there will be no chance to adjust the API during an unstable period.
