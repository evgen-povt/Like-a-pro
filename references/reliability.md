# Reliability and operational sanity

Apply these checks only where the feature can fail under real-world load, retries, partial failure, or external dependency behavior.

## External requests

For network calls:
- set finite connect/read/overall timeouts appropriate to the operation;
- avoid infinite/default hangs;
- retry only failures that are safe to retry;
- use exponential backoff/jitter when repeated retries are justified;
- bound response/body sizes where untrusted or potentially huge;
- propagate cancellation/deadlines when the stack supports it.

Do not blindly retry non-idempotent operations.

## Idempotency

Consider idempotency for:
- payment/value-changing requests;
- webhooks;
- job retries;
- message processing;
- client retries across flaky networks;
- provisioning/external API creates.

An idempotency key needs a durable uniqueness rule/record where duplicate side effects matter.

## Transactions and invariants

When multiple writes must succeed/fail together:
- use a transaction where the storage model supports it;
- define the invariant explicitly;
- do not split an atomic business operation into independent writes without compensation/recovery logic.

Remember: a DB transaction cannot roll back an email, blockchain transaction, payment-provider call, or another external system.

## Concurrency

For shared mutable state:
- identify races/lost updates;
- use database constraints/atomic updates/locking/optimistic concurrency as appropriate;
- do not assume a read-then-write sequence is atomic;
- test concurrent behavior when the invariant is important.

## Database performance

For potentially large datasets:
- avoid unbounded queries;
- paginate/stream where appropriate;
- verify indexes for common selective predicates/joins;
- avoid N+1 queries on hot paths;
- inspect query plans when performance is actually in question;
- do not add indexes speculatively without considering write/storage cost.

## Resource limits

Bound attacker/user-controlled resource consumption where relevant:
- upload size;
- request body size;
- pagination limit;
- recursion/decompression;
- concurrency;
- queue depth;
- generated output size.

## Background jobs

Jobs should have:
- clear retry behavior;
- idempotency or duplicate tolerance when needed;
- visibility into failed/dead-letter work;
- bounded attempts/backoff;
- enough context to diagnose without leaking secrets.

## Errors

- Preserve root cause internally while returning safe public errors.
- Do not swallow exceptions silently.
- Do not convert every error into success/empty result.
- Distinguish retryable vs permanent failures when the caller needs to react.

## Observability

For important flows, ensure enough telemetry exists to answer:
- Did the request/job happen?
- Did it succeed?
- How long did it take?
- Which dependency failed?
- Can related events be correlated?

Use structured logs/metrics/traces when the existing system supports them. Do not introduce an observability platform for a tiny project merely because it is fashionable.

## Performance work

Measure before optimizing when possible.

Prefer:
1. identify the actual bottleneck;
2. capture a baseline;
3. change one meaningful factor;
4. re-measure;
5. check correctness/regression.

Do not trade away security or correctness for unmeasured performance gains.
