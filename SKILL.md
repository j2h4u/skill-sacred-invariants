---
name: sacred-invariants
description: "Run a staged brownfield architecture-product investigation: rough codebase reconnaissance, lead-architect synthesis, drift questioning, and downstream handoff. Use when entering an unfamiliar codebase, reconstructing product model and architectural intent, identifying sacred invariants, checking real data boundaries and failure semantics, preparing lower-level agents to find logical bugs, or comparing encoded behavior with product-owner expectations. Produces architecture-product artifacts for agent loops; does not write code, write tests, or perform broad file inventory."
---

# Sacred Invariants

Use this skill only when explicitly asked or when the user clearly wants a deliberate architecture-product investigation of an unfamiliar brownfield codebase.

This is a rare, high-touch skill. It is not a default architecture review. It is for reconstructing the mental model of a very senior architect who has lived with the product since its greenfield phase.

The agent running this skill is an architect and investigator. It does not write code, write tests, refactor, or perform broad file inventory. It produces artifacts for later agent loops that may audit, test, refactor, document, or evolve the product.

Core rule:

> First reconstruct the product encoded in the codebase. Then identify what it must never break. Then interview humans only to fill gaps, expose drift, or validate product-level intent.

Abstraction rule:

> Read implementation details as evidence for product and architecture intent. Do not turn the investigation into implementation review. The architect identifies invariants, boundaries, risks, and downstream probes; lower-level agents inspect exact code paths and write fixes.

Stage rule:

> Treat this as a staged audit pipeline. Stage 1 is rough codebase reconnaissance. Stage 2 is lead-architect synthesis over that reconnaissance. Later stages ask product-level questions, handle drift, and hand off narrowed authority boundaries to downstream agents.

## Runtime Boundaries

- Do not ask the user for product intent before doing codebase-first discovery.
- Do not ask low-level engineering questions when a product/business question would answer the real issue.
- Do not treat user or product-owner narrative as the primary truth source.
- Do not smooth over differences between human narrative and codebase evidence.
- Do not proceed from weak guesses to downstream handoff without confidence labels and assumptions.
- Do not report implementation mechanics unless they prove or challenge a product or architecture claim.
- Do not let source reading collapse into line-by-line code review, local cleanup advice, naming/style critique, or tactical patch design.

## Outputs

When creating standalone artifacts, use `assets/SACRED_INVARIANTS.template.md` as a menu, not a form. Omit irrelevant sections.

Produce only the artifacts needed for the current run:

- `DISCOVERY.md` - rough codebase-first reconnaissance and candidate invariants.
- `LEAD_ARCHITECT_SYNTHESIS.md` - second-pass Staff/Lead Architect synthesis over the rough reconnaissance. Use `assets/LEAD_ARCHITECT_SYNTHESIS.template.md` when producing it separately.
- `QUESTIONS.md` - ranked product-level and architecture-level questions grounded in evidence.
- `DRIFT.md` - gap analysis between human narrative and encoded product, when drift exists.
- `INVARIANTS.md` - final sacred-invariants report.
- `AGENT_HANDOFF.md` - instructions for downstream audit, test, refactor, docs, product, or observability agents.

The final report should be dense, evidence-backed, and structured for agent loops. Humans may read it, but they are not the primary consumer. Prefer a smaller artifact that selects the decisive boundary over a broad report that fills every heading.

`AGENT_HANDOFF.md` is the primary downstream control plane. Most lower-level agents should receive it first; full reports are supporting context, not the default work queue. It must start with a `Start Here` section and an `Action Queue` so later agents do not have to reclassify the full report before acting.

Keep `Action Queue` focused. Default to the top three boundary probes or decisions, with exactly one `required_first_pass`. Mark other actions `follow_up` or `conditional` so they cannot compete with the first proof. Add broader maintenance work only when it protects the named invariant or blocks the first proof.

## Workflow

The default flow is multi-stage:

1. Rough reconnaissance: discover the encoded product and candidate invariants from codebase evidence.
2. Lead Architect synthesis: challenge the rough model, identify the strongest authority-lifecycle decisions, and separate visible mechanisms from missing policies or supporting contracts.
3. Owner questions and drift: ask product-level questions only after codebase-first discovery and synthesis expose material uncertainty or drift.
4. Downstream handoff: package the narrowed authority transfer, forbidden fixes, and first probes for lower-level agents.

Do not skip directly to the handoff unless the investigation is intentionally narrow and confidence is high.

### 1. Scope The Investigation

Name:

- repository or subsystem;
- task lens;
- target behavior, feature, PR, integration, or risk area;
- why this investigation is needed now;
- expected downstream agent types.

If the target is too broad, choose a product or architectural slice rather than mapping the entire repository.

### 2. Rough Codebase-First Reconnaissance

Before interviewing the user, inspect high-signal artifacts:

- root docs, product docs, architecture docs, ADRs, contribution guidance;
- package manifests, config, schemas, migrations, API contracts;
- representative source paths around the target;
- tests, especially regression, integration, and negative-path tests;
- realistic fixtures, logs, traces, serialized records, or runtime envelopes;
- issue/PR discussion and maintainer comments if available;
- recent commits near target symbols and contract words.

Search for contract language:

- `must`, `never`, `preserve`, `stable`, `recover`, `fallback`, `cache`, `schema`, `envelope`, `replay`, `idempotent`, `authoritative`, `trusted`, `append-only`, `intentional`, `last resort`.

Avoid exhaustive file inventory. Read enough to reconstruct the product and architecture intent encoded in the current system.

Stay above tactical implementation detail. Use source files, tests, schemas, and history as evidence, then translate them into product model, ownership boundaries, failure semantics, and invariant candidates. When a finding requires lower-level inspection, hand it off as a probe rather than completing the inspection yourself.

After identifying implementation-level contracts, do one abstraction pass without file names, function names, or local patch mechanics. State the product stages, authority owners, allowed mutations, forbidden mutations, and promotion rules in domain language.

For staged workflows, separate authority types. A stage can own data, evidence, presentation, publication, cleanup, or operator guidance without owning the others. Do not collapse them into one generic "truth" boundary.

Use parallel subagents for non-trivial codebases when available. The orchestrating architect remains responsible for synthesis and should keep subagent scopes disjoint.

Recommended parallel lanes:

- product/docs lane: root docs, product docs, ADRs, issue/PR language, user-facing workflows;
- architecture lane: package boundaries, config, schemas, migrations, runtime entrypoints, integration surfaces;
- contracts/tests lane: regression tests, integration tests, fixtures, failure cases, boundary envelopes;
- history/telemetry lane: commit history around contract words, logs, traces, telemetry, operational defaults.

Each lane should produce a small evidence note, not a broad map:

- sources read;
- candidate intents;
- candidate invariants;
- real data boundaries;
- confidence and evidence quality;
- contradictions or missing evidence;
- questions for the orchestrator.

Do not let subagents write implementation code, tests, or refactors. Do not assign overlapping write targets. Prefer separate temporary artifact paths per lane, then synthesize them into the main discovery and invariant reports.

This stage is intentionally provisional. It should produce evidence, hypotheses, candidate invariants, confidence labels, contradictions, and product-level questions. It should not pretend to be the final Staff Architect judgment.

### 3. Reconstruct The Encoded Product

Infer from evidence:

- what the product is for;
- primary users, workflows, and jobs-to-be-done;
- what outcomes count as product failure;
- what the system optimizes for;
- what the system deliberately does not optimize for;
- domain vocabulary, lifecycle states, ownership, permissions, and data boundaries;
- architectural center of gravity;
- visible tradeoffs and historical pressure.

Separate facts from interpretation. Mark each claim as `Proven`, `Likely`, `Suspected`, or `Unknown`.

### 4. Extract Candidate Invariants

Write invariants as product or architecture contracts, not module descriptions.

For each important invariant include only what downstream agents need: stable ID, statement, forbidden condition, evidence, confidence, risk if broken, and first proof or audit strategy.

Useful invariant types:

- product and domain;
- data shape;
- API contract;
- lifecycle or state machine;
- concurrency or idempotency;
- authorization or security;
- integration boundary;
- failure semantics.

### 5. Find Truth Boundaries

Identify where data changes shape or authority changes hands:

- API payloads;
- tool or provider results;
- transport envelopes;
- gateways and adapters;
- serialized sessions or replay history;
- persisted state;
- caches and queues;
- config;
- plugin or extension interfaces;
- user-visible output.

Compare real runtime shapes to tests and fixtures. A test on simplified data does not prove correctness at a structured boundary.

Select only the boundary lenses triggered by evidence. Do not fill every lens.

- Source/model contamination: raw evidence, canonical state, projections, and user-visible models share storage or refresh paths.
- Remote intent/projection gap: local mutation succeeds before a remote read projection catches up.
- Cleanup/retention ownership: pruning can delete records still owned by a user, workflow, audit trail, or downstream artifact.
- Marker-based artifact ownership: provenance markers prove a later stage owns content that generic refresh paths must preserve.
- Stage-specific authority: data, evidence, presentation, publication, cleanup, and operator guidance are owned by different stages.
- Expensive responsibility: correctness looks costly, so the real question is the supporting contract, not whether to drop the responsibility.
- Phase-scope boundary: the code exposes a primitive for a future lifecycle but current evidence does not prove end-to-end ownership.

For each relevant lens, reduce the finding to the authority-lifecycle kernel: decision right, owner, ownership token, allowed mutation, forbidden neighboring decision, transfer/removal rule, uncertainty posture, supporting contract, and first proof.

### 6. Separate Gates From Benefits

Gate metrics decide whether behavior is correct or safe.

Optimization metrics measure benefit only after gates pass.

Do not treat savings, speed, hit rate, reduced size, cleaner code, or lower complexity as correctness proof.

When a correctness responsibility appears expensive, do not redefine the responsibility away as the first move. Ask what storage, index, API, cache, namespace, ledger, or ownership contract would make the responsibility efficient and observable. Treat deferral, background maintenance, or "do not do this anymore" as a product-model change, not an equivalent technical default.

For every optimization, ask:

- what correctness contract could it violate?
- what failure signal would make the optimization invalid?
- what should happen under uncertainty: fail open, fail closed, return original, reject, retry, degrade, or ask for help?

### 7. Run Lead Architect Synthesis

Use the rough reconnaissance as the primary input. The synthesis pass should feel like a lead architect reviewing an investigation brief, not like another file mapper.

The lead architect should challenge:

- whether the visible mechanism is the real architecture or a substitute for a missing authority layer, product policy, ownership token, or supporting contract;
- whether the rough report found the broad invariant family but missed the exact decision right;
- whether a stage owns data, evidence, presentation, cleanup, publication, or operator guidance without owning neighboring decisions;
- whether a correctness responsibility that looks expensive should be preserved by a supporting contract before being reduced;
- whether the encoded product appears intentional, historical, obsolete, or ambiguous.

Before selecting the first downstream action, run a task-lens priority check. The requested phase, feature, PR, or risk lane remains the first handoff boundary unless an adjacent lifecycle risk directly explains or invalidates that current authority transfer. If the adjacent risk is real but not required to prove the requested boundary, record it as a follow-up, owner question, or deferred probe.

For each top boundary, answer the authority-lifecycle kernel:

- what decision is at stake?
- who or what owns it now?
- when does ownership start?
- what token proves ownership?
- what may be mutated?
- what neighboring decision must not be taken here?
- how is ownership transferred, overridden, removed, or expired?
- what happens under uncertainty?
- what supporting contract makes the responsibility viable?
- what first downstream proof validates the boundary?

For credential-bearing persisted state, include writer or opener lifecycle semantics in the first proof: artifacts created or opened by that path must be repaired, or the path must fail closed when repair is unsafe. Name semantic obligation and proof path, not implementation mechanics.

Run a phase-scope gate before handoff. If the codebase or plan exposes a primitive for a future lifecycle, separate:

- behavior that must be proven in the current stage;
- primitives that should exist now but are not an end-to-end product behavior yet;
- future or deferred lifecycle behavior;
- owner/product decisions required before downstream agents implement more.

Do not promote a prepared primitive, helper, migration, config field, marker, or API affordance into a delivered lifecycle unless current evidence says this stage owns the end-to-end behavior.

When producing `LEAD_ARCHITECT_SYNTHESIS.md`, keep it compact. Its job is to select and sharpen the strategic center of gravity for later stages, not to restate the rough discovery.

### 8. Produce A Question Report

After rough reconnaissance and lead-architect synthesis, stop when material uncertainty remains.

Generate questions only when the answer affects product behavior, architecture, safety, migration, compatibility, or downstream handoff.

Questions should be product/business-level by default. Ask humans about users, workflows, outcomes, constraints, tradeoffs, and unacceptable failures. The agent should derive architectural and technical consequences itself.

For each question include:

- question;
- why it matters;
- evidence that caused it;
- default assumption if unanswered;
- risk of proceeding with that assumption;
- who can answer.

If no interactive questions were asked during the run, say that explicitly as: "No interactive questions were asked; the following owner questions remain open." Do not imply there are no open questions when the artifact contains unresolved items.

### 9. Interview Humans After Discovery

Use the question report to interview the user, maintainer, or product owner.

Start at the product level:

- what is the product for?
- which user jobs matter most?
- what outcomes are unacceptable?
- what tradeoffs are intentional?
- what would be a wrong-premise fix?

Only descend into architecture or technology when product-level language cannot resolve the issue.

### 10. Produce Drift Analysis When Needed

If human narrative conflicts with codebase evidence, produce a drift artifact before final handoff.

Distinguish:

- code-preserved behavior;
- user-claimed or owner-claimed intent;
- stale docs;
- stale tests;
- stale human model;
- likely stale code;
- true ambiguity requiring owner confirmation;
- product evolution opportunity.

Offer an optional drift-reduction loop:

- align docs or human model to the code;
- ask for owner clarification;
- prepare downstream tasks to align code with intended behavior;
- proceed with explicit assumptions when owner access is unavailable.

The architect agent still does not write code or tests.

### 11. Finalize The Pyramid Report

Use this compact order unless the task explicitly needs a broader report:

1. Executive Reconstruction
2. Product And Architecture Model
3. Lead Architect Synthesis
4. Sacred Invariants
5. Relevant Boundary Patterns
6. Questions Or Drift
7. Downstream Probes
8. Agent Handoff

Keep prose tight. Prefer tables and stable IDs where downstream agents will consume the report.

Use consistent labels where possible:

- `evidence_level`: `direct_code`, `test_pinned`, `doc_claim`, `runtime_trace`, `inferred`, `not_verified`.
- `verification_level`: `unit`, `integration`, `full_path`, `runtime`, `none`.
- `actionability`: `required_first_pass`, `follow_up`, `conditional`, `needs_product_decision`, `observe_only`.

For repeated issues, create one canonical finding ID and refer to it from other artifacts instead of restating the issue with slightly different wording.

### 12. Hand Off To Downstream Agents

Use `assets/AGENT_HANDOFF.template.md` when producing `AGENT_HANDOFF.md` as a separate artifact.

Begin `AGENT_HANDOFF.md` with:

- artifact read order;
- one strongest suspected boundary;
- one required first-pass action;
- top downstream tasks, capped to three;
- current-stage proofs versus prepared-but-deferred lifecycle behavior;
- actions blocked on product/security/architecture decisions;
- minimal verification packet;
- do-not-implement-without-approval list.

Then include an `Action Queue` table:

| action_id | Finding | Type | Actionability | Requires decision? | Files / anchors | Acceptance criteria |
|---|---|---|---|---|---|---|

Exactly one action should be marked `required_first_pass`, normally `ACT-001`. Mark adjacent concerns as `follow_up` or `conditional`; if they could distract from the requested boundary, move them to owner questions or deferred probes. Each action should point to invariant or drift IDs when applicable. If an action does not protect the strongest boundary or unblock its first proof, omit it.

For credential-bearing tokens, `required_first_pass` must include writer repair or fail-closed semantics for artifacts created or opened by the selected path.

Also include an `Anchor Manifest`:

| anchor_id | File | Lines | Claim supported | Checked? |
|---|---|---|---|---|

Tailor handoff notes only for downstream agent types actually expected in this run. State what to preserve, what to prove first, and what must not be changed without owner or architecture approval.

## Evidence Standards

Strong claims need at least one evidence source, and critical claims should have multiple independent signals.

Evidence sources include:

- docs or contribution guidance;
- product documentation;
- file/line references;
- public names, types, schemas, config defaults;
- integration or regression tests;
- realistic fixtures or runtime traces;
- logs and telemetry fields;
- setup UX and defaults;
- issue/PR maintainer comments;
- `git log -p -S <symbol-or-contract-word>` around the target.

Treat a constraint as load-bearing when multiple independent signals point to it: docs, tests, public names/types, defaults, fallbacks, history, or cross-surface behavior.

Treat a detail as accidental when it only appears in one implementation branch, has no user-visible meaning, is not reflected in docs/tests/config, and is not tied to a failure mode.

## Anti-Patterns

- File inventory instead of architectural-product reconstruction.
- Interviewing humans before reading the encoded product.
- Asking humans to debug implementation details by default.
- Trusting an optimization metric as correctness evidence.
- Testing purified data when production uses envelopes, adapters, or serialized records.
- Treating absence as a bug without checking intent.
- Snapshot or change-detector tests instead of behavioral contracts.
- Ignoring negative paths because the happy path is green.
- Adding implementation work to this skill.
- Writing strong claims without evidence and confidence labels.
