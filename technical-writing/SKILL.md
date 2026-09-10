---
name: technical-writing
description: Create, update, review, and improve developer-facing documentation (internal or external) for software projects. Use proactively when documenting architecture, APIs, functions, classes, configuration, technical decisions, PRs, changelogs, design changes, workflows, operational behavior, or code that needs explanatory documentation.
---

# Technical Writing

Produce developer-facing documentation that is accurate, useful, easy to scan, appropriately detailed, and maintainable.

This is a **documentation-engineering** skill: first determine what kind of documentation is needed, inspect the repository for facts, then apply the right level of detail.

Primary goal (not mere brevity):

> Make the reader understand what matters, know what to do, and find the exact details they need without reconstructing the system themselves.

Optimize for **minimum cognitive load while preserving the information required for correct understanding**.

## Use when

- Architecture or system documentation
- API or endpoint documentation
- Function, method, class, module, or package documentation
- Configuration or CLI reference
- Technical design documents and ADRs
- Pull request descriptions
- Changelogs or release notes
- Migration documentation
- Developer guides and how-tos
- Explaining complex implementation behavior
- Documenting errors, failure modes, concurrency, consistency, retries, or operational behavior
- Reviewing existing technical documentation for correctness or clarity
- Turning implementation knowledge into human-readable documentation

Do not treat every request as the same document type. First identify the reader, task, and documentation type.

## Core principles



### 1. Write for the reader's task

Before writing, determine:

- Who is reading this?
- What are they trying to understand or accomplish?
- What do they already know?
- What decision or action should they take afterward?
- What would be dangerous or expensive to infer incorrectly?

If unclear, inspect the repository and surrounding docs before asking. Ask only when missing information materially changes the result.

### 2. Separate documentation purposes


| Type                | Primary question                          |
| ------------------- | ----------------------------------------- |
| README              | What is this?                             |
| Getting started     | How do I begin?                           |
| Tutorial            | Can you teach me?                         |
| How-to              | How do I accomplish X?                    |
| Concept/explanation | Why does this work this way?              |
| Architecture        | How is the system structured?             |
| API reference       | What exactly is the interface contract?   |
| Function reference  | What exactly does this function do?       |
| ADR                 | Why was this design chosen?               |
| PR                  | Why was this change made?                 |
| Changelog           | What changed between versions?            |
| Runbook             | What do I do when this breaks?            |
| Code comment        | Why is this local implementation unusual? |


When appropriate, separate overview, usage, explanation, and reference material.

### 3. Use progressive disclosure

Organize broad → deep:

1. Orientation
2. Mental model
3. Important behavior
4. Typical usage
5. Detailed behavior
6. Edge cases
7. Exact reference or implementation details

Readers should stop after the overview if they only need orientation. Do not put implementation details before explaining what the component does and why it exists.

### 4. Optimize for cognitive load

Prefer: related facts together, descriptive headings, short sections, concrete examples, tables for comparisons, lists for parallel items, diagrams for relationships/flows, consistent terminology, explicit contracts.

Avoid scattering important facts across unrelated sections.

### 5. Concise about the obvious; detailed about the consequential

> Be concise about what is obvious, detailed about what is consequential, explicit about what is surprising, and exhaustive about what constitutes a contract.

Do not simplify away necessary complexity merely to shorten documentation.

## Repository investigation

Before factual technical claims, inspect the source of truth:

- Source code, tests, public interfaces/schemas
- Existing documentation and ADRs
- Configuration, dependencies, build/deploy config
- Database migrations/schemas, events, API specs
- Recent commits / PR history when intent matters
- Generated docs only when authoritative

Prefer repository evidence over assumptions.

When behavior is unclear:

1. Search for the symbol or concept
2. Inspect implementation
3. Inspect tests
4. Inspect callers/consumers
5. Check configuration and related types
6. Check existing documentation
7. Check recent changes if historical intent matters
8. Only then write

Never invent implementation details, guarantees, defaults, performance characteristics, error behavior, or architectural rationale. If uncertain, say so.

## Facts vs decisions vs assumptions

Keep categories separate:


| Category       | Meaning                     | Example                                                   |
| -------------- | --------------------------- | --------------------------------------------------------- |
| Fact           | Current behavior            | The worker stores jobs in PostgreSQL.                     |
| Decision       | Intentionally chosen        | PostgreSQL is the system of record for workflow state.    |
| Rationale      | Why a decision was made     | PostgreSQL provides the required transactional semantics. |
| Assumption     | Relied upon, not guaranteed | Workflow state is expected to remain below 10 GB.         |
| Constraint     | Imposed requirement         | Must support at least 1,000 requests per second.          |
| Recommendation | Advice                      | Clients should retry `503` with exponential backoff.      |




## Language and style

- Prefer precise technical terms over vague paraphrase; define unfamiliar terms once, then use consistently.
- Avoid decorative jargon ("leverages an asynchronous execution paradigm").
- Keep distinct concepts distinct; do not alternate synonyms for the same thing.
- Modal language: `must` / `must not` (mandatory), `should` (recommendation), `may` (permission), `can` (capability), `typically` (common, not guaranteed), `always` / `never` (only when justified).
- Prefer concrete active voice: "The worker deletes expired sessions." Use passive when the actor is irrelevant.
- One meaningful cognitive operation per sentence — not artificially short fragments, not multi-condition monsters.



### Headings

Descriptive: `## Request lifecycle`, `## Retry behavior`. Avoid `## Details`, `## Other`, `## Miscellaneous`. Headings are navigation and search anchors.

### Lists, tables, prose

Prose for explanations; lists for parallel items; tables for parameters, errors, comparisons, state transitions, defaults. Do not bullet-ize normal prose.

### Code formatting

Inline code for literals (`UserService`, `POST /users`, `config.yaml`). Code blocks for commands, source, configs, schemas. Do not code-format ordinary emphasis.

## What to document thoroughly

Spend effort where misunderstanding is expensive:

```
Obvious → brief
Non-obvious → explicit
Consequential → detailed
Security / compatibility / API contract → precise and exhaustive
```

Always prioritize documenting when relevant:

- **Defaults and config** — defaults, required/optional, nullability, empty values, env vars, precedence, limits, feature flags, version-specific behavior
- **Errors and failure** — success/failure/retryability/recovery; timeouts; partial failure; duplicates; rollbacks; async failure
- **Invariants and guarantees** — words like `must`, `cannot`, `always`, `exactly`, `at least once` often mark contracts
- **Surprises** — soft delete, eventual consistency, hidden retries, non-idempotent ops, unexpected status codes, timeouts that do not cancel work, flag-dependent behavior
- **Versioning** — state when behavior changed (`Since v3.2, …`); do not mix incompatible contracts



## Historical information belongs in the right artifact


| Concern                          | Artifact              |
| -------------------------------- | --------------------- |
| Current behavior                 | Current documentation |
| Why a major design was chosen    | ADR                   |
| Why a particular change was made | PR                    |
| What changed between releases    | Changelog             |
| Local implementation rationale   | Code comment          |


Do not turn current docs into a project chronology.

## Avoid contradictory duplication

Useful contextual repetition is fine (endpoint: "Requires an OAuth access token" + full auth doc elsewhere). Avoid duplicated facts that can drift apart.

## Discoverability and links

Write for skimmers and searchers. Prefer task-shaped headings (`## Handle \`429 Too Many Requests`) when troubleshooting is the job. Links should answer "where next?" with a clear reason — not "see this for more information."

## Documentation workflow

1. **Classify** — tutorial, how-to, explanation, reference, architecture, ADR, PR, changelog, runbook, comment, other
2. **Identify the reader** — audience, knowledge, goal, expected action
3. **Investigate** — code, tests, config, APIs, schemas, docs, history
4. **Build the mental model** — components, responsibilities, data flow, state, boundaries, contracts, failures, decisions
5. **Choose hierarchy** — Purpose → Context → Mental model → Normal behavior → Usage → Semantics → Failures → Reference → Rationale (adapt to type)
6. **Write the overview first** — busy developer gets the important 20% without the remaining 80%
7. **Add depth** by consequence, surprise, and contractual importance
8. **Verify claims** against code, tests, types, config, authoritative docs
9. **Review cognitive load** — sentences, terms, grouping, headings, examples, defaults, failures
10. **Review maintainability** — staleness risk, source of truth, right artifact, natural update path



## Quality gate



### Reader

- [ ] Audience and task clear; prior knowledge reasonable; purpose immediate



### Structure

- [ ] Right document type; overview before depth; descriptive headings; progressive disclosure; related facts grouped



### Technical correctness

- [ ] Claims checked against the repo; nothing invented; defaults/errors/side effects/invariants/versions documented where relevant; uncertainty not presented as fact



### Language

- [ ] Consistent terminology; precise technical terms; no decorative jargon; concrete sentences; intentional modals; consistent code formatting



### Cognitive load

- [ ] Mental model given, not reconstructed; useful examples; tables/lists where appropriate; advanced detail separated; surprises easy to find



### Maintainability

- [ ] Current behavior vs history separated; changelog/ADR/API material not mixed; no contradictory duplication; clear relationship to the system



## Final standard

> A competent developer should understand the important 80% quickly, find the exact 20% they need, and trust that documented behavior reflects the actual system.

- **Five-minute test** — what/why/where it fits/normal flow/constraints/where deeper info lives
- **Ten-minute implementation test** (usage/API) — auth, I/O, defaults, errors, behavior, retry/idempotency, limits without reading source
- **Six-month test** (architecture/design) — why it looks this way, boundaries, deliberate decisions, rejected alternatives, assumptions



## Output behavior

When creating or updating documentation:

1. Inspect the repository before technical claims
2. Identify documentation type and reader
3. Prefer the smallest structure that fully serves the task
4. Overview before deep implementation detail
5. Precise technical terminology
6. Explain `why` when not obvious from code
7. Document observable behavior, not merely restated implementation
8. Explicitly document important defaults, constraints, errors, side effects, invariants, and surprises
9. Keep historical rationale in ADRs/PRs, not current-state docs
10. Verify against implementation and tests
11. Do not claim verification that did not happen
12. Mark genuine uncertainty; never invent
13. Preserve project terminology and doc conventions unless change is compelling
14. When editing, preserve useful local structure; improve incrementally
15. Prefer human-readable docs over merely comprehensive-looking ones

Desired result: **accurate, structured, discoverable, precise, appropriately detailed, cognitively light, technically meaningful, and maintainable.**

## Additional resources

- Document-type templates (architecture, API, function, PR, ADR, changelog, comments): [templates.md](templates.md)
- Anti-patterns and elaboration on surprises / comments: [anti-patterns.md](anti-patterns.md)

