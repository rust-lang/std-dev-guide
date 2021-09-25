# Reviewing

Every member of the Library Team, Library API Team, and Library Contributors has 'r+ rights'.
That is, the ability to approve a PR and instruct [`@bors`](https://bors.rust-lang.org/)
to test and merge it into Rust nightly.

If you decide to review a PR, thank you!
But please keep in mind:

- You are always welcome to review any PR, regardless of who it is assigned to.
  However, do not approve PRs unless:
    - You are confident that nobody else wants to review it first. If you think someone else on the team would be a better person to review it, feel free to reassign it to them.
    - You are confident in that part of the code.
    - You are confident it will not cause any breakage or regress performance.
    - It does not change the public API, including any stable promises we make in documentation, unless there's a finished FCP for the change.
      - For unstable API changes/additions, it can be acceptable to skip the RFC process if the design is small and the change is uncontroversial.
        Make sure to involve `@rust-lang/libs-api` on such changes.
- Always be polite when reviewing: you are a representative of the Rust project, so it is expected that you will go above and beyond when it comes to the Code of Conduct.

## High-five rotation

Some of the members of the team are part of the 'high-five rotation';
the list from which the high-five bot picks reviewers to assign new PRs to.

Being a member of one of the teams does not come with the expectation to be on this list.
However, members of this list should be on at least one of the three library teams.

If the bot assigns you a PR for which you do not have the time or expertise to review it,
feel free to reassign it to someone else.
To assign it to another random person picked from the high-five rotation,
use `r? rust-lang/libs`.

If you find yourself unable to keep up with reviews,
it might be a good idea to (temporarily) remove yourself from the list.
To add or remove yourself from the list, send a PR to change the
[high-five configuration file](https://github.com/rust-lang/highfive/blob/master/highfive/configs/rust-lang/rust.json).
