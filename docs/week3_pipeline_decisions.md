\# Week 3 Pipeline Decisions for Guide to Dia



\*\*1. Requirement Link\*\*

This data pipeline supports FR1 and RAI1. To calculate risk and provide safe medical answers the system needs to read the official text guidelines.



\*\*2. Selected Source\*\*

We used a synthetic text file named diabetes\_guidelines.txt to represent the official clinical guidelines.



\*\*3. Part B Comparison and Evidence\*\*

We compared fixed size chunking with paragraph aware chunking. 

Fixed size chunking cut sentences in half which lost the context. 

Paragraph aware chunking kept the full sentences together. 

We selected paragraph aware chunking because medical rules must be read as complete sentences to avoid confusion.



\*\*4. Accepted Limitation\*\*

The current chunking method does not preserve PDF page numbers because we are using a text file. If we use real PDFs later we may lose exact page references for citations.



\*\*5. ETL or ELT Decision\*\*

We used an ETL process. We extract the text, clean the formatting and chunk it into records. This is a batch process because the medical guidelines do not change every day.



\*\*6. Stage Contracts\*\*



| Stage | Contract or evidence | Failure response |

|---|---|---|

| Ingest | The file diabetes\_guidelines.txt exists and is readable. | Stop if the file is missing. |

| Inspect | The text is not empty and has characters. | Stop if the file is empty. |

| Prepare | Paragraph aware method produces the prepared text file. | Stop if no chunks are produced. |

| Validate | Output file exists and is non empty with correct hash. | Stop if output is missing. |

| Record lineage | Source link is written to the pipeline manifest. | Stop if mapping is missing. |

