# Deployment safety

Use when a change is going to a shared/staging/production environment or modifies deployed infrastructure.

## Separate readiness from deployment

“Implementation complete” does not mean “production changed.”

Do not deploy/publish/push to production unless:
- the user asked for it; or
- the active workflow explicitly authorizes deployment.

## Pre-deploy checks

Scale to risk, but consider:
- tests/checks appropriate to the change passed;
- build/package succeeds;
- migrations are compatible with deployment ordering;
- required environment variables/secrets exist without printing secret values;
- configuration points to the intended environment;
- dependency/lockfile changes are expected;
- rollback/recovery is understood for high-risk changes.

## Database + application compatibility

Prefer backwards-compatible sequencing:
- add schema first;
- deploy code supporting both states;
- migrate/backfill;
- switch behavior;
- remove old schema later.

Avoid a deployment where either old or new instances crash during rolling replacement because schema changed incompatibly.

## Staged rollout

For Level 3 changes, prefer an available mechanism such as:
- canary;
- feature flag;
- percentage rollout;
- one worker/instance first;
- shadow/dry-run mode;
- limited tenant/account rollout.

Do not add complex rollout infrastructure to a tiny app unless the risk warrants it.

## Infrastructure

Before destructive infra changes:
- inspect the plan/diff when IaC supports it;
- distinguish replacement from in-place modification;
- identify stateful resources;
- verify snapshots/backups when replacement can lose state;
- avoid broadening network/IAM access as a shortcut.

## Post-deploy smoke check

After a material deployment, verify the smallest set proving the service is healthy:
- health/readiness endpoint if meaningful;
- critical user/API path;
- error rate/logs around the changed component;
- migration completion/invariants;
- queue/worker health if touched.

For security-sensitive changes, verify both an allowed path and a denied path when feasible.

## Rollback trigger

For high-risk work, identify what would cause rollback, such as:
- elevated error rate;
- failed invariant;
- authorization regression;
- data mismatch;
- latency/resource spike;
- job backlog.

Do not keep a broken release live merely because rollback is inconvenient.
