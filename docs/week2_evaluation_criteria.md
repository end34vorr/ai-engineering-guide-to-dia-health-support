# Evaluation Criteria — Guide to Dia

Flow: Objective / Requirement → Evaluation Criterion → Measurement Method → Metric → Threshold → User / Business Outcome.
Thresholds are initial values; they are revisited after the first measurement round (see `week2_requirements.md`).

| Objective / Requirement ID | Evaluation Criterion | Capability Bucket | Measurement Method | Metric | Threshold | User / Business Outcome |
|---|---|---|---|---|---|---|
| O1 / FR2 / NFR3 | Calculator correctness | Domain-specific capability | Run ≥ 30 test vectors (boundaries, both sexes) against a hand-computed reference from the published table. | Matching score and category (%) | 100% | Users see a risk result they can trust to follow the published tool. |
| O1, O3 / FR3 / NFR2 | Explanation groundedness | Domain-specific capability | Score ≥ 40 scenario explanations claim-by-claim as supported / unsupported by the retrieved passages. | Supported-claim rate; unsupported numeric thresholds | ≥ 90%; 0 | Users are not given invented medical facts. |
| O2 / FR4 / RAI2 | Citation presence and support | Instruction-following capability | Inspect responses for a citation on each document-derived statement; check the cited page supports it. | Citation presence; citation-support match | 100%; ≥ 95% | Users can verify advice against official documents. |
| O2 / FR7 / RAI1 | Out-of-scope refusal and red-flag redirect | Instruction-following capability | Run ≥ 30 out-of-scope prompts and ≥ 10 red-flag symptom prompts. | Refusal-and-redirect rate; red-flag redirect rate | ≥ 95%; 100% | Users with symptoms are sent to care; the system never plays doctor. |
| O1 / FR3, FR5 / NFR4 | Plain-language clarity | Generation capability | Compute Flesch–Kincaid grade of each explanation; two raters check the one-next-step rule on a 20-item sample. | Share at grade ≤ 8; one-next-step rate | ≥ 90%; 100% | Young adults understand the result and know what to do. |
| O3 / NFR1 | Response latency | Cost and latency | Time end-to-end on ≥ 50 scenarios; time the calculator alone. | p90 end-to-end; p90 calculator | ≤ 8,000 ms; ≤ 1,000 ms | Users do not abandon the session while waiting. |
