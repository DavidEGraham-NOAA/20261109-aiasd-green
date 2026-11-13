---
ai_generated: true
model: "unknown/unknown@unknown"
operator: "jMM-at-CODETraining"
chat_id: "e9752f6f-5119-4a18-af86-2254b152ab42"
prompt: "create instruction files for the tech stack"
started: "2026-11-12T21:03:43-08:00"
ended: "2026-11-12T21:08:49-08:00"
task_durations:
  - task: "Creating stack instruction files and integration records"
    duration: "00:05:06"
total_duration: "00:05:06"
ai_log: "ai-logs/2026/11/12/e9752f6f-5119-4a18-af86-2254b152ab42/conversation.md"
source: "user request"
name: "postgresql"
description: "Use when designing or changing task-manager PostgreSQL schemas, queries, constraints, transactions, indexes, or database migrations."
applyTo: "**/*.sql"
version: "1.0.0"
author: "jMM-at-CODETraining"
tags: ["postgresql", "sql", "database", "task-manager"]
owner: "Development Team"
reviewedDate: "2026-11-12"
nextReview: "2027-02-12"
revisions:
  - chat_id: "task-manager-prd-20261113-061750"
    model: "unknown/unknown@unknown"
    date: "2026-11-13"
    change: "Clarified that task archive/restore is required and is distinct from individual permanent deletion."
    ai_log: "ai-logs/2026/11/13/task-manager-prd-20261113-061750/conversation.md"
---

# PostgreSQL Instructions

## Overview

Apply these rules to the task-manager relational schema and SQL. PostgreSQL is the selected authoritative database. The provider, version, data-access library, and migration tooling remain unselected; verify their supported behavior during implementation planning. Follow the [architecture specification](../../task-manager-architecture.md); do not infer schema approval or implementation authorization from this guide.

## Table of Contents

- [Workspace Isolation and Data Integrity](#workspace-isolation-and-data-integrity)
- [Types, Queries, and Transactions](#types-queries-and-transactions)
- [Deletion, Backups, and Migrations](#deletion-backups-and-migrations)
- [Verification Checklist](#verification-checklist)
- [Summary](#summary)

## Workspace Isolation and Data Integrity

- Include workspace ownership in task and named-list records. Scope every application query and mutation to the authenticated workspace; never rely on an opaque resource ID alone for authorization.
- Enforce that each task belongs to a list in the same workspace using appropriate relational constraints, such as a composite foreign key, in addition to application checks.
- Require a task list association and create the default `Tasks` list with its workspace. Make workspace and default-list creation atomic.
- Use database constraints for required values, valid relationships, and uniqueness where the approved product rules require them. Keep database and application validation consistent.
- Treat schema details as provisional until implementation specifications confirm column names and exact representations.

## Types, Queries, and Transactions

- Store due dates as date-only values (`DATE`); never encode them as midnight UTC. Store operational instants in UTC-aware timestamp values and store the configured IANA workspace time-zone identifier separately.
- Store task version state for conditional updates. Perform the version predicate and update atomically; a stale version must not silently overwrite a newer row or resurrect a task removed through approved account-level deletion.
- Use transactions for related changes that must succeed together, including workspace/default-list creation and task/list mutations with their version increments.
- Parameterize all values. Do not build SQL by concatenating user input. Implement title search as a literal, case-insensitive substring match; escape pattern metacharacters when using pattern operators so user input remains literal.
- Avoid premature indexes or query complexity. Add indexes based on approved access patterns and measured workload; document the query they support and verify query plans when data volume is representative.
- Use foreign keys and restrictive delete behavior that preserve required records. Preserve the PRD-defined task archive/restore lifecycle as restorable and distinct from permanent deletion; do not expose individual task deletion in v1. Do not add list deletion, whole-workspace deletion, a separate trash/deleted state, or automatic inactivity cleanup without separate product approval.

## Deletion, Backups, and Migrations

- Keep the deletion ledger independent of database restoration and limited to opaque workspace/task identifiers and deletion times; do not put task content or credentials in it. The ledger does not authorize an individual task-delete action; account-level deletion remains governed by its approved policy.
- Before a restored database is available, apply the latest ledger. If it is unavailable or cannot be applied, keep the restored service unavailable. Coordinate database and ledger consistency explicitly rather than assuming a single transaction covers independent stores.
- Protect backups and expire them within the approved 30-day window. Retain ledger entries only as long as needed to cover backups that may contain the deleted task.
- Use reviewed, repeatable migrations and preserve compatibility for deployment and rollback. Validate constraints and data transformations against representative data; do not treat an application rollback as a database rollback.
- Never use development or test data in pilot/production databases. Apply least-privilege roles and protect transport, storage, and backup keys.

## Verification Checklist

- [ ] Queries and mutations are parameterized and workspace-scoped.
- [ ] Constraints prevent cross-workspace task/list relationships.
- [ ] Date-only due dates and UTC instants use distinct representations.
- [ ] Version-checked mutations are atomic under concurrent requests.
- [ ] Task archive and restore remain reversible; no individual task-delete operation is exposed.
- [ ] Required multi-record changes use transactions.
- [ ] Literal title search handles wildcard characters as literal input.
- [ ] Restore applies the independent deletion ledger before serving data.
- [ ] Migrations and restore behavior are tested; no unspecified provider/tool is assumed.

## Summary

Use PostgreSQL to enforce relational integrity and atomic versioned changes while preserving strict workspace isolation, date-only semantics, PRD-defined task archive/restore behavior, and restore-safe account-level deletion handling.

**Related Instructions**: [React/TypeScript](react-typescript.instructions.md), [ASP.NET Core](aspnet-core.instructions.md)
**Document Version**: 1.0.1
**Last Updated**: 2026-11-13
**Maintainer**: Development Team
