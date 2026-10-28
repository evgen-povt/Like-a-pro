# Operating costs and spending limits

Use before introducing or materially expanding paid hosting, metered APIs, storage, background jobs, subscriptions, or publicly reachable expensive operations. Model/reasoning selection is covered separately in `model-routing.md`.

## Understand the cost before committing

Identify:
- the account/service and billing unit, such as instance-hour, request, image, message, storage, or data transfer;
- expected usage, peak usage, and a rough cost range with explicit assumptions;
- recurring charges, minimum commitments, and relevant overage behavior;
- existing user authorization and spending limits.

Check current provider pricing/limits when relying on them; do not invent prices or describe an unknown allowance as free. For material uncertainty, explain what is unknown and choose a bounded trial or sandbox while resolving it.

## Match hosting resources to the workload

Choose the simplest hosting model that meets the application's actual runtime, persistence, availability, and expected traffic needs:
- assess the project's hosting as a whole and reuse suitable existing infrastructure when it meets those needs;
- compare viable setups by total cost and the effort to configure, deploy, monitor, and maintain them; prefer fewer independently managed services when the requirements allow;
- add separate hosting services, delivery layers, or deployment pipelines only when a concrete operational, performance, security, or availability benefit justifies the added complexity;
- add server-side compute, databases, or workers only when the workload requires them;
- size CPU, RAM, storage, and instance count from runtime requirements and expected usage, with modest justified headroom;
- distinguish temporary build-time requirements from resources needed to serve the application;
- start with the smallest configuration that reliably meets those needs, verify it under representative use, and scale based on measured memory, CPU, latency, and traffic;
- bound autoscaling where available and account for storage, backups, and network charges as well as compute.

Justify substantial spare capacity with a concrete requirement or measurement rather than hypothetical future growth. Avoid undersizing that causes crashes or unacceptable performance just to minimize the initial price.

Apply sizing decisions to resources being selected for the task; do not resize existing production resources without authorization and the relevant deployment/recovery checks.

## Stay within authorized spending

Reuse spending authorization already established in the conversation or workflow. Ordinary work within agreed limits does not require repeated approval.

Before starting a material new paid commitment, enabling an unbounded paid workload, or exceeding an agreed limit, obtain the missing budget/scope decision. Prepare the implementation, cost estimate, and limit settings first so the decision is concrete. Permission to deploy alone does not establish an unlimited operating budget.

If budget authorization is unresolved, continue with sandbox credentials, mocked/disabled paid calls, or an undeployed configuration. Keep the live expensive path disabled and disclose the remaining activation step; do not claim the real integration was verified through a mock.

## Bound expensive work

For publicly reachable or automated metered work, apply controls appropriate to the billing path:
- server-enforced request/work-size limits and per-user/account quotas where relevant;
- bounded concurrency, queue depth, timeouts, and retry attempts;
- idempotency or duplicate suppression when retries can create duplicate charges;
- an enforceable budget/quota or bounded usage window, plus a way to stop new expensive work;
- accounting for in-flight work and retry fan-out when estimating the possible overshoot.

A per-user limit alone may be insufficient if new accounts or anonymous requests can multiply costs; consider an aggregate limit for the paid resource. Reuse provider/platform controls where effective instead of adding a new billing system for a tiny app.

## Alerts are not spending caps

Distinguish a billing notification, rate limit, and enforced spending/usage cap. Confirm the control's actual behavior, reset period, scope, and whether processing already in flight can still incur charges. Do not claim costs cannot exceed a budget merely because an alert is configured.

If the provider has no effective hard cap, identify the application-side bounds and remaining exposure. Resolve any material exposure outside agreed limits before enabling the workload.

## Verify and report proportionally

Before activation, check that expensive operations pass through the intended server-side controls and that automated retries cannot bypass them. Exercise limit/error paths with sandbox or non-billing checks when feasible.

For material paid resources, report the estimated recurring/usage cost, assumptions, limits actually configured, and any remaining activation decision. Stop task-created temporary paid resources or jobs when no longer needed and authorized to do so; do not remove existing user resources as cleanup.
