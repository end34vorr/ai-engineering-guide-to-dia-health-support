\# Guide to Dia



An AI assistant that calculates your diabetes risk and explains it in simple words using official medical guidelines.



\## Problem Statement

Many young adults in Malaysia do not know they might be at risk of Type 2 Diabetes. Existing calculators use medical terms like second degree relatives and only give you a scary score without explaining what to do next. Because it is too confusing, people just give up. Guide to Dia solves this by using AI to translate the medical jargon into simple empowering words and giving you one clear next step.



\## Evidence of the Problem

\- The Malaysian Clinical Practice Guidelines state that about 50 percent of people with diabetes are undiagnosed. This shows we need better screening awareness.

<<<<<<< HEAD
\- Existing risk calculators like ModAsian FINDRISC use hard clinical words which makes it difficult for young adults to understand and answer.
=======
Students often struggle to find timely, accurate scholarship information because eligibility rules, deadlines and application steps are scattered across government, university, and scholarship-body websites. Advisors also answer repetitive questions. ScholarshipScope provides source-grounded answers with citations and uncertainty warnings.
>>>>>>> 72eeed018e0288f68e36db2d4c539d99f810db05



\## Initial Related Work and Existing Approaches

\- MyDiabeticRisk 2023. A web app that predicts risk. The limitation is it does not explain the medical terms or give conversational advice.

\- ModAsian FINDRISC. A clinical questionnaire. The limitation is it is very jargon heavy and does not tell you why a certain factor is risky or what to do next.

\- General AI Chatbots. The limitation is they hallucinate and make up fake medical advice which is very dangerous.

<<<<<<< HEAD

=======
\- Official scholarship information is distributed across separate portals and directories like MIS and Study Malaysia, so applicants must compare multiple sources and monitor changing deadlines manually.

Evidence 2: A systematic review of LLM-based chatbots in higher education confirms RAG is the leading architecture for reducing hallucinations, but challenges with accuracy, privacy and costs persist.
>>>>>>> 72eeed018e0288f68e36db2d4c539d99f810db05

\## Engineering Gap and Limitation

There is a need for a hybrid system. A simple rule based calculator combined with a RAG based AI assistant. The AI acts like a medical translator. It explains the risk in simple words, cites official guidelines, and gives one clear action step while keeping the user safe.



\## Project Objectives

\- O1. To design a hybrid AI assistant that calculates diabetes risk and explains it using simple language and official health documents.

\- O2. To develop the solution with retrieval source citation and strict medical safety guardrails.

\- O3. To evaluate the solution based on explanation groundedness safety and latency.



\## Target Users

Young adults aged 18 to 35 in Malaysia. Mostly university students and early career professionals.



<<<<<<< HEAD
\## Data Sources
=======
Existing RAG solutions focus on institutional data or generic search. There is a need for a citation-first, document-grounded assistant focused specifically on external scholarship information with guardrails for uncertain, outdated    or time-sensitive information.
>>>>>>> 72eeed018e0288f68e36db2d4c539d99f810db05

\- CDC Prediabetes Risk Test Guidelines at https://www.cdc.gov/prediabetes/

\- Malaysian Clinical Practice Guidelines on Type 2 Diabetes at http://www.moh.gov.my

<<<<<<< HEAD
\- WHO Diabetes Fact Sheet at https://www.who.int/news-room/fact-sheets/detail/diabetes
=======
Initial Project Objectives



1: Design a document-grounded AI scholarship information assistant based on official public sources.

2: Develop the solution with retrieval, source citation, and guardrails for uncertain or outdated information.

3: Evaluate answer accuracy, groundedness, citation correctness, latency, and safety.



Target Users



Prospective and current students searching for scholarships, plus academic advisors and student support staff who answer scholarship-related questions.



Data Sources



Source 1: Malaysia International Scholarship (MIS) official site — https://biasiswa.mohe.gov.my/INTER/

Source 2: Study Malaysia Scholarship Directory — https://studymalaysia.com/scholarships/

Source 3: Ministry of Higher Education Malaysia portal — https://www.mohe.gov.my/



Comparison with EduAdvisor



Similar to EduAdvisor: Both are document-grounded question-answering assistants. Both use retrieval from public documents, provide cited answers, and require guardrails to reduce hallucination and unsafe responses. Both can be evaluated on answer quality, groundedness, latency, and safety.



Different from EduAdvisor: EduAdvisor focuses on university course prerequisites and academic advising. ScholarshipScope focuses on scholarship eligibility, deadlines, required documents and application steps. It must handle time-sensitive information and explicitly warn users when deadlines or rules may be outdated. It also targets prospective students and scholarship applicants rather than course registration.



Responsible AI Risks



Risk 1 — Hallucination / outdated information: The system may invent an eligibility rule or deadline, or cite an outdated page. A student could miss a deadline or prepare the wrong documents.

Mitigation idea: Require citations, show source date, and return “I cannot confirm from the provided sources” when evidence is missing.



Risk 2 — Fairness / access: The system may perform better for English-language or well-documented scholarships and worse for other languages or underrepresented groups.

Mitigation idea: Evaluate across source types and languages, and state coverage limits clearly.



Risk 3 — Privacy: Users may enter personal details such as income, ID numbers or grades.

Mitigation idea: Warn users not to submit personal data and avoid storing sensitive inputs.

>>>>>>> 72eeed018e0288f68e36db2d4c539d99f810db05

\- ModAsian FINDRISC Risk Calculator at https://diag.vn/en/risk-prediction-for-10-year-type-2-diabetes/



\## Weekly Progress

| Week | Focus | Deliverable | Status |

|---|---|---|---|

| W1 | System map and project scope | README and docs | Complete |

<<<<<<< HEAD
| W2 | Requirements and Architecture | Week 2 docs | Complete |

| W3 | Data Pipeline | Week 3 docs and notebook | In Progress |
=======
| W2 | TBD | TBD | Complete |

| W3 | TBD | TBD | In-Progress |
>>>>>>> 72eeed018e0288f68e36db2d4c539d99f810db05

| W4 | TBD | TBD | Planned |

| W5 | TBD | TBD | Planned |

| W6 | TBD | TBD | Planned |

| W7 | TBD | TBD | Planned |

| W8 | TBD | TBD | Planned |

| W9 | TBD | TBD | Planned |

| W10 | TBD | TBD | Planned |

| W11 | TBD | TBD | Planned |

| W12 | TBD | TBD | Planned |

| W13 | TBD | TBD | Planned |

| W14 | TBD | TBD | Planned |

