# API Change Proposals (ACP)

Changes to the standard library's unstable API go through the libs ACP process.

The API Change Proposal process is intended to be a lightweight first step to
getting new APIs added to the standard library. The goal of this process is to
make sure proposed API changes have the best chance of success. The ACP process
accomplishes this by ensuring all changes have had a libs-api team member
second the proposal indicating that they are optimistic that the proposal will
pass its eventual FCP and by focusing the initial discussion on the problems
being solved and concrete motivating examples.

You can create an ACP in the `rust-lang/libs-team` repo using [this issue template](https://github.com/rust-lang/libs-team/issues/new?assignees=&labels=api-change-proposal%2C+T-libs-api&template=api-change-proposal.md&title=%28My+API+Change+Proposal%29).

Once a t-libs-api team member has reviewed the ACP and judged that it should
move forward they will second the ACP and initiate an ICP (inital comment
period). This initial comment period is intended to give other stake holders a
chance to participate in the initial discussion prior to starting the
implementation. Once this ICP has completed you should proceed with the
implementation of your proposal and then move on to the next step of the
feature lifecycle, creating a tracking issue.
