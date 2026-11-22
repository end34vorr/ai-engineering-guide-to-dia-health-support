# Guide to Dia — A Diabetes-Prevention Habit Tracker with a Grounded AI Coach

Guide to Dia helps young adults in Malaysia build habits linked to lower type 2 diabetes risk. It starts with a rule-based risk score, sets small weekly goals from Malaysian and WHO guidance, tracks a two-minute weekly check-in on the user's own device and uses an AI coach that explains progress, answers questions and suggests one small next step, with every statement cited to an official document. Progress is always counted by code, never by the AI. It is a prevention-support tool. It does not diagnose diabetes, manage a diagnosed condition, set calorie or weight-loss targets or claim that it prevents diabetes.

## Problem Statement
Young adults in Malaysia can already get a one-off diabetes risk score online, but a score does not change habits and habits are what prevention depends on. Malaysia has national guidance for those habits such as the Malaysian Healthy Plate "Suku-Suku Separuh", yet in NHMS 2019 about 80% of adults had never heard of it. The digital prevention programmes we found in Malaysia are research pilots for people who are already high-risk or prediabetic and they rely on human coaches. A young adult who is not yet prediabetic has no trusted, everyday way to turn guideline advice into weekly action, to see their own progress or to ask "is teh tarik really a problem?" and get an answer that cites a Malaysian source. Guide to Dia addresses this issue through a hybrid approach that combines rule-based scoring, goals and progress tracking with AI to provide explanations and answer questions based on reliable information. It also includes safeguards to protect users' health and privacy, as well as guidance on seeking medical testing in Malaysia when necessary based on the user's score or age.

## Evidence of the Problem
*   **High rate of undiagnosed diabetes among young adults.**
    The Ministry of Health Clinical Practice Guidelines 2020 state that about 50 percent of people with diabetes are undiagnosed. A news report by The Star in August 2025 quoted the Ministry of Health stating that about 84 percent of adults aged 18 to 29 with diabetes were undiagnosed. This shows an urgent need for better awareness among young adults.

*   **Lifestyle changes prevent diabetes, but only for high-risk groups.**
    A study by the CDC and NIH found that high-risk adults who exercised 150 minutes a week and lost 5 to 7 percent of their weight had a 58 percent lower chance of getting type 2 diabetes. Because this evidence applies to high-risk people, this project focuses on building healthy habits rather than claiming to cure or prevent the disease.

*   **Low public awareness of national dietary guidelines.**
    The National Health and Morbidity Survey 2019 found that 79.6 percent of Malaysian adults had never heard of the Malaysian Healthy Plate, also known as Suku Suku Separuh. 

*   **Existing Malaysian digital health programs are limited.**
    Programs like MyDiPP, PRIME, and MY GODDESS are mainly research studies. They target high-risk individuals and rely on human coaches. A recent study also found very little evidence for digital diabetes therapy outside developed countries. Currently, there is no public Malaysian app using AI to help young adults prevent diabetes.

*   **Existing risk tools do not provide local Malaysian next steps.**
    The ModAsian FINDRISC calculator on the DIAG website asks for personal contact details and recommends blood tests in Vietnam. Additionally, the free PeKa Sihat screening is only for Sumbangan Tunai Rahmah recipients and their spouses aged 40 and above, excluding most young adults aged 18 to 35.

*   **User behavior needs validation.**
    Further testing with 5 to 10 target users is required to determine if young adults know where to get tested, their language preferences, and their likelihood of using the app weekly. 

## Initial Related Work / Existing Approaches
*   **One-off risk calculators.**
    Tools like ModAsian FINDRISC, the DIAG web tool, MyDiabeticRisk, and the Malay version of the ADA Diabetes Risk Test are useful. However, they only give a score and basic advice. They lack ongoing tracking, coaching, or follow-up features.

*   **Malaysian digital prevention pilots.**
    Local programs like MyDiPP, PRIME, and MY GODDESS use mobile apps combined with human coaches. Their main limitation is that they only target older, high-risk, or prediabetic groups. They do not address young adults who are not yet at that high-risk stage.

*   **Asian app-based lifestyle programs.**
    A program called D'LITE uses an app with in-app dietitian coaching. The limitation is that it is a clinical trial and is not grounded in official Malaysian health guidance.

*   **International prevention programs.**
    The CDC National Diabetes Prevention Program is a comprehensive year-long coached program. However, it is designed specifically for the United States population and may not align with Malaysian clinical guidelines.

*   **Static government materials.**
    Official resources like the Malaysian Dietary Guidelines, Healthy Plate materials, and the PeKa Sihat program are highly authoritative. However, they are static documents. They do not offer personal progress tracking or interactive follow-up questions.

*   **General-purpose AI chatbots.**
    While AI tools like ChatGPT are flexible, they are unsafe for medical topics. They often hallucinate, quote outdated eligibility rules, invent fake diet advice, and lack guaranteed citations or strict safety controls.

## Engineering Gap / Limitation
Based on the review of existing approaches, no current app combines all the necessary features for young adults in Malaysia. This project fills that specific gap by bringing four elements together:

*   **A transparent scoring and tracking system.** The app uses a simple rule-based calculator to give a risk score and track healthy habits, rather than just giving a one-off result.
*   **An AI coach grounded in official documents.** The AI explains the user's progress and answers questions using cited Malaysian and international medical guidelines.
*   **Strict privacy protection.** The tracking happens entirely on the user's device without collecting any names or contact details.
*   **Local health routes.** The app provides specific guidance on where young adults in Malaysia can go for testing.

All of these features are controlled by strict health-safety guardrails for users who are not yet prediabetic. This is a practical engineering gap. The project focuses on building a safe prototype, but the actual long-term effect on user behaviour is outside the scope of this study.

## Project Objectives
- **O1:** To design a diabetes-prevention habit tracker for Malaysian young adults, with a rule-based risk score, goals and progress engine and a grounded AI coach.
- **O2:** To develop it with retrieval over official documents, source citation, on-device data storage, a Malaysian pathway selector and strict health and wellbeing guardrails.
- **O3:** To evaluate engine correctness, groundedness, safety, supportive tone, readability, usability and response time. Whether users actually reduce their diabetes risk is out of scope.

## Target Users
Young adults aged 18–35 in Malaysia with no prior diabetes diagnosis.

## Application Scope

The application is designed to help young adults build healthy habits. To achieve this, the system supports the following tasks:

*   **Initial Assessment:** Complete a baseline diabetes risk score and view specific locations in Malaysia to get tested.
*   **Weekly Tracking:** Receive small, manageable health goals and log a weekly check-in to track personal progress.
*   **AI Coaching:** Get a simple weekly recap with one clear next step and ask the AI questions about food, physical activity and testing.
*   **Maintenance:** Re-check and update the risk score every 6 months.
*   **Data Privacy Control:** View, export or permanently delete all stored personal data at any time.

## Out of Scope

To ensure user safety and keep the project manageable, the system will **not** support the following:

*   **Medical Diagnosis and Treatment:** The app will not diagnose diabetes, manage existing diabetes or prediabetes, or prescribe any medication, insulin or supplements.
*   **Strict Diet and Weight Plans:** The app will not create personalised meal plans, calculate calories or macros or set strict weight-loss targets. It only gives general healthy eating tips based on official guidelines.
*   **High-Risk Groups:** The app is not designed for type 1 diabetes, pregnancy, gestational diabetes or users under 18 years old.
*   **Medical Emergencies:** The app will not handle emergency situations. It will only advise users to seek immediate medical care.
*   **Advanced Tech Features:** There will be no smartwatch integration, food-photo recognition or social media features in this version.
*   **Prevention Claims:** The app will not claim to prevent diabetes. It only aims to raise awareness and encourage healthy habits.
*   **Language:** Version 1 will be in English only. A Bahasa Malaysia version may be added later if user testing shows it is needed.

## Main Features

*   **Baseline Risk Score:** A simple 9-question calculator based on the ModAsian FINDRISC method to give users their starting risk level.
*   **Weekly Health Goals:** Small, achievable goals based on official guidance, focusing on physical activity, eating vegetables and reducing sugary drinks.
*   **2-Minute Weekly Check-in:** A quick weekly update that is saved privately on the user's own phone.
*   **Progress Tracking:** Visual streaks and trends that are calculated normally to ensure 100 percent accuracy.
*   **AI Health Coach:** An AI assistant that gives a weekly recap, suggests one small next step and answers questions with clear source references.
*   **Honest Answers:** The AI will clearly state when it does not know the answer, rather than making things up.
*   **Testing Guidance:** A prompt for a 6-month re-check, plus a guide on where to get tested in Malaysia such as Klinik Kesihatan or private clinics.
*   **Strict Safety Controls:** Built-in rules to detect red-flag symptoms and eating disorder signals, a strict ban on calorie-counting advice and total user control to export or delete their data.

## Data Sources

The AI knowledge base and project calculations rely on the following official documents:

*   **S01: Ministry of Health Malaysia Clinical Practice Guidelines (Type 2 Diabetes).**
    The 6th edition from December 2020. This is the primary document used to ground the AI's medical explanations and local screening advice.
    Link: https://mems.my/wp-content/uploads/2021/03/CPG-T2DM_6th-Edition-2020_210226.pdf

*   **S02: WHO Diabetes Fact Sheet.**
    A global reference from the World Health Organization. Used to provide general facts about diabetes prevention.
    Link: https://www.who.int/news-room/fact-sheets/detail/diabetes

*   **S03: Malaysian Dietary Guidelines 2020.**
    Official dietary guidance from the Ministry of Health, including the Malaysian Healthy Plate concept. Used for the app's food and habit advice.
    Link: https://hq.moh.gov.my/nutrition/wp-content/uploads/2024/03/latest-01.Buku-MDG-2020_12Mac2024.pdf

*   **Calculator Logic: ModAsian FINDRISC.**
    The scoring table from Lim et al. (2020) is used strictly to build the rule-based calculator. It is not fed into the AI as text.
    DOI: 10.1016/j.pcd.2020.02.008

*   **Background Evidence: CDC National Diabetes Prevention Program.**
    Used only as background research for the problem statement. It is not part of the AI's knowledge base.
    Link: https://www.cdc.gov/diabetes-prevention/programs/what-is-the-national-dpp.html


## Requirements Summary
Full details are in `docs/week2_requirements.md`.

| Type | IDs | Focus |
|------|-----|-------|
| Functional | FR1–FR12 | Baseline form, deterministic score, starting goals, weekly check-in, deterministic progress, coach recap, grounded Q&A, testing pathway, scope and red-flag refusal, wellbeing safeguard, disclaimers, data control |
| Non-functional | NFR1–NFR9 | Latency, groundedness, engine correctness, readability, cost, maintainability, reliability, currency of programme information, usability |
| Responsible AI | RAI1–RAI8 | Safety/scope, grounding, privacy, fairness and validity, supportive tone, equity of access, eating-disorder and over-exercise safety, no outcome claims |

## Evaluation Plan
Full criteria are in `docs/week2_evaluation_criteria.md`. Thresholds are initial values, revisited after the first measurement.

| Criterion | Metric | Threshold |
|-----------|--------|-----------|
| Engine correctness (score, goals, progress, streaks) | Matching expected output on test vectors | 100% |
| Explanation groundedness | Supported-claim rate; unsupported numeric targets, eligibility or fee statements | ≥ 90%; 0 |
| Citation presence and support | Presence; citation–source match | 100%; ≥ 95% |
| Out-of-scope refusal and red-flag redirect | Refusal-and-redirect rate; red-flag redirect rate | ≥ 95%; 100% |
| Eating-disorder and diet-plan safety | Prompts handled with supportive redirect and no diet or weight-loss advice | 100% |
| Supportive, non-judgemental tone | Share of coach messages rated supportive by two raters | ≥ 90% |
| Privacy | Identifiers or raw logs sent to the model or stored off-device; delete works | 0; 100% |
| Plain-language clarity | Share of messages at Flesch–Kincaid grade ≤ 8 | ≥ 90% |
| Usability | Test users completing a weekly check-in in ≤ 2 minutes without help | ≥ 80% |
| Response latency | p90 check-in save; p90 coach recap | ≤ 1,000 ms; ≤ 8,000 ms |

Test sets: ≥ 40 engine vectors, ≥ 40 grounding scenarios (including foods from Malay, Chinese, Indian and East Malaysian cuisines), ≥ 30 out-of-scope and ≥ 10 red-flag prompts, ≥ 15 eating-disorder-signal prompts, ≥ 15 unanswerable questions, ≥ 20 pathway scenarios, ≥ 50 latency runs, 5–10 users for usability.

## V1 Architecture
![Guide to Dia V1 architecture, simple view](docs/week2_architecture_v1_simple.png)

1. **Start:** you answer 9 simple questions and get your risk score from a rule-based calculator (no AI).
2. **Set goals:** small weekly goals come from Malaysian and WHO guidance. The system sets no calorie or weight-loss targets.
3. **Weekly check-in:** four quick numbers, saved on your own device.
4. **Progress:** counted by rules. The AI never does the maths.
5. **AI coach:** explains your progress, answers questions and suggests one small step, using only passages from official documents.
6. **Safety check:** confirms sources are shown, nothing sounds like a diagnosis or diet plan, and the tone is supportive. Then the loop repeats next week; every 6 months you re-check your score and see where to get tested in Malaysia.

The detailed component view, with requirement IDs, is `docs/week2_architecture_v1.png`. Decisions, trade-offs and the traceability matrix are in `docs/week2_architecture_decisions.md`.

## Technology Stack (Provisional)
Specific technologies will be selected in later topics. Current candidates:
- Python
- Streamlit
- Sentence Transformers
- FAISS or ChromaDB
- On-device storage (browser local storage or a local file) for tracker data
- Large Language Model (LLM)
- Retrieval-Augmented Generation (RAG)

## Responsible AI

To ensure the app is safe and ethical, the following rules are strictly applied:

*   All risk scores and progress tracking are calculated using standard code. The AI is only used to explain the results in simple words.
*   The AI will only answer using the provided official documents. It must always show citations and dates and it will honestly say when it does not have enough evidence to answer.
*   The app will never diagnose diabetes, recommend medical treatment or claim to prevent the disease. 
*   There will be no strict calorie counting, weight-loss or fasting targets. If the app detects signs of an eating disorder, it will provide a supportive message and redirect the user, rather than giving diet advice.
*   If a user mentions dangerous symptoms, the app will immediately prompt them to seek medical care.
*   The app will not collect names, phone numbers or IC numbers. All tracking data stays privately on the user's device and can be deleted anytime. Only anonymous summaries are sent to the AI model.
*   The AI will always be encouraging and will never shame users for missing a weekly check-in.
*   The app will always include free options like Klinik Kesihatan and will not assume the user can pay for private clinics.
*   The app will clearly state that the risk tools are less validated for users aged 18 to 24 and it will use accurate thresholds for different sexes.
*   All document versions, links and access dates will be recorded in the system and checked regularly to ensure the medical advice is not outdated.

## Documentation
| File | Description |
|------|-------------|
| `docs/week1_project_scope.md` | Week 1 project scope |
| `docs/week1_observation_sheet.md` | Week 1 group observation sheet (to add) |
| `docs/week1_system_map.png` | Week 1 group system map (to add) |
| `docs/week2_requirements.md` | Framing, scope, functional, non-functional and Responsible AI requirements, open items |
| `docs/week2_evaluation_criteria.md` | Evaluation criteria table |
| `docs/week2_architecture_decisions.md` | Architecture needs, ADRs, traceability matrix |
| `docs/week2_architecture_v1_simple.png` | V1 architecture, simple view |
| `docs/week2_architecture_v1.png` | V1 architecture, detailed component view |
| `docs/week3_data_inventory.md` | Data sources, rights, versions, inspection evidence |
| `docs/week3_pipeline_decisions.md` | Part B comparison, configuration, stage contracts, ETL/batch decisions |
| `docs/week3_pipeline_diagram.png` | Executed V1 data pipeline with two failure points |
| `notebooks/week3_pipeline.ipynb` | Runnable V1 data pipeline notebook (PDF pathway) and Week 4 audit cells |
| `data/week3_manifest.json` | Part B lineage manifest (generated by the notebook) |
| `data/pipeline_v1/documents/pipeline_manifest.json` | Part C pipeline run manifest (generated by the notebook) |
| `docs/week4_data_quality_report.md` | Data quality and bias report with quality-gate decision |

## Weekly Progress
| Week | Focus | Deliverable | Status | Link |
|------|-------|-------------|--------|------|
| W1 | Project scoping and GitHub setup | README and project scope | Scope complete; group observation sheet and system map to add | [README](README.md)<br>[Project scope](docs/week1_project_scope.md)<br>[Observation sheet](docs/week1_observation_sheet.md) (to add)<br>[System map](docs/week1_system_map.png) (to add)<br>[DIAG ModAsian FINDRISC tool](https://diag.vn/en/risk-prediction-for-10-year-type-2-diabetes/)<br>[CDC National DPP](https://www.cdc.gov/diabetes-prevention/programs/what-is-the-national-dpp.html)<br>[Healthy Plate awareness, NHMS 2019](https://pmc.ncbi.nlm.nih.gov/articles/PMC10351725)<br>[MyDiPP trial (Terengganu)](https://clinicaltrials.gov/study/NCT03997656)<br>[PRIME (Monash Malaysia)](https://openjournals.library.sydney.edu.au/DHW/article/view/16092)<br>[D'LITE app trial](https://www.frontiersin.org/journals/nutrition/articles/10.3389/fnut.2021.780567/full)<br>[PeKa B40 eligibility (MyGov)](https://www.malaysia.gov.my/en/categories/aid-welfare-and-assistance/healthcare-aid/peka-b40)<br>[The Star: many unaware (2025)](https://www.thestar.com.my/news/nation/2025/08/02/one-in-six-malaysians-are-diabetic-many-unaware)<br>[Doan et al. 2023, ModAsian FINDRISC](https://dovepress.com/modasian-findrisc-as-a-screening-tool-for-people-with-undiagnosed-type-peer-reviewed-fulltext-article-JMDH)<br>[Malay ADA Diabetes Risk Test](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8887991/) |
| W2 | Requirements, evaluation planning, V1 architecture | Requirements, evaluation criteria, architecture, ADR, traceability | Complete (review open items) | [Requirements](docs/week2_requirements.md)<br>[Evaluation criteria](docs/week2_evaluation_criteria.md)<br>[Architecture decisions](docs/week2_architecture_decisions.md)<br>[Architecture, simple](docs/week2_architecture_v1_simple.png)<br>[Architecture, detailed](docs/week2_architecture_v1.png) |
| W3 | Data inventory and V1 data pipeline | Notebook, inventory, diagram, decisions, manifests | In progress: run on real PDFs and fill results | [Pipeline notebook](notebooks/week3_pipeline.ipynb)<br>[Data inventory](docs/week3_data_inventory.md)<br>[Pipeline decisions](docs/week3_pipeline_decisions.md)<br>[Pipeline diagram](docs/week3_pipeline_diagram.png)<br>[Lineage manifest](data/week3_manifest.json) (generated)<br>[Pipeline manifest](data/pipeline_v1/documents/pipeline_manifest.json) (generated)<br>[WHO diabetes fact sheet](https://www.who.int/news-room/fact-sheets/detail/diabetes)<br>[ProtectHealth PeKa Sihat](https://protecthealth.com.my/peka-b40-eng/)<br>[MyGov PeKa B40](https://www.malaysia.gov.my/en/categories/aid-welfare-and-assistance/healthcare-aid/peka-b40) |
| W4 | Data quality and bias audit | Data quality report | In progress: numbers and decision pending the audit run | [Data quality report](docs/week4_data_quality_report.md)<br>[Audit cells in the notebook](notebooks/week3_pipeline.ipynb)<br>[Spot-check sheet](data/week4_spotcheck_sample.csv) (generated)<br>[Audit summary](data/week4_audit_summary.json) (generated) |
