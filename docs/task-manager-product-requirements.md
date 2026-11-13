---
ai_generated: true
model: "unknown/unknown@unknown"
operator: "@DavidEGraham-NOAA"
chat_id: "task-manager-prd-20261113-061750"
prompt: |
  create a product requirements specification for a task manager
started: "2026-11-13T06:17:00.720-10:00"
ended: "2026-11-13T10:10:14.849-10:00"
task_durations:
  - task: "Drafting and checking the task manager product requirements"
    duration: "00:02:29"
  - task: "Capturing user decisions and revising the product requirements"
    duration: "00:02:00"
  - task: "Reconciling business-rule analysis with the product requirements"
    duration: "00:01:00"
  - task: "Recording the decision to reopen completed tasks"
    duration: "00:00:11"
  - task: "Recording confirmed architecture constraints and persistence"
    duration: "00:10:29"
total_duration: "00:16:09"
ai_log: "ai-logs/2026/11/13/task-manager-prd-20261113-061750/conversation.md"
source: "user request"
---

# Task Manager Product Requirements

## Document control

- **Status:** Draft - for product-owner and stakeholder review; not approved for implementation.
- **Version:** 0.5
- **Product owner:** Product Owner role; name TBD.
- **Approver:** Product sponsor or designated stakeholder; name TBD.
- **Evidence base:** The initial request named a task manager but supplied no user research or existing product rules. Subsequent user answers established candidate v1 direction and provisional targets. No research validation, applicable organizational policies, or formal approval have been supplied.
- **Interpretation:** User-provided direction is distinguished from acceptance details inferred for review. This remains a draft, not an approved policy or implementation authorization.

## Summary

The proposed product helps an individual capture, organize, and track personal tasks through completion. The user-provided v1 direction is an authenticated, private account with a responsive, online-only web experience. Tasks have a required title and optional description, project name, and date-only due date; users can complete tasks, reopen completed tasks, archive tasks, and restore archived tasks. The product remains a draft pending named ownership, applicable policy checks, and review of provisional targets.

## Problem and evidence

The product category suggests a need to record work and know whether it remains active or is complete. This is a problem hypothesis, not a validated user need: the request does not identify a user group, current workflow, unmet need, or business outcome.

Before committing scope, validate who will use the product, what they need to manage, and how they currently track work.

## Goals and success indicators

### Proposed outcomes

1. A user can record a task and find it again.
2. A user can update a task as work changes and mark it complete.
3. A user can distinguish active tasks from completed tasks.

### Candidate indicators

The user selected a provisional usability target; its test protocol still needs definition. Adoption and retention targets remain open until a pilot establishes a baseline.

- **Usability:** At least 90% of participants can create, edit, and complete a task without help. Define participant profile, sample size, task script, and test environment before treating this as a release criterion.
- **Task capture:** Track the share of task-entry attempts that produce a saved task.
- **Task completion:** Track the share of created tasks users later mark complete, interpreted alongside task age and user intent.
- **Adoption and retention:** Set targets after a pilot establishes a baseline.

## Users and scenarios

The primary user for v1 is an individual managing a personal task list. Each task belongs to the authenticated account owner; shared workspaces and team roles are out of scope for v1.

1. **Capture work:** A user records a task with a title and optional details.
2. **Review work:** A user views active tasks and identifies tasks that are complete.
3. **Maintain work:** A user corrects task details or changes its completion state.
4. **Track a date:** A user adds, changes, or clears a task's optional date-only due date.

## Business-rule provenance

The following are candidate rules for stakeholder review, not approved policy:

- **User-provided v1 direction:** Tasks are private to an authenticated individual account; sharing is out of scope. A task has a required title and may have a description, free-text project name, and date-only due date. Users may complete tasks, reopen completed tasks, archive tasks, and restore archived tasks; individual task permanent deletion, reminders, and offline changes are out of scope.
- **Confirmed follow-up direction:** Saved tasks and edits persist across closing/reopening the service and later sign-in. Accounts use application-managed passwords, self-registration, and password recovery.
- **Inferred acceptance details:** FR-01 through FR-08 specify validation feedback, initial task state, list and empty-state behavior, edit failure behavior, and archive visibility/restoration state. These details were not all explicitly selected by the user and require review.
- **Provisional targets:** The usability and performance thresholds below were selected by the user as initial targets; their test conditions are not yet agreed.

## Proposed functional requirements

These requirements reflect the user's v1 direction, with acceptance details proposed for review. The document remains a draft and is not approved for implementation.

### Task capture and maintenance

| ID | Proposed requirement | Acceptance criteria |
|---|---|---|
| FR-01 | A user can create a task with a non-empty title. | Proposed detail: Given a title containing non-whitespace characters, when the user saves a new task, then it appears in the task list with an active status. Given a missing or whitespace-only title, when the user saves, then the task is not created and the user is told what needs correction. Confirm validation behavior and initial status. |
| FR-02 | A user can view the tasks owned by their authenticated account. | Given saved tasks owned by the account, when the user opens the list, then each task is represented once with its title and completion state. Tasks owned by another account are not shown. When there are no tasks, the list presents an empty state rather than a task row. |
| FR-03 | A user can edit a saved task's title and optional description, project name, and due date. | Given an existing task, when the user saves valid edits, then the updated values are shown and remain after closing/reopening the service and later sign-in. Invalid titles are rejected without replacing the last saved values; confirm this error behavior. |
| FR-04 | A user can mark an active task complete and reopen a completed task. | Given an active task, when the user marks it complete, then its state becomes completed. Given a completed task, when the user reopens it, then its state returns to active. |
| FR-05 | A user can view active tasks separately from completed tasks. | Given tasks in both states, when the user selects either state, then only tasks in the selected state are shown. When no tasks match, the view shows an empty state. |
| FR-06 | A user can archive and restore a task without permanently deleting it. | User direction: individual task permanent deletion is not available in v1. Proposed detail: archiving removes the task from normal views; restoring it returns it to its prior completion state. Confirm archive visibility and restore behavior. |

### Optional task fields

| ID | Proposed requirement | Acceptance criteria |
|---|---|---|
| FR-07 | A user can set, change, or clear an optional date-only due date on a task. | Given a task, when the user saves a due date, then the selected calendar date is shown using the user's locale. When the date is changed or cleared and saved, the updated value or absence of a due date remains after closing/reopening the service and later sign-in. Due dates have no time-of-day or timezone behavior, and no reminder is sent. |
| FR-08 | A user can set, change, or clear an optional free-text project name on a task. | Given a task, when the user enters a project name and saves, then that value is associated with and shown on the task and remains after closing/reopening the service and later sign-in. The field accepts a name without requiring a matching project record or project list. |

### Account access

| ID | Proposed requirement | Acceptance criteria |
|---|---|---|
| FR-09 | A user can self-register an application-managed account, sign in with a password, and recover access through password reset. | Registration and sign-in follow the approved account policy. Given an existing account, its owner can complete the approved recovery process and sign in with the new password. [NEEDS DECISION]: identifier/verification rules, password/session policy, recovery proof/delivery, expiry, and failure behavior. These account capabilities do not introduce task reminders or notifications. |

## Quality and operational considerations

The following quality and operational directions reflect the user's answers and remain provisional. Confirm applicable policies and validation conditions before treating them as release commitments.

- **Accessibility:** Target WCAG 2.2 AA. All task actions should be keyboard-operable, controls should have accessible names, and completion state should not be communicated by color alone. Confirm applicability with the responsible policy owner.
- **Data protection:** Only the authenticated account owner can access task content. There is no task sharing in v1.
- **Persistence and connectivity:** User-confirmed: saved tasks and edits remain available to the account owner after closing/reopening the service and later sign-in. The product is online-only; offline changes are not supported in v1. Disconnected-user feedback is unspecified.
- **Performance and scale:** List and save actions should complete in under 2 seconds at the 95th percentile in a test scenario with 500 tasks per user. Availability, recovery, and larger-scale targets remain unset.
- **Privacy and retention:** User direction: support account-level data export and deletion, and do not send task text to analytics. Individual task removal is reversible archive only. Account deletion is a separate operation; its scope, data-purge timing, and backup handling must follow confirmed organizational policy.
- **Localization:** The initial UI is English. Format date-only due dates according to the user's locale.

## Scope boundaries

### Candidate initial scope

- Create and edit individual tasks.
- Associate an optional project name as free text, without a project-list or project-record relationship.
- View tasks and their active or completed state.
- Mark tasks complete and reopen them.
- Set, change, and clear an optional date-only due date.
- Archive and restore tasks; do not permanently delete individual tasks.
- Export and delete account data, subject to the confirmed account-deletion policy.
- Provide an authenticated personal account; do not share tasks with other users.
- Support application-managed account self-registration and password recovery.
- Support desktop and mobile browsers through a responsive web experience; require connectivity.

### Not included in this draft baseline

The following are excluded from v1: team collaboration and assignment, project lists or linked project records, labels, priorities, subtasks, recurring tasks, reminders and notifications, third-party integrations, file attachments, offline use, reporting, and permanent deletion of an individual task.

## Dependencies and constraints

- **Known dependencies:** Authenticated account ownership; an account-level export/deletion policy; locale-aware date display; accessibility review against the selected target.
- **Known constraints:** User direction limits v1 to private individual accounts, responsive web, online-only use, and no team sharing or permanent task deletion. Confirmed architecture constraints are ASP.NET Core, React/TypeScript, SQL Server, self-hosted Windows Server/IIS, and application-managed passwords with self-registration and recovery; see the [proposed architecture](task-manager-architecture.md). Architecture approval remains separate.
- **Assumptions to validate:** Performance and usability targets are provisional; applicability of the accessibility target and account-deletion policy is unconfirmed.
- **Implementation boundary:** This is a product requirements draft, not an architecture or technology recommendation.

## Risks

- Adding shared workspaces later could change access, ownership, and collaboration requirements.
- Reversible archive-only behavior may not satisfy users who expect permanent task deletion.
- Account deletion without an agreed data-purge time and evidence could create unmet privacy expectations.
- The 2-second performance target at 500 tasks per user may not reflect real usage or infrastructure conditions.
- A 90% usability target is not meaningful without a defined participant profile, sample size, task script, and test environment.

## Open questions

Resolve these items before treating the related candidate rules as approved. **[NEEDS DECISION]** marks an unresolved product or policy decision.

1. **[NEEDS DECISION]** What availability and recovery targets are needed, and what user/task volume beyond the provisional 500 tasks per user should be supported?
2. **[NEEDS DECISION]** What scope, purge timing, backup handling, and evidence apply to account-level export and deletion?
3. **[NEEDS DECISION]** How long are archived tasks retained, and can any automatic purge occur?
4. **[NEEDS DECISION]** What should a user see or be able to do when disconnected, given that offline changes are unsupported?
5. **[NEEDS DECISION]** What title and project-name length/character limits, validation feedback, ordering/search behavior, and save-error handling are required?
6. **[NEEDS DECISION]** Does WCAG 2.2 AA satisfy the applicable accessibility policy, and is English sufficient for the initial release?
7. **[NEEDS DECISION]** What participant profile, sample size, task script, and test environment will verify the 90% usability target?
8. **[NEEDS DECISION]** What test environment and measurement boundary will verify the p95 performance target?
9. **[NEEDS DECISION]** Who will be named as Product Owner and approving sponsor or stakeholder?
10. **[NEEDS DECISION]** What registration/identifier/verification, password/session, and password-recovery delivery/proof policies apply?

## Decision history

| Date | Decision or change | Status |
|---|---|---|
| 2026-11-13 | Created an initial product-requirements draft from the task-manager request; marked inferred capabilities and scope as proposals. | Draft; stakeholder decisions remain open. |
| 2026-11-13 | Recorded user direction for individual authenticated private accounts, task fields, lifecycle, platform, accessibility, privacy, and provisional performance and usability targets. Clarified project name as free text, not a linked project list; individual tasks can be archived/restored but not permanently deleted. | Direction captured; document remains a draft pending policy checks and named approval. |
| 2026-11-13 | Reconciled the evidence statement with subsequent user answers; separated user-provided direction from inferred acceptance details and marked unresolved behavior and policy decisions. | Draft updated for stakeholder review; no policy or approval was inferred. |
| 2026-11-13 | User confirmed that completed tasks can be reopened. Updated FR-04 and its provenance to record reopening as confirmed direction rather than an unresolved proposal. | FR-04 reopening decision resolved; overall document remains Draft. |
| 2026-11-13 | User confirmed durable saved data, ASP.NET Core/React/TypeScript/SQL Server on Windows Server/IIS, local password accounts, self-registration, and password recovery. Added FR-09 and removed persistence decision markers. | Direction recorded; detailed account policy and proposed architecture remain subject to review. |

