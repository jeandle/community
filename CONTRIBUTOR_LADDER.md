# Jeandle Contributor Ladder

This document outlines the different contributor roles and their responsibilities within the Jeandle project. Consider it a guide to help you understand your role, expected responsibilities, and how to grow within the community. We have referenced the OpenJDK governance rules ([https://openjdk.org/bylaws](https://openjdk.org/bylaws)) in designing this document.

- [Goals](#goals)
- [Scope](#scope)
- [General Guidelines](#general-guidelines)
  - [Everyone is Welcome](#everyone-is-welcome)
  - [Mentorship and Guidance](#mentorship-and-guidance)
  - [Sustained Contribution](#sustained-contribution)
  - [Consensus for Merging](#consensus-for-merging)
  - [Initiating Discussions](#initiating-discussions)
- [Contributor Ladder](#contributor-ladder)
  - [Committer](#committer)
  - [Maintainer](#maintainer)
  - [Project Lead](#project-lead)
- [Additional Notes](#additional-notes)

## Goals

We have written this document with several key objectives in mind:

- To ensure the long-term health and sustainability of the Jeandle community.
- To encourage new contributors to become more deeply involved and take on formal roles.
- To provide a clear path for advancement for individuals and groups who wish to grow into maintainers of this project.

## Scope

This guide covers the roles and responsibilities for everyone who contributes to the Jeandle open-source project. Whether you are writing code, improving documentation, creating tests, or helping in other ways, this document applies to you.

Formal roles such as Committer, Maintainer, and Project Lead are defined in the [Jeandle Governance Document](./GOVERNANCE.md).

## General Guidelines

### Everyone is Welcome

We sincerely appreciate every contribution. You do not need a formal title to create or review pull requests (PRs), help resolve issues, or participate in discussions. Taking on a formal role is entirely voluntary, and you can contribute in any way that suits you.

### Mentorship and Guidance

As stated in our [Contributing Guide](CONTRIBUTING.md), we welcome all forms of contribution, whether it's PRs, issue feedback, or ideas for new features. If you are new, we recommend starting with small, meaningful changes. These changes are easier to review and are a great way to become familiar with our processes.

As we are an open-source project, review times may vary depending on the availability of Maintainers and Project Leads. You can build trust and accelerate your advancement by making consistent, small-scale contributions and providing constructive feedback on the work of others.

Every contribution we merge is one we commit to maintaining long-term. Therefore, larger changes not only require more effort but also a proven track record demonstrating your commitment to supporting them. Remember, ["No is temporary, yes is forever."](https://www.oreilly.com/library/view/hands-on-design-patterns/9781789135565/3f396314-dac8-446c-ab02-768ae91296fa.xhtml) 🙇‍♂️

### Sustained Contribution

If you are considering applying for one of the roles below, we hope it is because you plan to remain active in that role for the long term. If life circumstances prevent you from fulfilling your duties, it is perfectly acceptable to step back for a period. Members who are inactive for an extended time will be removed from the Jeandle GitHub organization, but you can always rejoin through the membership application process.

### Consensus for Merging

If you believe a change may be controversial, please hold off on merging to give others an opportunity to voice their opinions. Even if you have merge permissions, it does not mean you should always use them. While we want to avoid PRs being stuck indefinitely, it is equally important to ensure that everyone's perspective is heard.

### Initiating Discussions

If you are interested in advancing and taking on one of these roles, please contact a Project Maintainer on Slack or other community channels. We are happy to help you get started!

## Contributor Ladder

This document details the different contributor roles within our open-source project, including their responsibilities, requirements, and privileges. Contributors typically start at the first level and advance as their involvement and impact grow. We are here to support you at every step as you climb the contributor ladder!

Our ladder has three primary roles: **Committer**, **Maintainer**, and **Project Lead**. Each role comes with specific responsibilities, requirements, and privileges that reflect a contributor's level of involvement and trust within the community.

Here is an overview of the roles:

| Role             | Responsibilities                                                            | Requirements                                                                   | Privileges                                                                            |
| ---------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| **Committer**    | Push code changes directly, participate in project maintenance.             | A record of high-quality contributions and community trust.                    | Direct push access to the project repository, nominate new Committers.                |
| **Maintainer**   | Review code changes, ensure quality and compliance.                         | Extensive experience as a Committer, deep knowledge of specific project areas. | Approve code changes in designated repositories, nominate new Maintainers.            |
| **Project Lead** | Guide project direction, coordinate activities, manage technical decisions. | Long-term contributions, leadership, and a deep understanding of the project.  | Full technical authority, appoint and remove roles, represent the project externally. |

*Note: As the project evolves, these roles and responsibilities may change. This document will serve as the definitive guide.*

### Committer

A **Committer** is a contributor who has been granted direct push access to the project repository, allowing them to push code changes directly and take on greater responsibility within the community.

- **Responsibilities**:
  - Pushing code changes to the project repository and creating Pull Requests (PRs).
  - Ensuring that changes meet project standards and do not introduce issues.
  - Helping other contributors become Committers.
- **Requirements**:
  - A history of high-quality contributions and reviews.
  - A solid understanding of the project.
  - A commitment to the project's ongoing success.
  - Supporting new and occasional contributors by helping to improve their PRs for merging.
- **Privileges**:
  - Write access to the project repository.
  - May be listed as a Jeandle Committer for promotional purposes.
  - Can recommend and review other contributors for the Committer role.
- **How to Become a Committer**:
  1. Submit a PR to the team or membership management file to apply for the Committer role.
  2. The application is approved via "Lazy Consensus" from existing Committers (i.e., approved if no one objects).

### Maintainer

A **Maintainer** is an experienced Committer who has the authority to approve changesets for repositories that require formal change review.

- **Responsibilities**:
  - Regularly reviewing PRs in their designated area(s) of the project.
  - Ensuring that changes meet coding standards, are free of bugs, and are beneficial to the project.
  - Helping other Committers become Maintainers.
  - Assisting with issue triage and prioritization.
- **Requirements**:
  - Must already be a Committer.
  - A consistent record of high-quality reviews and contributions.
  - In-depth knowledge of a specific area of the project.
  - A commitment to the ongoing maintenance of their designated area(s).
  - Supporting new and occasional contributors by helping to improve their PRs for merging.
- **Privileges**:
  - GitHub permissions to approve PRs and manage labels in designated repositories.
  - May be listed as a Jeandle Maintainer for promotional purposes.
  - Can recommend and review other Committers for the Maintainer role.
- **How to Become a Maintainer**:
  1. Submit a PR to the team or membership management file to apply for the Maintainer role.
  2. The application is approved via "Three-Vote Consensus" from existing Maintainers (requires at least three "yes" votes or unanimous consent).

### Project Lead

A **Project Lead** is one of the most trusted members of the project, with the authority to merge code, vote on project matters, and help guide the project's direction. This role is for individuals with a deep commitment to the community.

- **Responsibilities**:
  - Holding full decision-making authority over the project's technical matters.
  - Serving as the primary point of contact for external communications.
  - Designating which code repositories require formal change review.
  - Coordinating releases and updating documentation.
  - Shaping the project's technical direction and roadmap.
  - Mentoring new Committers and Maintainers.
  - Participating in strategic and policy discussions for the project.
  - Voting on key project decisions in accordance with the governance rules.
  - Approving promotions to various roles within the project.
  - Publishing a written report each quarter summarizing recent project activities.
  - Acting as a project contact, maintaining project mailing lists, web content, and code repositories.
- **Requirements**:
  - A long history of high-quality contributions and leadership.
  - A deep understanding of the project's architecture and design.
  - A track record of supporting new contributors and helping to improve their contributions.
  - Demonstrated ability to exercise independent judgment and prioritize the project's interests over personal or employer interests.
  - A history of actively mentoring others in the community.
- **Privileges**:
  - Can merge code into project repositories.
  - Can designate which code repositories require formal change review.
  - Can vote on project matters.
  - May be listed as a Jeandle Project Lead for promotional purposes.
  - Can vote on project-level decisions.
  - Can nominate new Project Leads or Maintainers.
- **How to Become a Project Lead**:
  1. Nominated via a PR by an existing Project Lead or the governance committee.
  2. Approved by a "Two-Thirds Majority" vote of active Committers, in accordance with the governance rules.

Project Leads are automatically granted Maintainer status and retain it even if they step down from the Project Lead role.

## Additional Notes

- If there are objections to role promotions, responsibility assignments, or voting outcomes, a case may be presented to the governance committee.
- Role permissions and responsibilities may be adjusted as the project evolves. Such changes require approval from the governance committee and confirmation through a vote.
- All community members must adhere to the project's Code of Conduct. Any violation may result in the revocation of role permissions.