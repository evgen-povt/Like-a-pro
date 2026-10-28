# Calibration examples

These examples define the intended level of intervention.

## Example: “Make the button blue”

Risk: Level 0.

Expected behavior:
- inspect the relevant component/style;
- make the smallest change;
- optionally run a quick existing formatter/typecheck only if cheap/conventional;
- no threat model;
- no new tests solely for the color;
- no backup lecture;
- use fast/cheap routing if available.

Bad behavior:
- reading the whole repository;
- proposing a design-system migration;
- invoking a top reasoning model just for CSS.

## Example: “Add delete account endpoint”

Risk: Level 2, possibly 3 if production/user data is valuable.

Expected behavior:
- verify authentication and authorization;
- decide what “delete” means for related records/files;
- consider re-authentication or confirmation if appropriate to the product;
- test user cannot delete another user's account;
- consider irreversible-data/recovery implications;
- avoid logging sensitive deleted data.

## Example: “Fix CORS so the frontend works”

Risk: Level 2.

Expected behavior:
- identify intended frontend origin(s), credential mode, and request types;
- configure the narrow required origins/headers/methods;
- do not default to wildcard credentials or disabling browser/security controls;
- verify preflight/actual request behavior.

## Example: “Add payment-provider webhook”

Risk: Level 3 when it changes balances/orders.

Expected behavior:
- verify webhook signature;
- use replay/timestamp protection if provider supports it;
- process idempotently;
- map provider event to internal object safely;
- make value-changing DB work transactional where possible;
- test duplicate event handling;
- log event ID/outcome without leaking secrets.

## Example: “Rename `displayName` to `name` everywhere”

Risk: Level 1 unless persisted/public API compatibility is involved.

Expected behavior:
- search references;
- determine whether DB/API/client compatibility matters;
- use compiler/tests to catch misses;
- do not create architecture docs.

If it changes a persisted production column/public API, increase risk and plan compatibility/migration.

## Example: “Drop this old database column”

Risk: Level 3 in production.

Expected behavior:
- verify no old code/clients still read/write it;
- check whether removal is actually needed now;
- verify backup/recovery;
- prefer contract step after code has stopped depending on it;
- consider staged deploy;
- test migration/rollback compatibility where practical.

## Example: “The API request hangs sometimes”

Risk: Level 1–2 depending on impact.

Expected behavior:
- reproduce/inspect timing;
- check missing timeout, connection pool exhaustion, lock, retry loop, external dependency;
- do not add arbitrary long sleeps;
- add finite timeouts and cancellation where appropriate;
- escalate reasoning/context if two reasonable attempts fail to explain it.

## Example: “Install a package to generate a random UUID”

Expected behavior:
- first check whether the runtime already has a safe standard UUID facility;
- avoid adding a dependency for trivial built-in functionality.

## Example: “Run this migration in production”

Risk: Level 3.

Expected behavior:
- inspect migration semantics;
- determine lock/runtime/data-loss risk;
- verify recovery/backup if destructive;
- check app-version compatibility;
- run only when production action is explicitly authorized;
- verify post-migration invariants.
