---
name: technical-writing
description: Create, update, review, and improve developer-facing documentation (internal or external) for software projects. Use proactively when documenting architecture, APIs, functions, classes, configuration, technical decisions, PRs, changelogs, design changes, workflows, operational behavior, or code that needs explanatory documentation.
---

# Technical Writing

Produce technical documentation that is accurate, useful, well-structured, easy to understand, and maintainable.

Consider who will read this, why, and what kind of doc is needed. Inspect the structure and information available, then choose depth and structure that fit the reader’s needs.

Make it easy for the reader to understand what matters, know what to do, and find the details they need without reconstructing the system themselves.

Before writing, consider:

1. Who is reading, and what are they trying to do?
2. What must they understand to do their job?
3. What would be expensive or dangerous to infer wrongly?
4. What is already obvious from names, signatures, or nearby code?
5. What is non-obvious, consequential, or contractual?

Then invent the smallest structure that answers those questions for this project and doc type. Prefer the repo’s existing doc conventions and structure of similar documents when applicable.

## Principles

All of the below are to be used as general guidance, not laws. The specific context and circumstances may mean doing things differently, case-by-case.

### 1. Write for the reader’s task

**Prefer:** purpose, next action, and decision-relevant constraints up front.  
**Avoid:** dumping implementation chronology or every related fact before the reader knows why they are here.

Ask only when missing information would materially change the result; otherwise inspect the repo first.

### 2. One primary purpose per document

Different artifacts answer different questions (what is this, how do I start, how do I do X, why is it this way, what is the contract, why this change, what broke). Split overview, usage, explanation, and reference when one page is trying to do all of them badly.

**Prefer:** a how-to that gets someone unblocked, with a link to deeper reference.  
**Avoid:** a single mega-doc that mixes tutorial, full API catalog, and historical design narrative.

### 3. Progressive disclosure

Orient → mental model → normal behavior → usage → deeper semantics → edge cases → exact reference. (or similar logical structure, don't treat those literally as headers)

**Prefer:** a short overview a busy engineer can stop after.  
**Avoid:** opening with internals, file lists, or exhaustive edge cases before the reader knows what the thing is.

### 4. Low cognitive load

Keep related facts together. Use descriptive headings, short sections, concrete examples, tables for comparisons/parameters, lists for parallel items, diagrams for relationships/flows, and consistent terms.

**Prefer:**

> `timeout` — max request duration in ms. Default `30000`. `0` disables the client-side timeout.

**Avoid:** scattering default, units, and “what zero means” across three sections.

**Prefer:** headings that name the topic (`Retry behavior`, `Auth`).  
**Avoid:** `Details`, `Other`, `Miscellaneous`, or skill-specific labels that are not natural section titles for this project.

### 5. Detail scales with consequence

> Be brief about the obvious, explicit about the non-obvious, detailed about the consequential, and precise about contracts.

**Prefer:** documenting defaults, auth, errors, side effects, idempotency, consistency, and invariants when misunderstanding is expensive.  
**Avoid:** shortening away failure modes “to keep it clean,” or padding obvious getters with essay-length prose.

When behavior would catch a competent developer off guard (soft delete, eventual consistency, hidden retries, non-idempotent ops, timeouts that do not cancel work, flag-gated behavior), put it where they will see it—in normal prose under a natural heading—not buried, and not under a stock label invented by this skill.

### 6. Verify before you assert

Inspect code, tests, interfaces/schemas, config, existing docs/ADRs, and history when intent matters. Prefer evidence over vibe.

**Prefer:** “Retries up to 3 times with exponential backoff” (confirmed in code/tests).  
**Avoid:** inventing guarantees, performance claims, or rationale. If unsure, say what is known vs unknown.

Keep categories straight: fact (current behavior), decision, rationale, assumption, constraint, recommendation. Do not present assumptions as guarantees.

### 7. Precise language, project vocabulary

Use real technical terms when they increase precision; define once if ambiguous, then stick to one name per concept.

**Prefer:** “The worker processes jobs asynchronously.”  
**Avoid:** “leverages a highly scalable, decoupled, event-driven paradigm.”

Use modals deliberately: `must` / `must not` (requirement), `should` (advice), `may`/`can` (permission/capability), `typically` (common, not guaranteed), `always`/`never` (only when justified).

**Prefer:** one idea per sentence; active voice when the actor matters.  
**Avoid:** Mixing different synonyms (e.g `request ID` / `correlation ID` / `trace id`) for the same thing). Long explicit multi-condition sentences.

### 8. Right artifact for the information


| Concern                       | Usually belongs in      |
| ----------------------------- | ----------------------- |
| Current behavior              | Current docs            |
| Why a major design was chosen | ADR / design doc        |
| Why this change landed        | PR description          |
| What changed between releases | Changelog               |
| Local oddity in code          | Comment (why, not what) |


**Prefer:** current docs that describe the system as it is.  
**Avoid:** turning a reference page into a project chronology, or mixing obsolete plans with live contracts.

Useful short repetition is fine (“requires an OAuth token”); contradictory duplicated sources of truth are not.

### 9. Structure based on needs

Document structure depends on what the reader must learn.

- **Architecture:** responsibilities, boundaries, data/state ownership, sync vs async, failure/retry, consistency, trust boundaries—and *why* those boundaries exist. Diagrams for structure; prose for semantics.
- **API / function reference:** the observable contract (inputs, outputs, defaults, errors, side effects, auth, idempotency, limits)—not only the signature.
- **PR:** intent, behavior change, risks, test/rollout notes, review focus—not a file-by-file transcript the diff already shows.
- **ADR:** one decision: context, options, choice, rationale, consequences.
- **Changelog:** curated user/dev-relevant changes—not every commit.
- **Comments:** why the non-obvious approach exists; never `// increment counter`.

**Prefer (PR):** why, observable impact, risks, what was verified.  
**Avoid (PR):** “Added class A. Updated class B. Added tests.”

**Prefer (API):** what happens when the user is missing, deleted, unauthorized, or the call is retried.  
**Avoid (API):** signature-only docs that leave behavior unwritten.

**Prefer (comment):**

```text
// Publish after commit so consumers never see uncommitted state.
```

**Avoid (comment):**

```text
// Publish the event.
```



### 10. Discoverability

Write for skimmers and searchers. Headings and terms should match how engineers look for help. Links should say why to follow them.

**Prefer:** “See message delivery for retry and deduplication.”  
**Avoid:** “See this for more information.”

## Working loop

1. Classify the artifact and reader.
2. Investigate the source of truth.
3. Form the mental model (parts, flow, state, contracts, failures).
4. Choose a hierarchy that fits *this* doc—overview first.
5. Add depth where cost of misunderstanding is high.
6. Verify claims; mark uncertainty honestly.
7. Skim for cognitive load and maintainability (will this rot? wrong artifact? contradictory dupes?).



## Done when

A competent technical reader can:

- grasp what this is, why it exists, and the normal path quickly
- find exact contract details (defaults, errors, limits, edge behavior) without reading all the source
- trust that written behavior matches the system—and see clearly where something was not verified

Ship docs that are **accurate, structured, discoverable, precise, appropriately detailed, cognitively light, and maintainable**—using the project’s own voice, not this skill’s vocabulary as decoration.