# Risk model

Use this reference when task risk is not obvious.

## Two dimensions

Evaluate:

1. **System criticality** — what the software already controls or stores.
2. **Change risk** — what this specific change can break or expose.

Use the higher level as the effective risk.

## System criticality

### C0 — disposable/local

- toy project;
- throwaway prototype;
- local script with non-sensitive test data.

### C1 — normal application

- ordinary public website/app;
- non-sensitive internal tool;
- data is recoverable and consequences are limited.

### C2 — sensitive application

- accounts and authentication;
- personal/business data;
- public write APIs;
- integrations that can mutate external systems;
- privileged admin functionality.

### C3 — critical application

- money, balances, billing, financial records;
- production crypto/signing;
- health/safety or legally significant data;
- identity/security infrastructure;
- high-value production systems;
- changes with material business interruption or data-loss potential.

## Change risk

### R0 — cosmetic/local/reversible

- text, styles, spacing, simple rename;
- no persistence/security/behavior change.

### R1 — ordinary behavior

- CRUD or validation change;
- ordinary API/UI behavior;
- small internal refactor.

### R2 — sensitive boundary

- auth/session/permissions;
- new external input or file handling;
- webhook/integration;
- dependency with privileged access;
- schema migration preserving data;
- significant cache/job behavior.

### R3 — destructive/privileged/complex

- data deletion or lossy migration;
- production infra/security/IAM;
- accounting/value transfer;
- key/signature handling;
- concurrency/distributed consistency;
- public exposure of previously private service;
- incident response with active attacker possibility.

## Effective response

Use `max(C, R)` as a baseline, then lower or raise individual checks based on actual blast radius.

Do not mechanically require all Level 3 procedures because a project is generally critical. A CSS color change in a bank dashboard is still a trivial change if it cannot affect behavior or security.

Likewise, a one-line authorization change in a small app can be Level 2 or 3 because small diff size does not mean small risk.

## Escalation signals

Increase rigor when:
- requirements are ambiguous at a trust boundary;
- existing code has surprising security assumptions;
- there is no test coverage around a critical invariant;
- production state differs from repository state;
- rollback depends on data that will be destroyed;
- the task failed twice for unclear reasons;
- observed behavior contradicts the current mental model.

## De-escalation signals

Keep work lightweight when:
- change is cosmetic and isolated;
- generated output is disposable;
- no persisted state or trust boundary changes;
- existing tests/checks already cover the behavior;
- the user explicitly asks for a quick prototype and the result is clearly kept out of production.
