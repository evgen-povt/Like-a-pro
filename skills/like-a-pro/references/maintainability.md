# Maintainability and code quality

The goal is not “clean code” as ceremony. The goal is to stop AI-generated projects from accumulating contradictory patterns, duplication, accidental complexity, and code nobody can safely change.

## Follow the existing shape first

Before introducing a new pattern:
- search for how similar behavior is already implemented;
- reuse existing helpers/components/services when they fit;
- preserve naming and module conventions;
- avoid creating a second way to do the same thing without a reason.

If the existing pattern is unsafe or clearly causing the problem, fix or replace it deliberately rather than copying it.

## Keep changes local

Prefer the smallest coherent change.

Do not combine a feature/bug fix with unrelated:
- formatting churn;
- dependency upgrades;
- directory reorganizations;
- broad renames;
- architectural rewrites.

This reduces regression risk and makes Git rollback/review useful.

## Duplication vs abstraction

Avoid meaningful duplicated business/security logic, especially:
- authorization rules;
- money/fee calculations;
- validation schemas;
- serialization/parsing;
- protocol/API constants.

But do not create an abstraction just because two short blocks look similar. Abstract when the code represents the same concept and is likely to change together.

## Module/function size

Do not enforce arbitrary line limits.

Refactor when a unit has multiple responsibilities, hidden side effects, or is difficult to test/reason about. Prefer clear boundaries over splitting code into tiny wrappers that add indirection without meaning.

## Types and schemas

When the language/framework supports them:
- keep types aligned with runtime validation at trust boundaries;
- avoid using `any`/unchecked casts merely to silence the compiler;
- model impossible/invalid states out when it materially simplifies correctness;
- update API/schema types together with implementation.

A compiler passing is not proof that untrusted runtime input is valid.

## Comments and documentation

Comments should explain *why*, invariants, compatibility constraints, or non-obvious hazards—not restate obvious code.

Update documentation when the change modifies:
- public API/CLI behavior;
- setup/run/deploy instructions;
- required environment variables;
- operational recovery steps;
- important architecture assumptions.

Do not produce large documentation files for trivial internal edits.

## Dead code and TODOs

Remove dead code created by the current change when confidence is high.

Do not delete unfamiliar “unused-looking” code across a dynamic/reflection/plugin boundary without checking how it is invoked.

Do not leave vague TODOs for correctness/security work required for the feature to be safe. Either complete it, make the limitation explicit, or block the unsafe path.

## Errors and control flow

Prefer explicit failure handling over:
- empty catches;
- returning success after failure;
- broad exception suppression;
- magic fallback values that hide corruption/misconfiguration.

Keep exceptional paths understandable and test important ones.

## Configuration

Avoid duplicated configuration across source files when one authoritative source is practical.

Validate critical configuration at startup so failures occur clearly rather than much later on a request path.

## “AI smell” review

Before finishing a medium/large generated change, check for common AI artifacts:
- duplicate helpers with slightly different names;
- unused imports/functions;
- placeholders or fake values left in production paths;
- comments describing code that was later changed;
- broad catch/fallback logic hiding errors;
- hardcoded environment assumptions;
- extra dependencies for trivial work;
- tests that mirror implementation rather than behavior;
- code paths that were invented but never wired into the application.
