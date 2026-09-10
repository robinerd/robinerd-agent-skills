# Anti-patterns and extra checks

## Anti-patterns

### The implementation transcript

> Added class A, changed class B, modified function C.

Use a PR description for intent, behavior, risks, and review focus.

### The architecture noun list

> API, worker, database, queue, cache, service.

Explain responsibilities, boundaries, data flow, and semantics.

### The signature-only reference

> `getUser(id: string): Promise<User>`

Explain observable behavior, errors, side effects, defaults, and important guarantees.

### The jargon cloud

> The service leverages a highly scalable, decoupled, event-driven paradigm.

Replace vague terminology with concrete behavior.

### The giant introduction

Do not put every implementation detail into the overview. Orient first; disclose complexity progressively.

### The oversimplified documentation

Do not remove important behavior merely to shorten the document. Document consequential complexity.

### The FAQ graveyard

If a recurring question is a fundamental concept, improve the main documentation rather than accumulating disconnected FAQs.

### The stale-history document

Do not mix obsolete details, old decisions, current behavior, and future plans in one document.

### The false certainty

Never invent rationale, guarantees, performance characteristics, or failure behavior. If the repository does not establish something, state the uncertainty.

## Surprises worth calling out

Ask: what would surprise a competent developer?

Prioritize documenting:

- Synchronous-looking operations that are actually asynchronous
- Soft deletion
- Eventual consistency
- Duplicate message delivery
- Non-idempotent operations
- Unexpected status codes
- Hidden retries
- Timeouts that do not cancel underlying work
- Feature-flag-dependent behavior
- Unexpected mutation of input values
- Unusual transaction boundaries

Document surprising behavior prominently — do not bury it in notes.

## Cognitive-load heuristics (pre-finalize)

- Can the reader understand the purpose quickly?
- Does the document establish a mental model before details?
- Are related facts close together?
- Are headings meaningful and skimmable?
- Are examples explanatory rather than decorative?
- Are tables used where comparisons would otherwise need prose?
- Are advanced details separated from common usage?
- Does the reader need to remember information unnecessarily?
- Have constraints and surprising behaviors been made explicit?

If a paragraph requires remembering several unrelated facts, restructure it.

## Uncertainty

Distinguish: guaranteed, currently observed, typical, expected, assumptions, unverified claims, future plans. Do not present unverified claims as fact.

## Invariants language cues

Words such as `must`, `cannot`, `always`, `never`, `exactly`, `at least once`, `at most once`, `only after`, and `before` often indicate contracts worth documenting explicitly.
