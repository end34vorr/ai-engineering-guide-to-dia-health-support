# Architecture Decisions & Traceability — Guide to Dia

Diagrams: simple view `docs/week2_architecture_v1_simple.png`; detailed view `docs/week2_architecture_v1.png` (source `week2_architecture_v1.dot`). Components are labelled by function; technologies are chosen in later topics.

## Starting point (Step C1)
Simplest flow: **User → Application → AI capability → Response.** A bare language model fails several requirements: it cannot guarantee numbers (FR2, FR5, NFR3), cannot cite (FR7, RAI2), cannot remember progress privately (FR4, FR12), and has no scope or wellbeing control (FR9, FR10). Components were added only to close those gaps.

## Architecture needs from requirements (Step C2)

| Requirement ID | What the requirement needs | Architecture response |
|---|---|---|
| FR2 / NFR3 | The score must match the published table exactly. | Rule-based Risk Calculator, separate from the language model. |
| FR3, FR5 / NFR3 | Goals and progress must be correct and sourced. | Goal & Progress Engine (rules); goal values retrieved from cited documents; the model only explains. |
| FR4, FR12 / RAI3 | Store check-ins privately; export and delete. | On-device Tracker Store; no accounts. |
| FR6, FR7 / NFR2 / RAI2 | Recaps and answers grounded in, and cited to, official documents. | Knowledge Source + Data Preparation + Knowledge Access (retrieval with source metadata and dates). |
| FR7 | Decline when there is no supporting evidence. | Knowledge Access returns "no passage"; AI Coach must fall back. |
| FR8 / RAI6 / NFR8 | Malaysian testing route by age and optional STR status, dated and cited. | Malaysian Pathway Selector (rules) feeding Knowledge Access; S04/S05 with access dates. |
| FR9, FR10 / RAI1, RAI7 | Refuse out-of-scope; redirect red flags; handle eating-disorder signals. | Input Scope / Safety Control before the engines; wellbeing check on output too. |
| RAI1, RAI2, RAI5, RAI7, RAI8 | No diagnosis, diet plans or outcome claims; citations present and supporting; supportive tone. | Output Safety / Citation Control after the AI Coach. |
| RAI3 | The model must not receive raw logs or identifiers. | Only a derived summary passes from the engine to retrieval and the coach. |
| NFR1 / NFR7 | Meet latency; survive failures. | Tracker and progress work without the model; fallback message on failure. |
| Evaluation (Part B) | Evidence for each criterion. | Evaluation Point receiving logs and latency (no identifiers). |

## Architecture Decision Records

| Decision | Requirement / Constraint | Alternatives Considered | Selected Option | Trade-off Accepted |
|---|---|---|---|---|
| **ADR-1** Score, goals and progress computed by code, not by the model | FR2, FR3, FR5, NFR3 (100% correct) | LLM computes; ML model; rule-based engine | Rule-based engines; the model receives results read-only | Limited to the published questionnaire and guideline goals; no personalisation beyond them. |
| **ADR-2** Retrieval with source metadata for all explanations and answers | FR6, FR7, NFR2, RAI2 | Base model only; static FAQ; retrieval over prepared documents | Retrieval with source ID, URL, page and date | More complexity and latency; depends on document quality (audited in Topic 4). |
| **ADR-3** Explicit output control after generation | RAI1, RAI2, RAI5, RAI7, RAI8 | Prompt instructions only; explicit post-generation check | Output Safety / Citation Control with one retry then fallback text | Possible false rejections and extra latency (NFR1). |
| **ADR-4** On-device storage, no accounts (replaces the earlier session-only design) | FR4, FR12, RAI3 | Server accounts with history; session-only (no tracking); on-device | On-device store with export and delete | No sync across devices; data lost if the device is lost; simpler privacy story. |
| **ADR-5** Pathway chosen by rules; details retrieved from dated official pages | FR8, RAI6, NFR8 | LLM decides the route; hard-code eligibility and fees; rules plus retrieval | Rules choose the pathway; retrieval supplies cited details | Rules must change when programmes change (PeKa B40 was renamed PeKa Sihat). |
| **ADR-6** Only a derived summary reaches the model | RAI3 | Send raw check-in logs; send a derived summary | Derived summary (for example "activity 90 of 150 min") | Coach has less context; fewer privacy risks. |
| **ADR-7** No calorie, weight-loss or fasting targets; wellbeing check at input and output | FR3, FR10, RAI7 | Personalised weight and diet coaching; guideline-level habit goals only | Habit goals only, plus eating-disorder signal handling | Less "personal" coaching; avoids reinforcing disordered eating. |

## Initial Requirements Traceability Matrix

| Objective | Requirement | Evaluation Criterion | Architecture Element | Later Evidence |
|---|---|---|---|---|
| O1 | FR2, FR3, FR5 / NFR3 | Engine correctness (100% on ≥ 40 vectors) | Risk Calculator; Goal & Progress Engine | Unit-test report in `evaluation/` |
| O1, O3 | FR6, FR7 / NFR2 | Groundedness (≥ 90%, 0 unsupported targets) | Knowledge Access + AI Coach | Claim-level scoring sheet |
| O2 | FR7 / RAI2 | Citation presence 100%, support ≥ 95% | Prepared chunks with metadata; Output Citation Control | Citation audit table |
| O2 | FR9, FR10 / RAI1, RAI7 | Refusal ≥ 95%; red flag 100%; eating-disorder prompts 100% | Input Scope / Safety Control; Output Control | Safety test-set results |
| O2 | FR12 / RAI3 | 0 identifiers or raw logs sent or stored; delete 100% | On-device Store; derived-summary rule | Payload and storage inspection |
| O2 | FR6 / RAI5 | Supportive tone ≥ 90% | AI Coach; Output Control | Two-rater rubric sheet |
| O1, O2 | FR8 / RAI6 / NFR8 | Pathway 100%; claims supported 100%; source age ≤ 90 days | Pathway Selector; Knowledge Access; S04/S05 | Scenario table; source-date check |
| O1 | FR4 / NFR9 | ≥ 80% finish a check-in in ≤ 2 min | Application Interface | Usability check (H3) |
| O3 | NFR1 | p90 save ≤ 1 s; recap ≤ 8 s | Whole pipeline; Evaluation Point | Latency log |

## Final review checklist
- **Does each component support a requirement or evaluation need?** Yes; each node in the detailed diagram carries its requirement IDs.
- **Is anything important missing?** Bahasa Malaysia is deferred (OI-4); the scoring table, goal values and an official activity guideline are not yet sourced (OI-1, OI-9, OI-10); no official under-40 screening source yet (OI-7).
- **Any unjustified technology?** None named at V1.
- **Is there a simpler architecture that still meets the requirements?** Removing the engines would let the model do arithmetic (breaks NFR3); removing the output control would remove the main defence for RAI1, RAI2, RAI5, RAI7 and RAI8; removing retrieval would break FR7. A server-side store would be simpler to sync but breaks the privacy goal (ADR-4).
