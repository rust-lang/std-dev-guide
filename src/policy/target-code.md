# Reviewing target-specific code

When reviewing target-specific code, depending on the [tier] of the target in
question, different level of scrutiny is expected from reviewers.

For tier 1 targets, the reviewer should perform a full review of the code.
Essentially treat the code as *not* platform specific.

For tier 2 and tier 3 targets, the reviewer should confirm that the code:

* Only affects 1 or more of such targets (i.e., is truly target-specific)
* Does not introduce new licensing hazards (e.g., license headers or similar)
* Is either proposed by a target maintainer[^1] or has pinged and received +1s
  from at least one target maintainer. Where no maintainer is present, look for
  whether the author is reputable and/or affiliated with the target in some way
  (e.g., authored original code, works for a company maintaining the target, etc.).

Note that this review does *not* include checking for correctness or for code
quality. We lack the review bandwidth or expertise to perform detailed reviews
of tier 2 and tier 3 targets.

[^1]: Target maintainers are listed for most targets in the [platform support] documentation.

[tier]: https://doc.rust-lang.org/nightly/rustc/platform-support.html
[platform support]: https://doc.rust-lang.org/nightly/rustc/platform-support.html
