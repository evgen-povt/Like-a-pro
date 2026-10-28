# Testing strategy

Testing should prove the changed behavior, not maximize test count.

## Safe execution targets

Before tests, seed/reset scripts, or application startup, inspect the command, lifecycle hooks, migrations, and enabled background jobs for side effects.

For commands that can reach persistent state or external services:
- resolve the actual database/storage and external-service account/environment without printing credential values;
- use an isolated test database/data set and provider sandbox/test credentials; disable live payments, outgoing messages, and unrelated jobs;
- verify the target rather than trusting a filename, localhost, or an environment variable named `test`;
- keep destructive fixtures/reset scripts away from shared or production data.

If isolation is unknown, prepare a safe local/test setup and run checks that cannot touch live state. Report unavailable integration checks. Do not run against production merely because it is the only configured environment; a deliberate live verification needs specific authorization, bounded effects, and applicable recovery checks from `deployment.md` and `data-and-backups.md`.

## Acceptance criteria first

For new or changed behavior, derive short acceptance criteria from user actions and observable outcomes before choosing tests. Reuse the user's explicit requirements and fill obvious details with safe conventional assumptions; ask only about ambiguities that materially change the product.

For “save a request,” useful criteria might be: a valid request is saved, a fresh read/reload returns it, unauthorized users cannot access it when access is restricted, and a save failure is visible. Tests should establish the relevant outcomes, not simply agree with the code that was generated.

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

## Connected application verification

When behavior crosses UI/API/storage or an external integration boundary, verify at least one target acceptance scenario through the real connected application in a safe environment. Start it when needed and verify actual wiring, rather than relying only on mocks or a successful build.

For persistence, perform a fresh read/reload; include restart verification when durability across restarts is part of the contract. For UI behavior, interact with the relevant control; manual checks or focused existing tooling can be sufficient without creating a new browser-automation suite. Use provider sandboxes for external effects.

If the environment is unavailable, state separately what was implemented, what passed isolated checks, and which acceptance criteria remain unverified in the application. Do not present a stubbed API or mocked provider as a completed real integration unless that prototype scope was explicitly requested.

## Database/integration tests

For data-sensitive logic, test important invariants rather than only mocked repository calls.

Examples:
- unique/foreign-key constraints;
- transaction rollback behavior;
- idempotency keys;
- concurrent update protection when relevant;
- migration transform on representative fixtures.

## Basic UI usability checks

For new UI flows or changes to layout/interaction, exercise the affected user flow in a safe environment and check:
- a narrow/mobile and a normal viewport for usable layout, readable content, and reachable controls;
- keyboard access, logical focus order, visible focus, and usable dialog focus/escape behavior where relevant;
- semantic controls and accessible names, with field labels and errors associated with the right input;
- relevant loading, error, empty, and success states, including preserved input on failure and protection from duplicate submission;
- feedback that communicates state without relying on color alone.

Reuse native controls, established components, and available tooling. Focused manual checks can be sufficient; report what was actually checked. Do not create a new design system, require every possible state for an unrelated cosmetic edit, or claim accessibility certification from a brief smoke check. Use synthetic data in screenshots and previews.

## Reporting

Only report tests/checks that actually ran. Distinguish:
- passed;
- failed;
- not run;
- unavailable in the environment.

Distinguish implementation status, isolated test results, and verification through the connected application. A limitation affecting an acceptance criterion belongs in the completion report, even when the build and unit tests pass.
