\# Requirements Specification for Guide to Dia



\## Section 0 Project Framing and Application Scope



\*\*Problem and Evidence\*\*

Many young adults in Malaysia are unaware of their Type 2 Diabetes risk. The Malaysian Clinical Practice Guidelines report high rates of undiagnosed diabetes. Existing risk calculators like ModAsian FINDRISC use confusing medical words which causes users to guess or quit the tool.



\*\*Existing Approaches\*\*

1\. MyDiabeticRisk 2023 is a web based tool without conversational explanations.

2\. ModAsian FINDRISC is a validated questionnaire but it uses too much medical jargon.

3\. General AI like ChatGPT is prone to hallucinating fake medical advice.



\*\*Project Engineering Gap\*\*

A gap exists for a hybrid system. We need a rule based risk calculator combined with a RAG based AI assistant. The AI acts as a medical translator that explains the risk in simple words and cites official clinical guidelines.



\*\*Objective O1\*\*

To design a hybrid AI assistant that calculates diabetes risk and explains it using simple language and official health documents.



\*\*Objective O2\*\*

To develop the solution with retrieval, source citation, and strict medical safety guardrails.



\*\*Objective O3\*\*

To evaluate the solution based on explanation groundedness, safety, and latency.



\*\*Intended Users\*\*

Young adults aged 18 to 35 in Malaysia.



\*\*Supported Tasks\*\*

Calculating risk scores, translating medical jargon, and recommending screening tests.



\*\*Information Domain\*\*

Public health guidelines from CDC, WHO, and Malaysian CPG.



\*\*Operating Context\*\*

A web based chat interface.



\*\*Important Constraints\*\*

No medical diagnosis, strict privacy for health data, and use of plain English.



\*\*Out of Scope Tasks\*\*

The system SHALL NOT diagnose diabetes, prescribe medication, or replace a doctor.



\*\*Evaluation Focus\*\*

To determine whether the system accurately translates medical jargon and explains risk without hallucinating medical advice.



\## Section 1 Functional Requirements



| Number | Requirement The system SHALL | Scope Boundary SHALL NOT |

|---|---|---|

| FR1 | Calculate a diabetes risk score based on user input like Age, BMI, Blood Pressure, and Family History. | Provide a definitive medical diagnosis. |

| FR2 | Explain the calculated risk by retrieving relevant information from official clinical guidelines. | Invent risk factors or medical guidelines not in the source text. |

| FR3 | Translate medical jargon like second degree relative and Fasting Plasma Glucose into simple plain English. | Use complex clinical terms without explaining them. |

| FR4 | Provide a verifiable source reference for every clinical claim. | Present unsupported medical information as if it came from a doctor. |

| FR5 | Decline queries that fall outside the defined health screening scope. | Provide medical treatment, prescription, or cure advice. |

| FR6 | Explicitly state when the provided sources do not contain enough information to answer the query. | Guess or hallucinate a medical answer. |



\## Section 2 Non Functional Requirements



| Number | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |

|---|---|---|---|---|---|

| NFR1 | Performance | The system shall keep interactive responses within the agreed latency target. | Measure end to end latency on a defined test workload. | p90 latency | Less than 3000 ms |

| NFR2 | Correctness | Risk calculations shall match the standard CDC or CPG mathematical logic. | Test 20 predefined user profiles against expected scores. | Correct calculation rate | 100 percent |

| NFR3 | Maintainability | Updated clinical guidelines shall be incorporated without rebuilding the entire application. | Run a controlled document update test. | Update time | Less than 15 minutes |

| NFR4 | Cost | Inference cost shall remain within the student project budget. | Record model or API usage over the evaluation workload. | Cost per 100 queries | Less than 0.50 US dollars per 100 queries |



\## Section 3 Responsible AI Requirements



| Number | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |

|---|---|---|---|---|---|

| RAI1 | Medical Safety and No Diagnosis | The system shall explicitly state it is an AI not a doctor and refuse to diagnose the user. | Test 20 queries asking Do I have diabetes. | Refusal and redirect success rate | 100 percent |

| RAI2 | Privacy | The system shall not store or transmit user personal data like Age, BMI, and Blood Pressure. | Run privacy test cases and inspect logs. | Number of detected personal data transmissions | 0 instances |

| RAI3 | Transparency and Grounding | The system shall provide a verifiable source reference for every explanation. | Inspect responses for citation presence. | Citation presence rate | 100 percent |

| RAI4 | Hallucination and Fake Cures | The system shall not recommend unproven cures or medications. | Test with queries asking for diabetes cures. | Unsupported medical claim rate | 0 percent |



\## Reflection Part A



\*\*Question\*\*

Which Responsible AI requirement in your project will be the most difficult to measure and why.



\*\*Answer\*\*

RAI1 which is Medical Safety will be the hardest to measure. It is difficult to test every possible way a user might phrase a question that sounds like they are asking for a diagnosis. I need to create a test set that covers all edge cases like someone typing my blood sugar is 10 what should I do. This requires careful thought to ensure the guardrail catches it and redirects to a doctor.



\## Reflection Part B



\*\*Question\*\*

Which evaluation criterion is most important for determining whether your AI application will be useful to its intended users? Why?



\*\*Answer\*\*

For me the most important criterion is Medical Safety and No Diagnosis which is RAI1. If this app accidentally tells a young person that they are safe when they actually have high risk factors, or if it suggests a fake cure, it could cause real harm. People will only use a health app if they trust it completely. If the app acts like a real doctor and makes a mistake, users will delete it immediately. So making sure the AI knows its limits and always redirects to a real clinic is the most important thing for this app to be useful.



\## Reflection Part C



\*\*Question\*\*

Which architecture decision involved the most important trade-off? Which requirement drove the decision and what trade-off did you accept?



\*\*Answer\*\*

Adding the Dynamic Knowledge Access which is the RAG component for Decision 1 was the biggest trade-off. The requirement driving this was RAI3 for Transparency and Grounding and also FR2 for explaining the risk using official guidelines. If we just used a normal ChatGPT model without RAG it would be much faster and easier to build but it would definitely hallucinate fake medical rules. So we accepted the trade-off of a slightly slower response time and a more complicated system. I think it is worth it because for a health app getting the medical facts right is way more important than saving one second of loading time.

