# Data safety, backups, and rollback

Use for any operation that can mutate or destroy persistent state outside ordinary source-code edits.

## First identify the state at risk

Examples:
- SQL/NoSQL database;
- object/file storage;
- cloud resources;
- infrastructure state;
- secrets/keys;
- external SaaS records;
- queues/event streams;
- production configuration;
- user-generated content.

Do not say “backup exists” unless there is evidence for it.

## When a backup/recovery check is required

Always consider it for:
- destructive or lossy schema migrations;
- bulk UPDATE/DELETE;
- data transformation without easy inverse;
- storage bucket moves/deletes;
- replacing or resizing stateful infrastructure;
- credential/key rotation where loss of old material can strand data;
- production cleanup scripts;
- importing data that can overwrite canonical records.

Do not nag about backups for ordinary source edits or additive, easily reversible local development work.

## Minimum recovery questions

Before a high-risk action, determine:
- What exact state can be lost or corrupted?
- What is the latest recovery point?
- Is the recovery point recent enough for the consequence?
- Is restoration documented or otherwise understood?
- How long would restoration plausibly take?
- Does rollback require data that the migration will delete?
- Can the change be staged or made additive first?

## Database migrations

Prefer expand/migrate/contract for production changes:
1. add backwards-compatible schema;
2. deploy code that can work with old/new state;
3. backfill/migrate data with checks;
4. switch reads/writes;
5. verify;
6. remove old schema only after confidence and recovery window.

For destructive migrations:
- verify backup/snapshot where appropriate;
- estimate affected rows/data volume when feasible;
- test migration on representative data when consequences are material;
- avoid assuming transactionality: many DDL operations and external side effects do not roll back uniformly across engines;
- validate row counts/invariants before and after.

## Bulk data mutations

Prefer:
- a dry-run/count query first;
- explicit selection criteria;
- bounded batches for large datasets;
- idempotent/restartable scripts;
- audit output that does not leak sensitive data;
- post-change invariant checks.

For a production `DELETE` or destructive `UPDATE`, a correct WHERE clause is not a recovery plan.

## Backups vs restore confidence

Confidence ladder:
1. “backups are configured” — weak evidence;
2. recent backup/snapshot is visible — better;
3. restore procedure is documented — better;
4. restore has been tested recently — strong.

Scale the expectation to business impact. Do not demand disaster-recovery drills for a hobby app.

## Rollback

A rollback plan should identify:
- code rollback;
- schema compatibility after rollback;
- whether data written by the new version remains readable by the old one;
- how external side effects are handled;
- what observation triggers rollback.

Do not promise rollback when the operation is fundamentally irreversible. In that case, prioritize backup and staged execution.
