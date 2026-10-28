# Testing strategy

Testing should prove the changed behavior, not maximize test count.

## When tests are expected

Usually add/update tests for:
- new behavior;
- bug fixes;
- validation rules;
- authorization logic;
- parsers/transformations;
- persistence behavior;
- critical edge cases;
- regressions with meaningful business impact.

Tests may be unnecessary for:
- pure formatting/styling;
- comments/docs;
- mechanical renames already enforced by compiler/typechecker;
- generated/disposable prototypes where user explicitly accepts that tradeoff.

## Minimal behavioral matrix

For ordinary behavior changes, consider:
- happy path;
- expected failure path;
- important boundary/edge case.

Add security-specific negative tests when relevant:
- unauthenticated request rejected;
- authenticated but unauthorized actor rejected;
- cross-tenant/object access rejected;
- malformed/tampered signature rejected;
- duplicate webhook/payment event does not duplicate effect.

Do not add all of these when the feature does not touch them.

## Bug-fix workflow

When practical:
1. reproduce the bug;
2. create/identify a test that demonstrates the failure;
3. confirm it fails for the expected reason;
4. apply the fix;
5. confirm the test passes;
6. run nearby regression tests.

If reproducing first is expensive or dangerous, state that limitation and use the best safe verification available.

## Test quality

A useful test:
- checks observable behavior or a real invariant;
- fails when the behavior is broken;
- is deterministic enough for CI;
- avoids implementation-detail coupling unless that detail is the contract;
- has clear setup and expectations.

Avoid tests that merely assert mocks were called without proving the behavior they represent, unless interaction itself is the required contract.

## Do not “greenwash” failures

Do not:
- delete a failing test because it blocks completion;
- weaken assertions to match incorrect output;
- add broad sleeps/timeouts instead of fixing races;
- skip flaky/security-critical tests without identifying the issue;
- regenerate snapshots blindly when they contain an unintended change.

If behavior intentionally changes, update the test and describe the changed contract when material.

## Verification order

Prefer fast feedback first:
1. targeted test(s);
2. typecheck/static analysis;
3. targeted lint;
4. package/build;
5. relevant integration/e2e;
6. full suite if risk or project conventions justify it.

Do not run a 30-minute full suite for a trivial isolated change unless required by the project workflow.

## Database/integration tests

For data-sensitive logic, test important invariants rather than only mocked repository calls.

Examples:
- unique/foreign-key constraints;
- transaction rollback behavior;
- idempotency keys;
- concurrent update protection when relevant;
- migration transform on representative fixtures.

## Reporting

Only report tests/checks that actually ran. Distinguish:
- passed;
- failed;
- not run;
- unavailable in the environment.
