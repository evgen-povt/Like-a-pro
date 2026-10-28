# Model, reasoning, and context routing

The goal is not “always use the smartest model.” The goal is reliable work at proportionate cost/latency.

If the environment supports model selection, reasoning effort, subagents, or specialized workers, route accordingly. If it does not, use these rules to control analysis depth and context size.

## Tier A — fast/cheap

Good for:
- formatting;
- CSS/style tweaks;
- text changes;
- mechanical renames;
- repetitive boilerplate;
- obvious syntax/config edits;
- simple tests following an established pattern.

Do not invoke deep architectural/security analysis unless the small change crosses a sensitive boundary.

## Tier B — general coding

Good for:
- ordinary feature implementation;
- CRUD/API/UI behavior;
- local refactors;
- writing tests;
- routine debugging;
- normal library integrations.

This should be the default for most development work.

## Tier C — strong reasoning

Use for:
- architecture/system design;
- authentication/authorization/security logic;
- threat modeling;
- cryptography/key/signature handling;
- money/accounting/balance logic;
- complex migrations with existing production data;
- concurrency/distributed consistency;
- difficult performance analysis;
- ambiguous multi-system failures;
- incident response;
- debugging after one or two reasonable attempts did not explain the root cause.

Diff size is not the criterion. A one-line permission change may deserve Tier C; a 500-line generated UI may not.

## Dynamic escalation

Start at the lowest tier that should reliably solve the task.

Escalate when evidence appears:
- two attempted fixes fail or merely move the symptom;
- the model cannot explain the observed behavior;
- assumptions about state/control flow are contradictory;
- the change crosses a previously unnoticed trust boundary;
- data loss/security/value risk appears;
- performance symptoms require cross-layer reasoning.

Do not repeatedly burn a cheap model on a problem it demonstrably does not understand.

## Context budget

Prefer the smallest sufficient context:
- inspect the target file/component first;
- search for referenced symbols/callers/config;
- load adjacent modules only when needed;
- summarize stable background instead of repeatedly loading large files;
- avoid feeding the entire repository for a local edit.

Expand context when:
- ownership/control flow is unclear;
- behavior spans several layers;
- authorization/config is defined elsewhere;
- tests reveal hidden coupling;
- a migration/deploy requires cross-version compatibility.

## Specialized work

When specialized agents/tools exist, prefer them for bounded jobs:
- static/security scan;
- test execution;
- dependency audit;
- repository-wide search;
- performance profiling.

Do not use multiple expensive agents merely to obtain consensus on a trivial task.

## User-facing behavior

Normally do not discuss model routing with the user.

Mention it only when:
- the user asks about cost/model choice;
- a stronger model is required but unavailable;
- the current environment is materially mismatched to a critical task;
- switching models would materially reduce cost/latency for an ongoing repetitive workflow.
