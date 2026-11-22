# Requirements Specification — Guide to Dia
Version: 1.0 (draft for review)

## Section 0 — Project Framing & Application Scope Checkpoint

| Element | Content |
|---|---|
| **Problem Evidence** | ~50% of people with diabetes are undiagnosed (CPG 6th ed. QRG); ~84% of adults aged 18–29 with diabetes were undiagnosed in NHMS 2023 (The Star, 2 Aug 2025, quoting MOH). Routine screening of asymptomatic adults starts at 30, so under-30s depend on knowing their own risk factors. |
| **Existing Approaches** | (1) MyDiabeticRisk — logistic regression on glucose, blood pressure, BMI, age. (2) ModAsian FINDRISC — self-reported questionnaire with Asian BMI/waist cut-offs, validated in Malaysian primary care (Lim 2020). (3) CDC/ADA Prediabetes Risk Test — 7 questions, US population. (4) General chatbots — flexible but ungrounded. |
| **Project / Engineering Gap** | None combines a transparent calculator with plain-language, cited, safety-controlled explanation. Practical gap (grounding, traceability, safety, usability), not a novelty claim. |
| **Objective O1** | To design a hybrid assistant that computes a rule-based risk score and explains it in simple language using official health documents. |
| **Objective O2** | To develop it with retrieval, source citation and strict medical-safety guardrails. |
| **Objective O3** | To evaluate groundedness, safety, readability and latency. |
| **Intended Users** | Adults aged 18–35 in Malaysia, no prior diabetes diagnosis. |
| **Supported Tasks** | T1 answer plain-language risk-factor questions; T2 get a score and category from the calculator; T3 understand why each factor matters; T4 get one next step; T5 ask what a medical term means. |
| **Knowledge Domain** | Type 2 diabetes risk factors, screening/testing guidance, prevention basics, as stated in S01–S03. |
| **Operating Context** | Web/mobile chat used alone, on a personal device; English in V1; session-only (no accounts). |
| **Important Constraints** | Only public documents; redistribution rights not yet verified; student budget and no clinical validation; scoring table must come from a published source (open item OI-1). |
| **Explicit Out-of-Scope** | Diagnosis; interpreting personal lab values; medication/insulin/supplement/diet-plan advice; type 1 diabetes; pregnancy/gestational diabetes; under-18s; emergency triage beyond "seek medical care promptly"; storing identifiable data. |
| **Evaluation Focus** | Does the system give risk explanations that are understandable, supported by the cited documents, safe (refuses out-of-scope, redirects red flags), and fast enough? |

## Section 1 — Functional Requirements

| # | Requirement — The system SHALL | Scope Boundary — SHALL NOT |
|---|---|---|
| **FR1** | Ask the risk-factor questions in plain language and explain any medical term (for example "first-degree relative" = parent, sibling or child) on request. | Ask for name, IC/passport number, phone, address or any other identifier. |
| **FR2** | Compute the risk score and category from the user's answers with a deterministic calculator that uses the published scoring table. | Let the language model compute, alter or re-interpret the score or category. |
| **FR3** | Explain the result in plain language: which answers contributed to the score and why each factor matters, using passages retrieved from the official documents. | State or imply that the user has, or does not have, diabetes. |
| **FR4** | Give a source reference (document, page or URL) for every statement that comes from the official documents. | Present unsupported information as if it came from an approved source. |
| **FR5** | Give exactly one primary next step that matches the risk category (for example, book a blood glucose test at a clinic, or keep current habits and re-check on a stated schedule). | Recommend medication, supplements, or a personalised diet/exercise prescription. |
| **FR6** | Say that it cannot find the answer in the official documents when retrieval returns no supporting passage. | Answer medical questions from the model's memory. |
| **FR7** | Decline out-of-scope requests and redirect appropriately; redirect red-flag symptoms (for example excessive thirst with unexplained weight loss) to prompt medical care. | Give diagnosis, dosing or emergency instructions. |
| **FR8** | Show with every result that it is a screening aid and not a diagnosis, and that only a blood test can confirm diabetes. | Hide or omit this statement. |

## Section 2 — Non-Functional Requirements

| # | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| **NFR1** | Performance | Responses shall arrive within an acceptable wait. | Time end-to-end on a fixed workload of ≥ 50 scenarios; time the calculator separately. | p90 end-to-end latency; p90 calculator latency | ≤ 8,000 ms; ≤ 1,000 ms (initial; recalibrate after the first measurement) |
| **NFR2** | Correctness / Groundedness | Claims in explanations shall be supported by the retrieved official passages. | Score a curated set of ≥ 40 scenarios with a supported / unsupported rubric per claim. | Supported-claim rate; unsupported numeric thresholds | ≥ 90%; 0 |
| **NFR3** | Correctness (calculator) | The calculator shall reproduce the published scoring. | Run ≥ 30 test vectors (boundary values, both sexes) against hand-computed reference results. | Matching score and category (%) | 100% |
| **NFR4** | Usability / Readability | Explanations shall be in plain language. | Compute Flesch–Kincaid grade for every explanation in the evaluation set. | Share of explanations at grade ≤ 8 | ≥ 90% (initial) |
| **NFR5** | Cost | Inference cost shall stay within the project budget. | Log model/API usage over the evaluation workload. | Cost per 100 sessions | < US$0.50 (illustrative) |
| **NFR6** | Maintainability | An updated source document shall be re-ingested without rebuilding unrelated components. | Replace one source file and rerun the pipeline; compare manifests. | Re-ingestion time; unrelated source checksums changed | ≤ 15 min; 0 |
| **NFR7** | Reliability | The core result shall survive component failure. | Inject 50 failures (retrieval error, model timeout) and check output. | Sessions that still show calculator result + fallback next step | 100% |

## Section 3 — Responsible AI Requirements

| # | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| **RAI1** | Safety / Scope | The system shall refuse out-of-scope requests and redirect red-flag symptoms to medical care. | Run ≥ 30 out-of-scope prompts (diagnosis, dosing, type 1, pregnancy, under-18) and ≥ 10 red-flag prompts. | Refusal-and-redirect rate; red-flag redirect rate | ≥ 95%; 100% |
| **RAI2** | Transparency / Grounding | Every document-derived statement shall carry a verifiable citation, and unsupported questions shall be declined. | Inspect responses for citation presence; check that the cited passage supports the claim; run ≥ 15 unanswerable questions. | Citation presence; citation-support match; correct "not found" rate | 100%; ≥ 95%; ≥ 90% |
| **RAI3** | Privacy | The system shall not collect or retain identifiers and shall send only derived, non-identifying features to any external model API. | Privacy test cases; inspect outgoing payloads and logs; check storage after session end. | Identifiers requested/stored; identifiers in payloads/logs; answers retained after session | 0; 0; 0 |
| **RAI4** | Fairness / Validity | Results and explanation quality shall not differ materially by sex or BMI group, and ages 18–24 shall be told the validation limits. | Stratified test set; compare supported-claim rates; run calculator vectors per sex; check the disclosure. | Absolute gap in supported-claim rate; calculator pass rate per sex; disclosure present | ≤ 10 percentage points; 100% each; 100% |
| **RAI5** | Alarm / Tone | High-risk results shall be explained calmly with a concrete next step. | Two raters apply a short rubric (calm, actionable, no diagnosis words) to all high-risk explanations. | Share rated non-alarming and actionable | ≥ 90% |

## Open items (resolve before treating v1.0 as final)

| ID | Item | Why it matters |
|---|---|---|
| OI-1 | Transcribe the exact ModAsian FINDRISC point table, category cut-offs and the testing threshold from Lim et al. 2020 (doi:10.1016/j.pcd.2020.02.008); have a second person verify. Do **not** reuse weights from memory or from a commercial calculator page. | NFR3 and FR2 cannot be tested without the authoritative table. |
| OI-2 | Check the 84% figure against the original NHMS 2023 report. | It is currently cited through a news article. |
| OI-3 | Confirm how Malaysia's Personal Data Protection Act 2010 treats health answers, and what consent text is needed. | Informs RAI3; this file is not legal advice. |
| OI-4 | Decide whether Bahasa Malaysia is needed in V1. | Affects NFR4 (readability metric is English-specific) and data sources. |
| OI-5 | Write the wording of the validity statement for ages 18–24. | RAI4. |
| OI-6 | Record the exact URL, date accessed and licence of each source document. | Rights and provenance (Topics 3 and 4). |
