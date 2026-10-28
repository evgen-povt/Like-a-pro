# Project bootstrap and maturity

Use when creating a new project, changing substantial setup, handing a project over, or when an existing vibe-coded project lacks basic engineering structure.

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
- owner-controlled or owner-accessible repository and service accounts, with known access/recovery arrangements.

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

## Ownership and access

For a shared or deployed project, identify who controls the repository, domain/DNS, hosting, database/storage, and material third-party services. Prefer accounts controlled by the project owner or their organization and role-based collaborator access with appropriate privileges. Verify the owner has the access needed to operate/recover the project; successful deployment through an agent or contractor session is not evidence of owner access.

Record non-secret account/project identifiers, service locations, access roles, and the owner-controlled recovery process where useful. Keep passwords, tokens, private keys, and recovery codes in appropriate secret storage, not documentation or chat. Do not request credentials pasted into chat.

For existing accounts outside the owner's control, identify the gap and prepare the smallest transfer/access plan. Do not transfer ownership, change billing, or revoke existing access without the appropriate authorization. Continue safe implementation/documentation while an access dependency remains unresolved, and disclose it in the handover.

## Continuity and handover

For substantial delivery, update the existing README/setup/operations notes so another person or fresh chat can continue from the repository. Keep one maintained source of setup information; a separate handoff file is only useful when the existing documents cannot serve that purpose.

Record the relevant subset of:
- required runtime/package-manager versions and install/run/build/test commands;
- environment-variable names and how to obtain/configure values securely, using placeholders only;
- connected services, deployment target, account ownership, and access verified versus still needed;
- migration/deploy ordering and backup/recovery pointers when applicable;
- important non-obvious decisions/constraints, acceptance scenarios actually verified, and remaining limitations or next steps.

For a new/shared project's initial handover or material setup change, check reproducibility from a clean checkout in a safe environment when feasible. Do not copy a live secret file, trigger production jobs, or create paid resources just to make that check pass. If verification is unavailable, say which setup/access steps remain unverified. Code/configuration are evidence when old notes contradict them; reconcile the notes rather than carrying contradictory instructions forward.

Do not add handover paperwork for a cosmetic edit or mark operational handover complete while essential owner access or setup remains unknown.

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
