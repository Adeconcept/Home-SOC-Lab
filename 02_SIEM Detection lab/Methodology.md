# Investigation Methodology

> **Project:** SIEM Detection Lab: Windows Event Investigation with Splunk
>
> **Purpose:** Define the structured investigation process used throughout this project to ensure every case is investigated consistently, objectively, and with evidence.

---

## Why an Investigation Methodology Matters

Security investigations should never begin with conclusions.

They begin with questions.

A SIEM generates alerts based on predefined conditions, but an alert alone does not determine whether malicious activity has occurred. The responsibility of the analyst is to gather evidence, validate assumptions, and determine the most accurate explanation supported by the available telemetry.

Following a consistent investigation methodology helps:

* Reduce bias during investigations.
* Improve investigation quality.
* Produce repeatable analysis.
* Support incident response.
* Document findings clearly.
* Minimize false positives.

Throughout this project, every investigation followed the same structured workflow.

---

## Investigation Workflow

```text
Alert or Suspicious Activity
            │
            ▼
Develop Initial Hypothesis
            │
            ▼
Define Investigation Scope
            │
            ▼
Collect Evidence
            │
            ▼
Execute SPL Searches
            │
            ▼
Build Timeline
            │
            ▼
Analyze Evidence
            │
            ▼
Map to MITRE ATT&CK
            │
            ▼
Determine Investigation Outcome
            │
            ▼
Recommend Analyst Action
            │
            ▼
Document Lessons Learned
```

This workflow was applied consistently across all three investigation cases in this project.

---

## Step 1. Alert or Suspicious Activity

Every investigation begins with an observation.

Examples include:

* Multiple failed authentication attempts.
* PowerShell execution.
* Unexpected process creation.
* DNS activity.
* Network connections.

At this stage, nothing is assumed to be malicious.

The objective is simply to identify behaviour that warrants further investigation.

---

## Step 2. Develop an Initial Hypothesis

Before collecting evidence, an initial hypothesis is created.

A hypothesis is a possible explanation, not a conclusion.

For example:

**Observed Activity**

Three failed authentication attempts.

Possible explanations:

* User entered an incorrect password.
* Password spraying.
* Brute-force attack.
* Laboratory testing.

Each possibility remains valid until evidence supports or rejects it.

---

## Step 3. Define the Investigation Scope

Clearly defining the investigation scope prevents unnecessary analysis and ensures that only relevant data is reviewed.

For each investigation, the following scope was documented:

* Host
* User account
* Investigation window
* Relevant Event IDs
* Data sources
* Investigation objective

Example:

| Field        | Value                         |
| ------------ | ----------------------------- |
| Host         | Windows 11 ARM                |
| Account      | Lab User                      |
| Time Window  | 15 Minutes                    |
| Data Sources | Windows Security, Sysmon      |
| Event IDs    | 4624, 4625, Sysmon Event ID 1 |

---

## Step 4. Collect Evidence

Evidence was collected from multiple telemetry sources to support the investigation.

Primary data sources included:

* Windows Security Logs
* Sysmon Operational Logs
* PowerShell activity
* Process creation events
* DNS activity
* Network activity

Each source contributed a different part of the investigation.

For example:

Windows Security Logs answered:

> Who authenticated?

Sysmon answered:

> What process executed?

Combining multiple sources improves confidence in investigation findings.

---

## Step 5. Execute SPL Searches

Evidence was retrieved using Splunk Search Processing Language (SPL).

Each query was written to answer a specific investigative question.

Examples include:

| Question                      | Example SPL      |
| ----------------------------- | ---------------- |
| Were there failed logons?     | `EventCode=4625` |
| Did PowerShell execute?       | `powershell`     |
| Which processes were created? | `EventCode=1`    |

Running searches without a clear question often produces excessive results that are difficult to interpret.

---

## Step 6. Build a Timeline

Events were arranged chronologically to understand how activity unfolded.

Timeline analysis allows investigators to identify:

* Cause and effect.
* Event sequence.
* Parent-child relationships.
* User actions.
* Process execution order.

Example timeline:

09:42

Failed Logon

↓

09:43

Failed Logon

↓

09:44

Failed Logon

↓

09:45

Successful Logon

↓

09:46

PowerShell Started

A timeline provides context that isolated events cannot.

---

## Step 7. Analyze the Evidence

Analysis focuses on what the evidence supports and what it does not support.

Rather than asking:

"Did an attacker compromise the system?"

The investigation asks:

"What explanation is most consistent with the available evidence?"

For every case, the following questions were considered:

* Does the timeline support the hypothesis?
* Do multiple data sources agree?
* Are there alternative explanations?
* Does the activity match the documented lab testing?
* Is additional evidence required?

This approach reduces confirmation bias.

---

## Step 8. Map to MITRE ATT&CK

Where appropriate, observations were mapped to relevant MITRE ATT&CK techniques.

This mapping provides a standardized language for describing attacker behaviour.

Examples used in this project include:

| Technique | Description |
| --------- | ----------- |
| T1110     | Brute Force |
| T1059.001 | PowerShell  |

MITRE mappings describe possible attacker techniques.

They do not independently confirm malicious activity.

---

## Step 9. Determine the Investigation Outcome

Each investigation concluded with a documented outcome.

Possible outcomes include:

* Closed
* Monitoring Required
* Escalated
* Benign Positive
* False Positive
* Confirmed Malicious

All investigations within this project concluded as **Benign Positive** because the telemetry matched authorized laboratory testing.

---

## Step 10. Recommend Analyst Action

Every investigation should conclude with a recommended next step.

Possible recommendations include:

* Close the investigation.
* Continue monitoring.
* Escalate to Tier 2.
* Collect additional evidence.
* Isolate the endpoint.
* Notify the incident response team.

Recommendations should be proportional to the evidence collected.

---

## Investigation Principles

Throughout this project, the following principles guided every investigation.

### Evidence Before Assumption

Conclusions should only be supported by collected evidence.

---

### Context Matters

A suspicious indicator is not automatically malicious.

Context determines meaning.

---

### Correlate Multiple Sources

Avoid relying on a single event.

Use multiple telemetry sources whenever possible.

---

### Maintain Objectivity

Alternative explanations should always be considered before reaching a conclusion.

---

### Document Everything

Every search, finding, limitation, and conclusion should be documented to support future review and knowledge sharing.

---

## Common Investigation Pitfalls

Several common mistakes were identified during this lab.

Avoid:

* Assuming alerts equal attacks.
* Ignoring timestamps.
* Investigating outside the defined scope.
* Relying on a single event.
* Ignoring false positives.
* Skipping documentation.

Recognizing these pitfalls helped improve the quality and consistency of each investigation.

---

## Lessons Learned

One of the most valuable lessons from this project was understanding that investigation quality depends more on methodology than on tooling.

Splunk provides the platform for searching and analyzing telemetry, but it is the analyst's structured approach that transforms raw events into meaningful conclusions.

Following the same methodology across multiple cases improved consistency, reduced assumptions, and made the investigation process easier to explain and reproduce.

This disciplined approach will continue to guide future projects involving detection engineering, threat hunting, and incident response.
