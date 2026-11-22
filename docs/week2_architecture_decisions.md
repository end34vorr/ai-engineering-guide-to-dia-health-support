# Architecture Decisions & Traceability — Guide to Dia

Diagram: `docs/week2_architecture_v1.png` (source: `week2_architecture_v1.dot`). Components are labelled by function; technologies are chosen in later topics.

## Starting point (Step C1)
Simplest flow: **User → Application → AI capability → Response.** A bare language model fails several requirements: it cannot guarantee the score (FR2/NFR3), cannot cite (FR4/RAI2), and has no scope control (FR7/RAI1). Components were added only to close those gaps.

## Architecture needs from requirements (Step C2)

| Requirement ID | What the requirement needs | Architecture response |
|---|---|---|
| FR2 / NFR3 | The score must match the published table exactly. | Rule-based Risk Calculator, separate from the language model. |
| FR3 / FR4 / NFR2 / RAI2 | Explanations grounded in, and cited to, official documents. | Knowledge Source + Data Preparation + Knowledge Access (retrieval with source metadata). |
| FR6 | Decline when there is no supporting evidence. | Knowledge Access returns "no passage"; Language Model Service must fall back. |
| FR7 / RAI1 | Refuse out-of-scope requests; redirect red-flag symptoms. | Input Scope / Safety Control before the calculator. |
| RAI1 / RAI2 / RAI5 | No diagnosis words, citations present and supporting, calm tone. | Output Safety / Citation Control after the model. |
| RAI3 | No identifiers; no retained answers. | Identifier blocking in Input Control; session-only handling; only derived features passed to the model. |
| NFR1 / NFR7 | Meet latency; survive failures. | Calculator result shown independently of the model path; fallback text on model/retrieval failure. |
| Evaluation (Part B) | Evidence for each criterion. | Evaluation Point receiving logs and latency (no identifiers). |

## Architecture Decision Records

| Decision | Requirement / Constraint | Alternatives Considered | Selected Option | Trade-off Accepted |
|---|---|---|---|---|
| **ADR-1** Separate deterministic calculator from the language model | FR2, NFR3 (100% match to published table) | LLM computes the score; trained ML model on a public dataset; rule-based calculator | Rule-based calculator; model receives the score read-only | Limited to the published questionnaire and its categories; no personalisation beyond it. |
| **ADR-2** Include knowledge access with source metadata | FR3, FR4, FR6, NFR2, RAI2 | Base model only; static FAQ text; retrieval over prepared documents | Retrieval over prepared official documents with source ID, URL and page | More complexity and latency; depends on document quality (audited in Topic 4). |
| **ADR-3** Explicit output control after generation | RAI1, RAI2, RAI5 | Prompt instructions only; explicit post-generation check | Explicit output safety / citation control with one retry then fallback text | Possible false rejections and extra latency (NFR1). |
| **ADR-4** Session-only handling, no accounts | RAI3, PDPA open item OI-3 | Accounts with history; session-only | Session-only | Cannot show progress over time or re-contact users; simpler privacy story. |

## Initial Requirements Traceability Matrix

| Objective | Requirement | Evaluation Criterion | Architecture Element | Later Evidence |
|---|---|---|---|---|
| O1 | FR2 / NFR3 | Calculator correctness (100% on ≥ 30 vectors) | Rule-based Risk Calculator | Unit-test report in `evaluation/` |
| O1, O3 | FR3 / NFR2 | Explanation groundedness (≥ 90% supported claims) | Knowledge Access + Language Model Service | Claim-level scoring sheet |
| O2 | FR4 / RAI2 | Citation presence 100%, support match ≥ 95% | Prepared chunks with source metadata; Output Citation Control | Citation audit table |
| O2 | FR7 / RAI1 | Refusal ≥ 95%; red-flag redirect 100% | Input Scope / Safety Control; Output Control | Safety test-set results |
| O3 | NFR1 | p90 latency ≤ 8 s | Whole pipeline; Evaluation Point | Latency log |
| O1 | RAI3 | 0 identifiers stored or sent | Input Control; session-only design | Payload and log inspection |

## Final review checklist
- **Does each component support a requirement or evaluation need?** Yes — each node in the diagram carries its requirement IDs.
- **Is anything important missing?** Language handling (Bahasa Malaysia) is deferred (OI-4); the scoring table is not yet transcribed (OI-1).
- **Any unjustified technology?** None named at V1.
- **Is there a simpler architecture that still meets the requirements?** Dropping the output control would remove the main defence for RAI1/RAI2/RAI5, so it is retained; dropping retrieval would break FR4.
