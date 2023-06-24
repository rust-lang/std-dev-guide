# The Library Team

The Rust standard library and the official `rust-lang` crates are
the responsibility of the Library Team.
The Library team makes sure the libraries are maintained,
PRs get reviewed, and issues get handled in time,
although that does not mean the team members are doing all the work themselves.
Many team members and other contributors are involved in this work,
and the team's main task is to guide and enable that work.

## The Library API Team

A very critical aspect of maintaining and evolving the standard library is its stability.
Unlike other crates, we can not release a new major version once in a while for backwards
incompatible changes. Every version of the standard library is semver-compatible
with all previous versions since Rust 1.0.

This means that we have to be very careful with additions and changes to the public interface.
We can deprecate things if necessary,
but removing items or changing signatures is almost never an option.
As a result, we are very careful with stabilizing additions to the standard library.
Once something is stable, we're basically stuck with it forever.

To guard the stability and prevent us from adding things we'll regret later,
we have a team that specifically focuses on the public API.
Every RFC and stabilization of a library addition/change goes through a FCP process
in which the members of the Library API Team are asked to sign off on the change.

The members of this team are not necessarily familiar with the implementation details
of the standard library, but are experienced with API design and understand the details
of breaking changes and how they are avoided.

## The Library Contributors

In addition to the two teams above, we also have the Library Contributors,
which is a somewhat more loosely defined team consisting of those who regularly contribute
or review changes to the standard libraries.

Many of these contributors have a specific area of expertise,
for example certain data structures or a specific operating system.

## Team Membership

The Library Team will privately discuss potential new members for itself and Library Contributors,
and extend an invitation after all members and the moderation team is on board with the potential addition.

See [Membership](./membership.md) for details.

### r+ permission

All members of the Library Team, the Library API Team, and the Library Contributors
have the permission to approve PRs, and are expected to handle this with care.
See [Reviewing](./reviewing.md) for details.

### high-five rotation

Some of the members of the team are part of the 'high-five rotation';
the list from which the high-five bot picks reviewers to assign new PRs to.

Being a member of one of the teams does not come with the expectation to be on this list.
However, members of this list should be on at least one of the three library teams.
See [Reviewing](./reviewing.md) for details.
