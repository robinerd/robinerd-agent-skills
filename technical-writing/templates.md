# Document-type templates

Use only the sections that are meaningful. Detail scales with **semantic complexity**, not line count.

## Architecture documentation

Mental model before implementation details. Typical structure:

```
Overview
Goals
Non-goals
System context
Major components
Responsibilities and boundaries
Important data flows
State and ownership
Failure handling
Security boundaries
Scalability characteristics
Operational considerations
Design decisions
Related documentation
```

Explain:

- What each major component owns and is responsible for
- Which components may call each other
- Where authoritative state lives
- Where transaction boundaries exist
- Sync vs async operations
- How failures propagate and how retries work
- Strong vs eventual consistency
- Security / trust boundaries
- Why important boundaries exist (not just boxes in a diagram)

A useful flow to make understandable:

```
Request
  → authentication
  → validation
  → business operation
  → persistence
  → event publication
  → response
```

Then document semantics of each important step.

### Diagrams

Use diagrams for: component relationships, request/event flows, dependency graphs, state machines, deployment topology.

Use prose for: why relationships exist, guarantees, constraints, failure behavior, trade-offs, semantics. A diagram is not a substitute for explaining behavior.

## API documentation

Describe the **contract**, not merely the implementation.

Consider documenting: purpose, authn/authz, inputs, required vs optional, types, defaults, nullability, constraints, outputs, errors, side effects, idempotency, pagination, ordering, rate limits, retry behavior, consistency, edge cases, examples.

### Endpoint template

```
## `POST /orders`

Creates an order.

### Authentication

...

### Request

#### Headers

...

#### Body

...

### Response

`201 Created`

...

### Errors

- `400` — invalid request.
- `401` — authentication failed.
- `409` — conflicting state.
- `429` — rate limit exceeded.

### Semantics

- ...

### Example

...

### Related endpoints

...
```

### Function or method template

```
### `functionName`

One-sentence purpose.

#### Behavior

What it actually does.

#### Parameters

Meaning, constraints, defaults, and nullability.

#### Returns

Type and semantic meaning.

#### Errors

Failure conditions.

#### Side effects

State changes, I/O, events, mutations, etc.

#### Concurrency

Only when relevant.

#### Performance

Only meaningful characteristics or constraints.

#### Example

Typical usage.

#### Notes

Important surprising behavior.
```

### Behavior, not signatures alone

A signature like `getUser(id: string): Promise<User>` does not establish missing-user behavior, soft-delete visibility, caching, consistency, authorization, side effects, or retryable errors. Document those semantics when they matter.

## Defaults and configuration

Prefer:

> `timeout` — maximum request duration in milliseconds. Defaults to `30_000`. Set to `0` to disable the client-side timeout.

Document: defaults, required/optional, nullability, empty-value behavior, env vars, precedence, limits, feature flags, version-specific behavior. Readers should not need source to discover important defaults.

## Errors and failure behavior

For important operations, cover:

```
Success
Failure
Retryability
Recovery
```

For retries: safety, idempotency requirements, backoff, limits, headers/metadata, exhaustion behavior.

Also cover: timeouts, dependency failures, partial failure, concurrency, stale data, duplicates, lost connections, rollback, async processing. Do not document only the happy path.

## Examples

Prefer a few high-value examples:

1. Minimal — smallest useful invocation
2. Typical — realistic application
3. Important edge case — only when non-obvious

Demonstrate semantics, not merely syntax. Avoid redundant examples.

## Pull request documentation

Explain what the diff cannot easily communicate. Prefer:

```
## Summary

...

## Context

Why this change is necessary.

## Approach

What changed and the important design choices.

## Behavior

What users or dependent systems will observe.

## Alternatives

Important alternatives considered.

## Testing

What was tested and what was not.

## Risks

Potential regressions or operational concerns.

## Rollout

Feature flags, migrations, deployment ordering, etc.

## Review focus

Specific areas where reviewer attention is particularly valuable.
```

For small PRs, use fewer sections. Do not turn the description into an implementation transcript ("Added class A. Updated class B."). Focus on why, behavior change, design decisions, risks, testing, rollout, review focus.

## Architecture Decision Records

One significant decision and its rationale — not a full system specification.

```
# ADR-NNN: Decision title

Status: Proposed | Accepted | Superseded | Deprecated
Date: YYYY-MM-DD

## Context

Problem, requirements, constraints, and relevant background.

## Options considered

### Option A

Pros:
- ...

Cons:
- ...

### Option B

Pros:
- ...

Cons:
- ...

## Decision

State the chosen option clearly.

## Rationale

Explain why it was selected.

## Consequences

### Positive

- ...

### Negative

- ...

## Rejected alternatives

...

## Related decisions

...
```

Important: context, decision, alternatives, rationale, consequences. Record credible alternatives that materially influenced the decision.

## Changelogs and release notes

Curated notable changes — not a commit log. Categories: Added, Changed, Deprecated, Removed, Fixed, Security, Breaking.

```
## [2.4.0] - 2026-09-10

### Added

- Added support for bulk user imports.

### Changed

- API requests now return `429` when the account rate limit is exceeded.

### Fixed

- Fixed an issue where expired sessions could remain active after restart.

### Breaking

- `User.email` is now required when creating users.
```

Omit internal refactors unless they have meaningful consequences for users or maintainers.

## Code comments

Explain what the code does not adequately communicate: unusual approach, why simpler is wrong, invariants, external constraints, compatibility workarounds, non-obvious performance/concurrency.

Avoid restating code (`// Increment count.`). Prefer why:

```
// Increment before publishing so the emitted metric reflects the
// state observed by downstream consumers.
count++;
```

Do not use comments as a substitute for clear code when the code can be improved.
