# Hunting Methodology

## Seven-Stage Process

| Stage | Question | Output |
|---|---|---|
| Define the question | What behaviour are we looking for? | Security question |
| Write the hypothesis | What evidence supports or refutes it? | Testable hypothesis |
| Identify data | Which logs and fields are required? | Data requirements |
| Establish expected behaviour | What is normal in this dataset? | Limited baseline |
| Search broadly | What population should be reviewed first? | Broad search |
| Investigate anomalies | Which leads need enrichment? | Timeline and alternatives |
| Operationalize | What should change after the hunt? | Candidate, logging improvement, or no action |


---


## Hunting Versus Detection

| Detection | Threat hunting |
|---|---|
| Runs repeatedly | Conducted as an investigation |
| Uses defined conditions | Begins with a hypothesis |
| Should create manageable alerts | Can use broad searches |
| Often threshold-based | Often exploratory |
| Requires stable fields | Can expose data gaps |
| Produces an alert | Produces knowledge |


---


## Quality Principles

1. Begin broad enough to reduce confirmation bias.
2. Record data requirements before searching.
3. Treat rare events as leads.
4. Compare leads with known test timelines.
5. Consider legitimate explanations.
6. Document zero-result and inconclusive hunts.
7. Do not save every hunt as an alert.
8. Operationalize only stable patterns.
