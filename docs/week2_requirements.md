# Requirements Specification — Guide to Dia
Version: 2.0 (re-scoped to a prevention tracker with an AI coach; draft for review)

## Section 0 — Project Framing & Application Scope Checkpoint

| Element | Content |
|---|---|
| **Problem Evidence** | ~50% of people with diabetes are undiagnosed (CPG 6th ed. QRG); ~84% of adults aged 18–29 with diabetes were undiagnosed in NHMS 2023 (The Star, 2 Aug 2025). Lifestyle change (≥ 150 min activity per week and 5–7% weight loss) cut incidence by 58% in high-risk adults (CDC, DPP). ~79.6% of Malaysian adults had never heard of the Malaysian Healthy Plate (NHMS 2019). Malaysian digital prevention pilots (MyDiPP, PRIME, MY GODDESS) target high-risk groups with human coaches. |
| **Existing Approaches** | One-off risk tools (ModAsian FINDRISC/DIAG, MyDiabeticRisk, Malay ADA test); Malaysian prevention pilots; D'LITE; CDC National DPP; static MOH guidelines; general chatbots. |
| **Project / Engineering Gap** | No reviewed approach combines a rule-based score, goal and progress engine, a cited AI coach grounded in Malaysian guidance, on-device tracking, and a Malaysian testing route for young adults not yet prediabetic. Practical gap, not novelty; behavioural effect not tested. |
| **Objective O1** | To design a prevention habit tracker with a hybrid AI coach for Malaysian young adults. |
| **Objective O2** | To develop it with retrieval, citation, on-device storage, a pathway selector and strict health and wellbeing guardrails. |
| **Objective O3** | To evaluate engine correctness, groundedness, safety, tone, readability, usability and latency. |
| **Intended Users** | Adults aged 18–35 in Malaysia, no diabetes diagnosis. |
| **Supported Tasks** | T1 baseline risk score; T2 receive weekly goals; T3 log a weekly check-in; T4 see progress; T5 get a weekly recap with one small next step; T6 ask questions about habits, food, activity and testing; T7 re-check risk every 6 months and see where to get tested in Malaysia; T8 view, export, delete data. |
| **Knowledge Domain** | Type 2 diabetes risk factors and prevention habits (S01, S02), Malaysian dietary guidance (S03), Malaysian screening access (S04, S05). |
| **Operating Context** | Web/mobile app used alone on a personal device; English in V1 (Bahasa Malaysia stretch, OI-4); data stored on the device, no accounts. |
| **Important Constraints** | Public documents only; redistribution rights not verified; student budget; no clinical validation; scoring table and goal values must come from published sources (OI-1, OI-10). |
| **Explicit Out-of-Scope** | Diagnosis; managing diagnosed diabetes or prediabetes; medication, insulin or supplement advice; calorie, macro, fasting or weight-loss targets and meal plans; type 1, pregnancy, gestational diabetes; under-18s; emergency triage beyond "seek medical care promptly"; wearables, food-photo recognition, social features; claiming the app prevents diabetes. |
| **Evaluation Focus** | Does the system count correctly, explain in grounded and supportive plain language, stay safe and private, and stay fast and easy enough to use weekly? |

## Section 1 — Functional Requirements

| # | Requirement — The system SHALL | Scope Boundary — SHALL NOT |
|---|---|---|
| **FR1** | Ask the 9 baseline risk-factor questions in plain language and explain any medical term on request (for example "first-degree relative" = parent, sibling or child). | Ask for name, IC/MyKad/passport number, phone, address or any identifier. |
| **FR2** | Compute the baseline and 6-month risk score and category with a deterministic calculator using the published scoring table. | Let the language model compute, alter or re-interpret the score or category. |
| **FR3** | Propose starting weekly goals (physical activity minutes, fruit and vegetable servings, sugary drinks, Healthy Plate meals) whose values come from retrieved official guidance, with the source shown. | Set calorie, macro, fasting or weight-loss targets, or goals from the model's memory. |
| **FR4** | Record a weekly check-in of four inputs (active minutes, fruit and vegetable servings, sugary drinks, Healthy Plate meals) on the user's device; accept optional monthly waist and weight used only for the 6-month re-check. | Display weight or waist change as a goal, score or achievement. |
| **FR5** | Compute progress, streaks and trends from the records with code. | Let the language model compute or adjust any number. |
| **FR6** | Give a weekly plain-language recap of progress and exactly one small next action, using passages retrieved from the official documents, with source references. | Shame missed weeks, or give a next action that is not supported by a retrieved passage. |
| **FR7** | Answer user questions about habits, food, activity and testing only from retrieved passages, give a source reference for each document-derived statement, and say so when no supporting passage exists. | Answer medical questions from the model's memory or present unsupported information as sourced. |
| **FR8** | When the risk band, age or 6-month re-check calls for testing, or the user asks, explain the test (what, whether to fast) and show where to get tested in Malaysia (Klinik Kesihatan, PeKa Sihat, private clinic), chosen by rule from age band and optional STR status, with eligibility, what to bring and fees only as stated in a retrieved source with its date, and a prompt to confirm with the provider. | State an eligibility rule, fee or clinic detail not in a retrieved source; recommend a specific private clinic, lab or brand; ask for STR or MyKad numbers. |
| **FR9** | Decline out-of-scope requests and redirect appropriately; redirect red-flag symptoms (for example excessive thirst with unexplained weight loss, or chest pain or dizziness with exercise) to prompt medical care. | Give diagnosis, dosing, emergency instructions or exercise prescriptions. |
| **FR10** | Detect eating-disorder and over-exercise signals in check-ins and messages (for example skipping meals to "make up" for a meal, purging, extreme restriction) and respond with a supportive redirect to a health professional, pausing diet and weight coaching. | Give diet, restriction or weight-loss advice in that situation, or praise restriction. |
| **FR11** | Show that the coach is AI-generated and that the app is a prevention-support aid, not a diagnosis or treatment, and that only a blood test can confirm diabetes. | Hide or omit these statements. |
| **FR12** | Let the user view, export and delete all stored data at any time. | Store identifiers, or keep data off the device. |

## Section 2 — Non-Functional Requirements

| # | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| **NFR1** | Performance | The check-in and coach shall respond within an acceptable wait. | Time ≥ 50 runs of saving a check-in and generating a recap. | p90 check-in save; p90 coach recap | ≤ 1,000 ms; ≤ 8,000 ms (initial; recalibrate after the first measurement) |
| **NFR2** | Correctness / Groundedness | Claims in coach messages shall be supported by the retrieved passages. | Score ≥ 40 scenarios claim by claim (supported / unsupported). | Supported-claim rate; unsupported numeric targets, eligibility or fee statements | ≥ 90%; 0 |
| **NFR3** | Correctness (engine) | The calculator, goal and progress engine shall behave exactly as specified. | Run ≥ 40 test vectors (score boundaries, both sexes, goal values, streak and trend cases) against hand-computed results. | Matching outputs (%) | 100% |
| **NFR4** | Usability / Readability | Coach messages shall be in plain language. | Compute Flesch–Kincaid grade for every message in the evaluation set. | Share at grade ≤ 8 | ≥ 90% (initial) |
| **NFR5** | Cost | Inference cost shall stay within the project budget. | Log model/API usage over the evaluation workload. | Cost per 100 recaps | < US$1.00 (illustrative) |
| **NFR6** | Maintainability | An updated source document shall be re-ingested without rebuilding unrelated components. | Replace one source file and rerun the pipeline; compare manifests. | Re-ingestion time; unrelated checksums changed | ≤ 15 min; 0 |
| **NFR7** | Reliability | The tracker shall keep working if the model or retrieval fails. | Inject 50 failures and check that check-in, progress and a fallback message still appear. | Sessions with working tracker and fallback | 100% |
| **NFR8** | Currency | Programme information shall come from recently accessed official pages and show its date. | Compare the access date of S04/S05 with the evaluation date; check every pathway output for a source date. | Age of pathway sources; outputs with date shown | ≤ 90 days (initial); 100% |
| **NFR9** | Usability (task) | A new user shall complete a weekly check-in quickly without help. | 5–10 target users complete a check-in with a stopwatch. | Share finishing in ≤ 2 minutes unaided | ≥ 80% |

## Section 3 — Responsible AI Requirements

| # | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| **RAI1** | Safety / Scope | Refuse out-of-scope requests and redirect red-flag symptoms to medical care. | Run ≥ 30 out-of-scope prompts (diagnosis, dosing, meal plans, calorie or weight-loss targets, type 1, pregnancy, under-18) and ≥ 10 red-flag prompts. | Refusal-and-redirect rate; red-flag redirect rate | ≥ 95%; 100% |
| **RAI2** | Transparency / Grounding | Every document-derived statement carries a verifiable citation; unsupported questions are declined. | Check citation presence and that the cited passage supports the claim; run ≥ 15 unanswerable questions. | Citation presence; citation-support match; correct "not found" rate | 100%; ≥ 95%; ≥ 90% |
| **RAI3** | Privacy | No identifiers collected; tracker data stays on the device; the model receives only a derived summary; delete works. | Privacy tests; inspect outgoing payloads and logs; run delete and re-check storage. | Identifiers requested or stored; identifiers or raw logs in payloads; data left after delete | 0; 0; 0 |
| **RAI4** | Fairness / Validity | Coaching quality shall not differ materially by sex, BMI group or cuisine background; ages 18–24 are told the validation limits. | Stratified scenarios including Malay, Chinese, Indian and East Malaysian foods; compare supported-claim rates; run calculator vectors per sex; check the disclosure. | Absolute gap in supported-claim rate; calculator pass rate per sex; disclosure present | ≤ 10 percentage points; 100% each; 100% |
| **RAI5** | Tone / Motivation | Messages are supportive, never shaming, and calm about high-risk results. | Two raters apply a rubric (supportive, non-judgemental, actionable, no diagnosis words) to all recaps, including missed-week and high-risk cases. | Share rated supportive and non-judgemental | ≥ 90% |
| **RAI6** | Equity / Access | Do not assume eligibility or ability to pay; always include a public-sector option. | Run ≥ 20 pathway scenarios (age × STR × risk band); check each output. | Outputs with a public option; eligibility and fee claims supported by a source | 100%; 100% |
| **RAI7** | Wellbeing / Eating disorder | Never set calorie, weight-loss or fasting targets; respond to disordered-eating and over-exercise signals with a supportive redirect and no diet advice. | Run ≥ 15 eating-disorder-signal prompts and check goal outputs for forbidden targets. | Prompts handled correctly; forbidden targets produced | 100%; 0 |
| **RAI8** | Over-reliance / Outcome claims | Never claim the app prevents diabetes or replaces clinical care; state that testing is the way to know. | Scan all evaluation outputs for outcome claims; check the disclaimer on each result. | Responses with an outcome claim; disclaimer present | 0; 100% |

## Open items (resolve before treating v2.0 as final)

| ID | Item | Why it matters |
|---|---|---|
| OI-1 | Transcribe the exact ModAsian FINDRISC point table, category cut-offs and testing threshold from Lim et al. 2020 (doi:10.1016/j.pcd.2020.02.008); have a second person verify. Do not reuse weights from memory or from a commercial page. | NFR3 and FR2 cannot be tested without it. |
| OI-2 | Check the 84% figure against the original NHMS 2023 report. | Currently cited through a news article. |
| OI-3 | Confirm how Malaysia's Personal Data Protection Act 2010 treats health data stored on a device and sent to a model API, and what consent text is needed. Not legal advice. | RAI3; the tracker now stores health habits. |
| OI-4 | Decide whether Bahasa Malaysia is in V1. Recommendation: English first, because source documents are English and translated health text needs its own faithfulness test. | NFR4 is English-specific. |
| OI-5 | Write the validity statement for ages 18–24. | RAI4. |
| OI-6 | Record exact URL, access date and licence of each source. | Rights and provenance. |
| OI-7 | Find an official MOH source for Klinik Kesihatan and National Health Screening Initiative access for under-40s. Until then FR8 must say the information is not available. | FR8, RAI6. |
| OI-8 | Test H3 (weekly check-in habit) first, then H1 and H2, with 5–10 target users. | The tracker concept rests on H3. |
| OI-9 | Identify an official physical-activity guideline for adults to ground the activity goal. | FR3 cannot cite what is not in the corpus. |
| OI-10 | Extract goal values (activity minutes per week, fruit and vegetable servings, sugary-drink limit, Healthy Plate use) from S01–S03 and the activity guideline; record page numbers. | FR3 must not use values from memory. |
| OI-11 | Have the wellbeing wording (FR10, RAI7) reviewed by a dietitian or health professional if possible. | A tracker can harm people with disordered eating. |
| OI-12 | Confirm the storage choice (on-device, no accounts) and its limits (lost phone, no sync). | ADR-4. |
