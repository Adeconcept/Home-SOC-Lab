# Detection Engineering Process

## Purpose

This process converts an investigation finding into detection logic that is testable, explainable, and useful to an analyst.


---


## Lifecycle

| Stage | Main question | Output |
|---|---|---|
| Identify behaviour | What activity should be visible? | Behaviour statement |
| Confirm telemetry | Which event records it, and is it in Splunk? | Data requirements |
| Write hypothesis | What pattern may justify review? | Detection hypothesis |
| Build logic | How is the pattern represented in SPL? | Detection query |
| Validate | Does known activity match? | Test evidence |
| Test normal activity | Does related normal activity remain quiet? | Negative-test evidence |
| Review false positives | What legitimate activity may match? | False-positive list |
| Tune | How can noise be reduced without hiding risk? | Versioned decision |
| Deploy | What schedule, trigger, and suppression are appropriate? | Alert design |
| Maintain | When should the rule be reviewed? | Review plan |


---



## Detection, Alert, and Investigation

| Term | Meaning |
|---|---|
| Detection | Logic that identifies a defined behaviour in available telemetry |
| Alert | A notification or record created when the detection condition matches |
| Investigation | The analyst review that determines context, legitimacy, and required action |

A detection match is not proof of compromise.


---



## Specification Checklist

Every detection in this project records:

- Detection ID and name
- Status and version
- Purpose and hypothesis
- Data source and required fields
- SPL query
- Threshold and time window
- Trigger condition
- MITRE ATT&CK mapping
- Severity and confidence
- Validation method and results
- Known false positives
- Tuning decisions
- Analyst response
- Limitations
- Future improvements


---




## Quality Principles

1. Use the narrowest useful behaviour, not the broadest possible search.
2. State which data supports the conclusion.
3. Do not label authorized test activity as malicious.
4. Treat lab thresholds as assumptions until production baselines exist.
5. Do not add exclusions only to make an alert quiet.
6. Keep ATT&CK mappings limited to behaviour actually evidenced.
7. Record failed tests and telemetry gaps.
8. Revalidate after meaningful query, data, or environment changes.


