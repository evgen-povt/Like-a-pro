# Project bootstrap and maturity

Use when creating a new project or when an existing vibe-coded project lacks basic engineering structure.

Do not add enterprise ceremony to a tiny prototype. Add guardrails as the project becomes less disposable.

## Stage 0 — local prototype

Minimum useful baseline:
- simple, reproducible run command;
- dependency lockfile when the ecosystem uses one;
- real secrets kept outside source;
- stack-appropriate `.gitignore` excluding secrets, installed dependencies, build/download outputs, caches, and local runtime files while retaining required source inputs and lockfiles;
- local Git if the project is more than disposable scratch work.

Avoid spending time on CI, elaborate observability, or deployment architecture before there is a need.

## Stage 1 — shared/public application

Add as appropriate:
- `.gitignore` and clean secret handling;
- formatter/linter/typechecker conventional for the stack;
- a test framework and tests around real behavior;
- build/check command that can be run before release;
- minimal README/run instructions;
- server-side input validation for public inputs;
- basic production error handling/logging.

If the repository has a remote/shared workflow, a small CI job that runs the normal tests/typecheck/build is usually worthwhile.

## Stage 2 — users or important data

Add as appropriate:
- explicit authentication/authorization model;
- negative authorization tests;
- dependency/security update hygiene;
- backup/recovery for persistent data;
- production configuration/secrets management;
- migration discipline;
- operational health checks/logging;
- rate/resource limits on abuse-prone endpoints.

## Stage 3 — critical/value-bearing system

Add based on actual threat/business impact:
- stronger review for security/value-changing logic;
- tested restore/recovery procedures;
- staged/reversible deployments;
- audit/security event visibility;
- explicit invariants/idempotency/transaction design;
- incident/key-rotation procedures;
- tighter production access and least privilege.

## Standard project files

For a normal project, consider:
- local Git repository;
- `.gitignore` appropriate to the stack;
- dependency lockfile;
- clear run/build/test commands;
- environment-variable handling;
- `.env.example` with placeholders when configuration is non-obvious;
- formatter/linter/typechecker already conventional for the language/framework;
- at least one test path/framework for behavioral code;
- minimal README/run instructions for a project another person may need to start.

## Secrets

From the start:
- keep real secrets outside source control;
- do not embed secrets in frontend/client bundles;
- distinguish public configuration from credentials;
- provide placeholders, not example live keys.

## Avoid unnecessary scaffolding

Do not automatically add:
- Kubernetes;
- microservices;
- message queues;
- complex CI matrices;
- an ORM when simple DB access already fits;
- observability vendors;
- elaborate design systems;
- dozens of dev dependencies.

Add complexity when a concrete requirement justifies it.

Do not call a prototype “production-ready” merely because it runs locally.
