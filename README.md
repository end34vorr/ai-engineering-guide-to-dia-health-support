\# Guide to Dia



An AI assistant that calculates your diabetes risk and explains it in simple words using official medical guidelines.



\## Problem Statement

Many young adults in Malaysia do not know they might be at risk of Type 2 Diabetes. Existing calculators use medical terms like second degree relatives and only give you a scary score without explaining what to do next. Because it is too confusing, people just give up. Guide to Dia solves this by using AI to translate the medical jargon into simple empowering words and giving you one clear next step.



\## Evidence of the Problem

\- The Malaysian Clinical Practice Guidelines state that about 50 percent of people with diabetes are undiagnosed. This shows we need better screening awareness.

\- Existing risk calculators like ModAsian FINDRISC use hard clinical words which makes it difficult for young adults to understand and answer.



\## Initial Related Work and Existing Approaches

\- MyDiabeticRisk 2023. A web app that predicts risk. The limitation is it does not explain the medical terms or give conversational advice.

\- ModAsian FINDRISC. A clinical questionnaire. The limitation is it is very jargon heavy and does not tell you why a certain factor is risky or what to do next.

\- General AI Chatbots. The limitation is they hallucinate and make up fake medical advice which is very dangerous.



\## Engineering Gap and Limitation

There is a need for a hybrid system. A simple rule based calculator combined with a RAG based AI assistant. The AI acts like a medical translator. It explains the risk in simple words, cites official guidelines, and gives one clear action step while keeping the user safe.



\## Project Objectives

\- O1. To design a hybrid AI assistant that calculates diabetes risk and explains it using simple language and official health documents.

\- O2. To develop the solution with retrieval source citation and strict medical safety guardrails.

\- O3. To evaluate the solution based on explanation groundedness safety and latency.



\## Target Users

Young adults aged 18 to 35 in Malaysia. Mostly university students and early career professionals.



\## Data Sources

\- CDC Prediabetes Risk Test Guidelines at https://www.cdc.gov/prediabetes/

\- Malaysian Clinical Practice Guidelines on Type 2 Diabetes at http://www.moh.gov.my

\- WHO Diabetes Fact Sheet at https://www.who.int/news-room/fact-sheets/detail/diabetes

\- ModAsian FINDRISC Risk Calculator at https://diag.vn/en/risk-prediction-for-10-year-type-2-diabetes/



\## Weekly Progress

| Week | Focus | Deliverable | Status |

|---|---|---|---|

| W1 | System map and project scope | README and docs | Complete |

| W2 | Requirements and Architecture | Week 2 docs | Complete |

| W3 | Data Pipeline | Week 3 docs and notebook | In Progress |

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

