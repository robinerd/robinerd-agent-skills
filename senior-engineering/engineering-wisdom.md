# Engineering Wisdom

This is a field guide of engineering heuristics. Treat these as defaults, not laws: context wins, but good defaults keep teams from relearning the same lessons the hard way.

## Core Attitude

- Optimize for the outcome, not for proving that your idea was clever.
- Prefer simple, boring, well-understood solutions unless the problem clearly deserves novelty.
- Think out loud early when the decision is ambiguous, risky, or likely to affect other people.
- Make uncertainty explicit: write down what you know, what you assume, and what would change your mind.
- Ask "what problem are we solving?" before asking "what code should I write?"
- Ask "what breaks if this is wrong?" before deciding how much process, testing, or review is enough.
- Own the consequences of your work after it ships.
- Make the next engineer's job easier, even when the next engineer is you in six months.
- Favor steady improvement over heroic cleanup.
- Be skeptical of both overengineering and underengineering; both are failures to fit the solution to the problem.
- Prefer evidence over vibes: logs, metrics, traces, tests, user reports, profiling, and production behavior.
- Do not hide risk. Surface it early enough that the team still has options.
- A senior engineer reduces ambiguity for others without pretending ambiguity does not exist.
- Your work is not done when the code compiles; it is done when the system, team, and users can safely live with it.

## Understanding The Problem

- Clarify the user-visible behavior before choosing the implementation.
- Separate symptoms from causes when debugging or designing a fix.
- Identify the smallest useful outcome that proves the direction is right.
- Look for the constraint that actually matters: time, correctness, latency, cost, reliability, simplicity, security, migration risk, or team capacity.
- Before building a general system, name at least two real use cases it must support.
- If you cannot explain the requirement in plain language, you probably cannot implement it cleanly.
- Ask whether the problem is technical, product, operational, organizational, or communication-related.
- A workaround is acceptable when it is deliberate, documented, reversible, and proportionate to urgency.
- Avoid solving a more prestigious problem than the one users actually have.
- Validate with real data or real usage as early as possible; production reality is often stranger than design-room reality.

## Clean And Maintainable Code

- Code is read more often than it is written; optimize for the reader.
- Prefer clarity over cleverness.
- Make the happy path obvious and the exceptional paths explicit.
- Keep related logic close together unless separation reduces real complexity.
- Name things after the domain concept they represent, not after the mechanics used to implement them.
- Use precise names: `timeoutMs` is better than `timeout`; `pendingInvoiceIds` is better than `items`.
- Avoid names that encode lies, stale assumptions, or implementation details that may change.
- Keep functions small enough that their purpose is obvious, but do not split code into fragments that force readers to jump around for no gain.
- Keep modules cohesive: a file should have a reason to change that is easy to describe.
- Minimize coupling across ownership, deployment, persistence, and abstraction boundaries.
- Prefer explicit data flow over hidden global state.
- Prefer immutable values and narrow mutation scopes when practical.
- Avoid surprising side effects in functions that look like queries or transformations.
- Treat error handling as part of the design, not cleanup after the "real" code.
- Avoid swallowing errors unless you can explain why losing that signal is safe.
- Include enough context in errors for the person debugging at 2 AM.
- Use comments to explain why, tradeoffs, invariants, edge cases, or surprising constraints.
- Delete comments that merely restate the code or preserve obsolete history.
- Let the existing codebase teach you its patterns before introducing a new one.
- Consistency is valuable even when the local style is not your favorite, as long as it does not harm correctness or maintainability.
- Avoid speculative abstractions; duplication is sometimes cheaper than premature generality.
- Remove duplication when the duplicated code represents the same concept and is likely to change together.
- Keep public APIs smaller and more stable than private implementation details.
- Prefer explicit contracts at boundaries: types, schemas, validation, documented assumptions, and tests.
- Make invalid states unrepresentable when the language and codebase make that reasonable.
- Avoid Boolean parameters whose meaning is unclear at call sites; prefer named options or separate functions.
- Do not mix unrelated concerns in one change just because you noticed them nearby.

## Design And Tradeoffs

- Every design optimizes for something and sacrifices something else.
- Write down the tradeoff when the cost is non-obvious.
- Prefer reversible decisions when the right answer is uncertain.
- Use irreversible decisions sparingly and with more review.
- Reduce blast radius: isolate risky changes behind flags, narrow deployments, small migrations, or limited surfaces.
- Optimize for the expected lifetime of the code. A throwaway script and a protocol API deserve different care.
- Avoid building frameworks when a function, module, or local helper will do.
- Add an abstraction when it removes meaningful duplication, protects a volatile boundary, or gives the domain a clearer shape.
- Do not add an abstraction just to make code look "architectural."
- Treat simplicity as a feature, but remember that simple for the author is not always simple for the system.
- Prefer boring dependencies over fashionable dependencies unless the new tool clearly changes the economics.
- Consider build-vs-buy through maintenance cost, operational burden, data ownership, security, integration risk, and exit cost.
- Make the common path easy and the dangerous path explicit.
- Prefer one source of truth for state that must remain consistent.
- When two systems must agree, decide which one is authoritative and how drift is detected.
- Think about failure modes at the design stage: partial failure, retries, duplicate requests, stale caches, bad inputs, slow dependencies, and clock issues.
- Separate durable decisions from reversible configuration.
- Design migrations so old and new versions can coexist when deployments are not atomic.
- Do not mistake "flexible" for "good"; flexibility has cognitive and testing costs.
- It is okay to choose a narrow solution when the problem is narrow and the cost of changing later is low.

## Testability And Tests

- Tests should increase confidence, not just coverage numbers.
- Test behavior and contracts more than implementation details.
- Good tests fail when the behavior is broken and keep passing through harmless refactors.
- Add tests for the bug before or alongside the fix when practical.
- Cover the happy path, important edge cases, and meaningful failure modes.
- Test the risks introduced by the change, not every line equally.
- If a change touches money, identity, permissions, data loss, migrations, or safety, raise the testing bar.
- Avoid tests that assert incidental formatting, ordering, timestamps, or private structure unless those are part of the contract.
- Prefer deterministic tests. Control clocks, randomness, network, and filesystem state where possible.
- Make tests readable enough that they document expected behavior.
- Keep test setup minimal; large fixtures hide the reason a test exists.
- Avoid mocks that only prove your code called the mock the way you wrote it.
- Mock slow, flaky, expensive, or external dependencies; prefer real logic for your own domain code when practical.
- Integration tests should test real integration boundaries, not unit tests with extra steps.
- End-to-end tests are valuable for critical flows but expensive; keep them focused.
- If a test is flaky, treat it as a production bug in the test suite.
- When refactoring code with weak coverage, consider adding characterization tests first.
- If tests are hard to write, listen to the design feedback: the code may be too coupled or unclear.
- Do not ask reviewers to catch bugs that tests could catch cheaply.
- CI should run the checks humans should not have to remember.

## Debugging

- Reproduce before changing code when possible.
- Generate hypotheses before editing: list plausible causes, then test the most likely ones.
- Use logs, metrics, traces, debuggers, and small experiments to distinguish causes.
- Add temporary diagnostic logs deliberately; remove or convert them to useful permanent observability before merging.
- Check recent changes, dependencies, configuration, data shape, permissions, time, concurrency, and environment differences.
- Make the bug smaller: isolate inputs, reduce the test case, and remove unrelated variables.
- Beware of fixing the first visible symptom while leaving the cause intact.
- If the bug disappears when observed, suspect timing, concurrency, caching, undefined behavior, or environmental state.
- Keep a debugging trail when the investigation is non-trivial; future incidents often rhyme.
- Once fixed, ask what would have caught it earlier: a test, alert, type, invariant, validation, runbook, or smaller deployment.

## Performance And Resource Use

- Measure before optimizing unless the performance bug is obvious by inspection.
- Know the performance budget: latency target, throughput target, memory ceiling, battery budget, cost ceiling, or user tolerance.
- Optimize the bottleneck, not the code that feels inelegant.
- Understand algorithmic complexity before tuning constants.
- Watch allocations in hot paths, tight loops, real-time code, mobile code, and high-throughput services.
- Caching trades compute for freshness, memory, invalidation complexity, and operational risk.
- Add caches only with a clear invalidation story and a way to observe hit rate and failures.
- Avoid unbounded queues, maps, retries, concurrency, memory growth, and log volume.
- Put timeouts on remote calls.
- Use retries only with limits, backoff, jitter, and idempotency awareness.
- Treat latency distributions seriously; p95 and p99 often matter more than averages.
- Optimize for perceived performance when user experience is the concern.
- Make slow operations cancellable when users or callers may give up.
- Prefer streaming or pagination when data can grow without bound.
- Understand the cost of serialization, deserialization, copying, network round trips, database queries, and lock contention.
- Use profiling tools before rewriting working code.
- Performance work should leave behind a benchmark, metric, or note explaining the win and the tradeoff.
- Do not make code significantly harder to understand for a marginal speedup outside a hot path.

## Refactoring And Technical Debt

- Refactoring means changing internal structure without changing external behavior.
- Keep tests green while refactoring; if you cannot, make smaller steps.
- Prefer opportunistic refactoring near the code you are already changing.
- Leave the code better than you found it, but do not turn every task into a cleanup crusade.
- Separate broad refactors from behavior changes when possible; reviewers need different mindsets for each.
- Rename, move, and reformat in separate commits or PRs when they would obscure logic changes.
- Refactor before adding a feature when the refactor makes the feature simple and safer.
- Refactor after adding a feature when the feature taught you the right shape.
- Do not refactor stable, low-change code just because it offends your taste.
- Pay down debt when the interest is slowing delivery, increasing bugs, blocking a goal, or creating operational risk.
- Take on debt only deliberately: document what was traded, why, and when to revisit it.
- A mess is not automatically prudent debt; sometimes it is just a mess.
- Avoid "while I am here" changes that are unrelated to the current goal and increase review risk.
- A large refactor should have a migration plan, test strategy, and rollback story.
- If a refactor cannot be reviewed, it cannot be trusted.

## Dividing Work Into Small Pieces

- The best change is one coherent idea that can be reviewed, tested, deployed, and reverted independently.
- Small is conceptual, not only line count: one focused 500-line generated change may be easier than 80 lines mixing five concerns.
- Slice by user value when possible: smallest useful behavior first, refinements later.
- Slice by architectural layer when that reduces review complexity: schema first, API next, UI last.
- Slice by risk: land safe mechanical changes separately from risky logic.
- Slice by novelty: isolate the new design decision from routine plumbing.
- Use feature flags to merge incomplete paths without exposing them prematurely.
- Use stacked PRs when a feature needs sequential foundations.
- Avoid one PR that contains formatting, renames, refactors, feature logic, tests, and deployment config unless there is no practical alternative.
- Make each PR leave the system in a working state.
- Design migrations so each step is safe: expand, backfill, dual-read or dual-write if needed, switch, contract.
- Keep commits coherent enough that you can reorder, split, revert, or bisect them later.
- If a branch has become too large, stop and split before asking for final review.

## PR Hygiene

- A PR is a request for someone else to spend scarce attention; make that attention count.
- Use a title that says what changed in human language.
- Explain why the change exists, not only what files changed.
- Link the issue, design note, incident, or discussion that gives context.
- Include a short implementation summary when the diff is not self-explanatory.
- Call out the riskiest or most novel parts explicitly.
- Include a test plan with commands, scenarios, screenshots, or manual steps as appropriate.
- For UI changes, include screenshots or video when useful.
- For behavior changes, include before/after examples when useful.
- For migrations or infra changes, include rollout and rollback notes.
- Keep the PR focused. If reviewers keep asking about unrelated changes, the PR is probably mixed.
- Do your own review before requesting review; remove debug code, stale comments, and accidental churn.
- Wait for CI unless the review is explicitly early or draft.
- Mark draft PRs clearly when you want design feedback before polish.
- Add inline notes where a file is mostly mechanical, generated, or intentionally unusual.
- Choose reviewers who know the area, own the risk, or can learn from the change.
- Do not request review from everyone just to spread responsibility.
- Respond to review comments with either a change, a question, or a reasoned explanation.
- Prefer synchronous conversation when a review thread is becoming a debate.
- Thank reviewers by making the next review easier, not by adding ceremony.

## Reviewing Code

- Start with intent: does this change solve the right problem?
- Then review design: does it fit the system and its direction?
- Then review correctness: does it handle important paths, edge cases, and failure modes?
- Then review tests, observability, security, performance, naming, comments, and style.
- Do not spend human review energy on formatting that tooling should enforce.
- Review the whole relevant context, not only the changed lines, when the local context may be misleading.
- Ask whether the change improves or degrades code health over time.
- Distinguish blocking comments from optional suggestions.
- Prefix nits or educational comments so the author knows they are not merge blockers.
- Be specific about why something matters.
- Prefer questions when you are uncertain and direct statements when you are sure.
- Do not use code review to relitigate decisions already made elsewhere unless new evidence appears.
- Do not demand your personal style when the existing codebase has an acceptable pattern.
- Accept code that improves the system even if it is not perfect.
- Reject code that definitely worsens code health unless there is a real emergency and a follow-up plan.
- Remember that review is also knowledge transfer and mentoring.
- A good review catches important problems without making authors afraid to submit work.

## Commit Messages And History

- A commit message should help a future reader understand intent.
- Use the subject to summarize the change; use the body to explain why when the why is not obvious.
- Prefer imperative summaries when that is the repo convention: "Add retry backoff" rather than "Added retry backoff."
- Keep commits scoped to one coherent change.
- Do not hide risky behavior changes inside "cleanup" commits.
- Mention breaking changes explicitly.
- Mention migrations, operational concerns, or follow-up requirements when relevant.
- If the repo uses Conventional Commits, follow it consistently: `fix:`, `feat:`, `docs:`, `refactor:`, and explicit breaking-change markers.
- Good commit history helps bisect regressions, generate changelogs, review stacked work, and understand old decisions.
- Squashing is fine when the final commit preserves useful intent.
- Many tiny commits are not better if they are noisy and meaningless.
- One huge commit is not better if it cannot be reviewed or reverted safely.

## Deployments And Operations

- "It works locally" is not a deployment plan.
- Know how the change reaches production and who or what can stop it.
- Prefer small, frequent deployments over rare, high-stakes deployments.
- Have a rollback or roll-forward plan before deploying risky changes.
- Rollback is not just reverting code: consider database state, caches, queues, clients, third-party systems, data migrations, and user-visible side effects.
- Use feature flags for risky behavior changes, but remember flags also add complexity and need cleanup.
- Avoid deploying high-risk changes when the team cannot respond.
- Deploy migrations in phases when old and new code may run at the same time.
- Backfill data with observability, idempotency, rate limits, and pause/resume capability.
- Make jobs restartable when possible.
- Add monitoring for new critical behavior before relying on it.
- Alert on user-impacting symptoms, not every internal twitch.
- Track the golden signals where relevant: latency, traffic, errors, and saturation.
- Use SLOs and error budgets to balance reliability work against feature work.
- DORA-style metrics are useful because they look at both throughput and instability.
- A deployment that fails safely and recovers quickly is better than one that assumes perfection.
- After incidents, improve the system rather than blaming the person closest to the failure.
- Write runbooks for operations that are rare, stressful, or easy to get wrong.

## Security, Privacy, And Data

- Treat security as a design constraint, not a final checklist.
- Use least privilege for users, services, tokens, databases, and cloud resources.
- Never commit secrets.
- Prefer secret managers over ad hoc environment files.
- Validate inputs at trust boundaries.
- Escape or parameterize outputs and queries where injection is possible.
- Authenticate who the caller is and authorize what they may do; do not confuse the two.
- Be careful with logs: do not leak tokens, personal data, payment data, private customer data, or sensitive internal state.
- Minimize data collection and retention; data you do not store cannot leak.
- Encrypt sensitive data in transit and at rest where appropriate.
- Think about abuse cases, not only normal users.
- Rate-limit expensive, public, or abuse-prone operations.
- Design admin tools as production software; internal does not mean safe.
- Make dangerous actions auditable.
- Prefer safe defaults and explicit opt-ins for risky behavior.
- Keep dependencies updated, but understand the risk of dependency churn.
- Have a response plan for leaked secrets, compromised accounts, and vulnerable dependencies.

## APIs, Compatibility, And Contracts

- An API is a promise; make fewer promises when you can.
- Be conservative in what you expose and explicit in what you require.
- Document behavior that callers can reasonably depend on.
- Version or phase changes that break external or persistent contracts.
- Preserve compatibility for shipped behavior, persisted data, and public interfaces.
- Do not preserve compatibility with unshipped branch experiments unless doing so reduces real risk.
- Make deprecations visible, timed, and supported by migration guidance.
- Return errors that callers can act on.
- Avoid leaking implementation details through API shape.
- Design idempotency for operations that may be retried.
- Think about pagination, ordering, filtering, limits, and future growth before exposing list endpoints.
- Treat schemas, events, and file formats as APIs if other code or people depend on them.

## Data And Persistence

- Data outlives code; be more careful with persisted changes than with local implementation.
- Make migrations reversible when practical and explicitly irreversible when not.
- Test migrations on realistic data volume, not only empty dev databases.
- Understand locking, indexes, query plans, and write amplification before changing hot tables.
- Back up before destructive operations.
- Prefer additive schema changes before destructive schema changes.
- Keep data transformations idempotent when jobs may be retried.
- Record enough provenance to debug how important data was produced.
- Beware of time zones, clock skew, daylight saving time, leap seconds, and inconsistent timestamp precision.
- Avoid using floating point for money or exact accounting.
- Know whether consistency, availability, latency, or cost matters most for a given data path.
- Design for partial failure when writing to multiple systems.

## Dependencies And Tooling

- Every dependency has a maintenance, security, build, licensing, and cognitive cost.
- Use the standard library or existing project utilities when they are enough.
- Prefer actively maintained dependencies with clear ownership and adoption.
- Avoid adding a dependency for a trivial helper.
- Pin or lock dependencies according to the ecosystem's norms.
- Keep generated files, formatting, and lockfile churn out of unrelated changes.
- Automate formatting so humans do not debate whitespace.
- Automate linting, type checking, tests, dependency checks, and basic security checks where reasonable.
- Make local development setup reproducible.
- A tool that only one person understands is a risk.
- Remove unused dependencies and flags when they have served their purpose.

## Documentation

- Documentation should answer what this is, why it exists, how to use it, how to test it, how to operate it, and what can go wrong.
- Update docs when behavior, setup, release process, or user-facing contracts change.
- Put documentation where future readers will look for it.
- Keep docs close to the code when they describe code-specific behavior.
- Keep architectural decisions in durable notes when they affect future work.
- Prefer examples over abstract descriptions when explaining APIs or workflows.
- Document non-obvious constraints, tradeoffs, and failure modes.
- Delete or update stale docs; wrong docs are worse than missing docs.
- Use diagrams when they reduce cognitive load.
- A good PR description is temporary documentation; important parts may need to become permanent documentation.

## Collaboration Inside An Organization

- Software engineering is a team sport played through code, docs, tools, rituals, and trust.
- Communicate early when scope, risk, or timeline changes.
- Escalate blockers before they become surprises.
- Write decisions down when they affect more people than were in the room.
- Bring options and tradeoffs, not only problems.
- Align with product and business goals without pretending engineering constraints are optional.
- Protect focus: not every good idea belongs in the current task.
- Respect ownership, but do not use ownership as a wall against collaboration.
- Share context generously; bottlenecked knowledge slows the whole organization.
- Mentor through explanations, examples, and review comments that teach principles.
- Ask for help before burning days in isolation.
- Give help in a way that leaves the other person more capable.
- Make invisible work visible: migrations, cleanup, reliability, tooling, and documentation need advocacy.
- Build systems that make the right behavior easy and the risky behavior hard.
- Culture is encoded in defaults, review standards, incident response, and what leaders tolerate.

## Working With Uncertainty

- There is rarely one perfect answer; there are tradeoffs under constraints.
- Prefer experiments when debate is cheaper to settle with data.
- Time-box investigations when the search space is large.
- Write down decision criteria before comparing options.
- Choose the simplest option that preserves the ability to learn.
- Avoid pretending estimates are commitments; explain confidence and risk.
- Split unknowns into technical unknowns, product unknowns, operational unknowns, and organizational unknowns.
- Create feedback loops: prototypes, logs, metrics, staged rollout, user testing, and post-ship review.
- When wrong, update the model and tell the people affected.

## Before Coding

- What problem am I solving?
- Who is affected by this change?
- What is the smallest useful version?
- What existing pattern should I follow?
- What assumptions am I making?
- What could break?
- What tests or evidence will prove this works?
- Does this need a design note, discussion, or early review?

## Before Opening A PR

- Is the PR focused on one coherent change?
- Did I remove unrelated churn?
- Did I run the relevant tests, linters, formatters, or type checks?
- Did I update docs or examples if behavior changed?
- Did I include a useful description and test plan?
- Did I call out risky, surprising, or intentionally unusual parts?
- Are generated files, migrations, screenshots, or data changes explained?
- Would I be able to review this PR carefully if someone else sent it to me?

## Before Merging

- Has CI passed or has the exception been made explicit?
- Are blocking review comments resolved?
- Are tests meaningful for the risk?
- Is the rollback or follow-up plan clear if needed?
- Are feature flags, config changes, docs, migrations, and dashboards ready?
- Does the change improve or at least preserve code health?
- Is this being merged at a time when the team can respond if it breaks?

## Before Deploying

- What is the expected user-visible change?
- How will I know it is working?
- How will I know it is failing?
- What is the blast radius?
- Can I disable, roll back, or roll forward quickly?
- Are migrations, background jobs, caches, queues, and clients compatible?
- Who is watching the deployment?
- What is the first action if the main metric moves the wrong way?

## When Debugging

- What changed recently?
- Can I reproduce it?
- What are the top five plausible causes?
- What evidence would distinguish them?
- What logs, metrics, traces, or tests can I add safely?
- Is this data-specific, environment-specific, timing-specific, or user-specific?
- What is the minimal fix?
- What would prevent this class of bug from returning?

## Useful Sources

- Google Engineering Practices: [Small CLs](https://google.github.io/eng-practices/review/developer/small-cls.html), [What to look for in a code review](https://google.github.io/eng-practices/review/reviewer/looking-for.html), and [The standard of code review](https://google.github.io/eng-practices/review/reviewer/standard.html).
- Google: [Software Engineering at Google, Chapter 9: Code Review](https://abseil.io/resources/swe-book/html/ch09.html).
- Martin Fowler: [Opportunistic Refactoring](https://martinfowler.com/bliki/OpportunisticRefactoring.html), [Technical Debt Quadrant](https://martinfowler.com/bliki/TechnicalDebtQuadrant.html), and [Refactoring](https://martinfowler.com/books/refactoring.html).
- DORA: [Software delivery performance metrics](https://dora.dev/guides/dora-metrics/).
- Google SRE: [Implementing SLOs](https://sre.google/workbook/implementing-slos/), [Alerting on SLOs](https://sre.google/workbook/alerting-on-slos/), and [Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/).
- Conventional Commits: [Specification v1.0.0](https://www.conventionalcommits.org/en/v1.0.0/).
- Atlassian: [The written unwritten guide to pull requests](https://www.atlassian.com/blog/git/written-unwritten-guide-pull-requests).
- Artsy Engineering: [Strategies For Small, Focused Pull Requests](https://artsy.github.io/blog/2021/03/09/strategies-for-small-focused-pull-requests/).
