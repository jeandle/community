# Jeandle Community Membership

- [Decision-Making Process](#decision-making-process)
- [Membership Management](#membership-management)
  - [Adding New Committers/Maintainers](#adding-new-committersmaintainers)
  - [Removing Inactive Committers/Maintainers](#removing-inactive-committersmaintainers)
  - [Voluntary Resignation](#voluntary-resignation)
- [Voting Mechanisms](#voting-mechanisms)
  - [Consensus Voting](#consensus-voting)
  - [Maximum Representation Limit](#maximum-representation-limit)
  - [Vacancies](#vacancies)
- [Community Bi-Weekly Meeting](#community-bi-weekly-meeting)
  - [Meeting Schedule](#meeting-schedule)
  - [Meeting Agenda](#meeting-agenda)
  - [Participation and Procedures](#participation-and-procedures)

This document outlines the responsibilities of different contributor roles within the Jeandle community. Our project consists of multiple sub-projects, and the responsibilities of each role are tied to these specific sub-projects and their code repositories.

For detailed information on community member roles, responsibilities, and eligibility requirements, please refer to our [Contributor Ladder](CONTRIBUTOR_LADDER.md).

## Decision-Making Process

Jeandle is an open source project committed to transparent operations. Our code repositories are the single source of truth for everything—our values, designs, documentation, roadmap, and interfaces.

In short, every decision is implemented by updating the project. Any change affecting Jeandle, large or small, follows these simple steps:

- **Step 1**: Propose a change or raise a concern by creating an issue in the relevant repository.
- **Step 2**: Discuss the issue in the comments.
- **Step 3**: Merge or close the issue based on community consensus.

For most issues (except for those regarding new memberships):

If we have fewer than seven active members, an issue can be merged under the following conditions:

- At least one active maintainer comments "LGTM" (Looks Good To Me).
- No other member objects to the change.

Additionally, when we have fewer than seven active members, we may adopt a "lazy consensus period" for important issues. This means that after an issue receives initial approval, we allow a period (e.g., 7 days) for everyone to have a final opportunity for discussion. If no one objects, it can be closed. If issues arise, we address the concerns and then proceed.

## Membership Management

Our members share the responsibility for the project's success. Collectively, they:

- Are responsible for the project's outcomes.
- Are committed to the long-term improvement of the project.
- Dedicate time to all necessary tasks, not just the interesting ones.

Members often work hard behind the scenes, and their efforts are not always visible. While it's easy to get excited about new features, it is equally important—and often more challenging—to handle bug fixes, minor improvements, stability optimizations, and all the other foundational work that keeps the project robust.

### Adding New Committers/Maintainers

Committers/Maintainers are initially dedicated contributors committed to the long-term success of the project. If you aspire to become a Committer/Maintainer, you should be actively involved in resolving issues, contributing code, and reviewing proposals and pull requests for at least two months.

Membership is built on trust, which is about more than just writing code. You need to earn the trust of existing Committers/Maintainers by consistently acting in the best interests of the project.

New Committers/Maintainers are nominated by existing ones (via an issue) and require a supermajority vote from existing members for approval. Similarly, Committers/Maintainers can be removed by a supermajority vote, or they can choose to step down voluntarily by creating an issue to inform other members. For details on how Committers/Maintainers are elected and their terms, see the [Election Process](#election-process).

### Removing Inactive Committers/Maintainers

If an existing Committer/Maintainer no longer meets the role's expectations, they can be removed from the active list. Any other Committer/Maintainer can propose their removal via a pull request if the member meets either of the following criteria:

- They have not participated in community activities for over three months.
- They have violated governance rules more than twice.

Once the conditions are confirmed, the Committer/Maintainer can be removed. If someone is removed, we will add them to the alumni section of this document to recognize their past contributions.

### Voluntary Resignation

If a member wishes to voluntarily step down from their position, they can notify the community of their resignation at any time by creating an issue in the community repository. In this case:

1.  **Create a Resignation Notice**
    * Post an issue in the community repository.
    * Provide at least 7 days' notice before the last working day.
    * Clearly state the specific resignation date.
2.  **Provide Handover Details**
    * Share handover information.
    * Explain any ongoing responsibilities.

The resignation will be publicly announced and remain visible for at least 7 days. During this period:

- No formal vote is required.
- The resigning member should include any relevant handover information in the notice.
- Other members should acknowledge the resignation with a message of thanks.
- The resigning member will be added to the alumni section of this document to recognize their contributions.

After the resignation date, the member and their GitHub ID will be moved to the emeritus section of the `MEMBERSHIP.md` file to honor their contributions. This process ensures a proper handover and recognition while providing adequate notice to the community.

## Voting Mechanisms

Many role promotions and project decisions are approved or confirmed through the following voting methods:

- **Voting Period**: Votes typically last for two weeks unless otherwise specified.
- **Changing Votes**: Voters can change their vote at any time during the voting period, with only the latest vote being counted.
- **Principle of Integrity**: Individuals participating in a vote must act in good faith.
- **Obligation to Respond**: The person proposing an action must promptly respond to all questions and objections raised during the vote.
- **Transparency**: Unless otherwise specified, all votes must be conducted on the appropriate public project mailing list or channel. The person who called the vote is responsible for announcing the results at the end of the voting period.

### Consensus Voting

Used for decisions that do not require the full attention of all eligible voters.

- **Options**: Yes, Veto, Abstain.
- **Veto Requirement**: A veto must be accompanied by a justification; a veto without a reason is invalid. If a veto is cast, the vetoer and the proposer must collaborate to find a mutually acceptable solution. If the veto is resolved, the vetoer must explicitly withdraw the veto or cast a "Yes" vote.
- **Types**:
  - **Lazy Consensus**: Passes if there are no vetoes.
  - **Three-Vote Consensus**: Passes with no vetoes and at least three "Yes" votes, or unanimous consent if there are fewer than three eligible voters.
- **Optimization**: If all eligible voters cast a "Yes" vote before the voting period ends, the action is immediately approved.
- **Exception**: In special circumstances, a Project Lead may override an unresolved veto. This decision can be appealed to the governance committee.

### Maximum Representation Limit

To ensure balanced representation, no more than four active members may be from the same company. If more than four candidates from one company are elected, only the top four vote-getters will serve. The remaining spots will be filled by the next-highest vote-getters from other organizations.

### Vacancies

If an elected Maintainer steps down, the candidate with the next-highest number of votes from the previous election will be invited to fill the position. This process continues until the position is filled.

If this method fails, a special election will be held using the eligible voters from the most recent election. A Maintainer elected in a special election will serve the remainder of their predecessor's term.

## Community Bi-Weekly Meeting

To foster communication and collaboration within the Jeandle community, we hold a bi-weekly community meeting organized and hosted by the **Project Lead**. This meeting is an important platform for community members to share progress, resolve issues, and plan for the future. All contributors are welcome to participate.

### Meeting Schedule

- **Frequency**: Every two weeks. The specific time is determined by the Project Lead based on the availability of community members and announced in advance in the community group.
- **Format**: Online meeting (via video or DingTalk, etc.). It may be adjusted to asynchronous discussion under special circumstances.
- **Duration**: Typically 60 minutes.
- **Records**: Meeting minutes and summaries will be published to the project's public repository after the meeting to ensure that members who could not attend can stay informed.

### Meeting Agenda

1.  **Sync-up on Current Matters**: Members report on their current work progress and important issues.
2.  **Sync-up on Issues**: Discuss the status of currently open issues and problems.
3.  **Community Feedback and Discussion**: Collect and discuss community feedback.
4.  **Next-Phase Planning and Summary**: Formulate the work plan for the next phase and summarize the key points of the meeting.

### Participation and Procedures

- Any community member (including contributors, committers, maintainers, etc.) can freely participate in the bi-weekly meeting without any formal role requirement.
- If you are unable to attend a meeting, you can provide your feedback in advance through community channels. The Project Lead will discuss it on your behalf during the meeting.
- The meeting host rotates among the Project Leads. If a Project Lead is unable to host, they may temporarily delegate another Maintainer or Committer to host.

