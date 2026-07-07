# Sacred Invariants Report: <repo/subsystem>

Date:
Task lens:
Target:
Downstream consumers:
Overall confidence: High / Medium / Low
Stage: Rough reconnaissance / Lead architect synthesis / Final handoff

Use this template as a menu. Omit irrelevant sections. Prefer one sharp boundary over a broad filled form.

## Start Here

- Strongest suspected boundary:
- Required first-pass downstream action:
- Top downstream tasks (ACT-001 required; others follow_up/conditional):
- Safe immediately:
- Blocked on product/security/architecture decision:
- Current-stage proof vs prepared/deferred lifecycle:
- Minimal verification packet:
- Do not implement without approval:

## Action Queue

Use `assets/AGENT_HANDOFF.template.md` when producing the handoff as a separate artifact.

Default to at most three actions. Use exactly one `required_first_pass`; mark adjacent work `follow_up` or `conditional`. Each action must protect the named boundary or unblock its first proof.

| action_id | Boundary / finding | Type | Actionability | Requires decision? | Anchors | Acceptance criteria |
|---|---|---|---|---|---|---|
| ACT-001 |  | audit / contract-test / code-fix / docs / product / observability | required_first_pass / follow_up / conditional / needs_product_decision / observe_only | yes/no |  |  |

## Executive Reconstruction

- Encoded product purpose:
- Architecture center of gravity:
- Highest-confidence invariants:
- Highest-risk unknowns:

## Product And Architecture Model

- Primary users / workflows:
- Jobs to be done:
- Product failure means:
- Domain vocabulary:
- Authority owners:
- Intentional tradeoffs or historical pressure:

## Lead Architect Synthesis

Use `assets/LEAD_ARCHITECT_SYNTHESIS.template.md` when producing this as a separate artifact.

| Rough finding | Lead-architect judgment | Decision right / token / policy / supporting contract | Confidence |
|---|---|---|---|
|  | keep / challenge / discard / owner_question_required |  | High / Medium / Low |

## Sacred Invariants

| invariant_id | Statement | Forbidden condition | Evidence | Confidence | First proof / audit |
|---|---|---|---|---|---|
| SI-001 |  |  | direct_code / test_pinned / doc_claim / runtime_trace / inferred / not_verified | Proven / Likely / Suspected / Unknown |  |

## Authority-Lifecycle Kernel

Fill this for the top one to three boundaries only.

| Field | Answer |
|---|---|
| Decision at stake |  |
| Current owner |  |
| Ownership start |  |
| Ownership token / evidence boundary |  |
| Allowed mutation |  |
| Forbidden neighboring decision |  |
| Transfer / override / removal / expiry |  |
| Uncertainty posture | preserve / relabel / rerank / reconcile / fail open / fail closed / defer visibly / ask / block |
| Supporting contract | namespace / index / ledger / cache / API lookup / marker / migration / observability / ownership table |
| First downstream proof |  |

## Relevant Boundary Patterns

Select only patterns triggered by evidence. Leave the rest out.

| Pattern | Why relevant | Rule or risk | First proof |
|---|---|---|---|
| source/model contamination |  |  |  |
| remote intent/projection gap |  |  |  |
| cleanup/retention ownership |  |  |  |
| marker-based artifact ownership |  |  |  |
| stage-specific authority |  |  |  |
| expensive responsibility |  |  |  |
| phase-scope boundary |  |  |  |

## Questions Or Drift

Use this wording when applicable: "No interactive questions were asked; the following owner questions remain open."

| Priority | Question / drift | Why it matters | Evidence | Default assumption | Risk if unanswered |
|---|---|---|---|---|---|
| P0/P1/P2 |  |  |  |  |  |

## Downstream Probes

| invariant_id | Positive proof | Negative probe | Required real-data shape | Expected outcome | Suggested agent |
|---|---|---|---|---|---|
| SI-001 |  |  |  |  | audit / contract-test / refactor / docs / product / observability |

## Anchor Manifest

| anchor_id | File | Lines | Claim supported | Checked? |
|---|---|---|---|---|
| A-001 |  |  |  | yes/no |
