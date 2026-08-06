# Project Report


---


## Project

**Threat Hunting Lab: Hypothesis-Driven Analysis with Splunk**


---


## Purpose

Proactively search for suspicious behaviour that may not produce an existing alert, while documenting benign explanations, negative findings, telemetry gaps, and detection opportunities.


---


## Outcome Summary

| Area | Intended outcome | Actual result |
|---|---|---|
| **Data health** | Confirm sources, time ranges, event IDs, and duplication risk | **Validated**: Confirmed functional event routing across Windows Security logs (4624/4625) and Sysmon events (1/3/11/22). Accounted for static CSV duplication risk through index deduplication. |
| **Process baseline** | Identify common and rare process relationships | **Established**: Profiled core execution frequency. Identified high-volume system wrappers and browsers (`svc.exe`, `WmiPrvSE.exe`, `MicrosoftupdateEdge.exe`, `cmd.exe`) alongside anomalous single-execution discovery binaries. |
| **Authentication baseline** | Summarize successes and failures | **Completed**: Profiled `114` successful and `11` failed logons. Isolated credential friction to localized administrative users while identifying message field extraction gaps. |
| **Network baseline** | Summarize DNS or connection activity | **Established**: Identified `svc.exe` as the most active DNS actor. Documented administrative lookups to `aka.ms` and `example.com` along with a rare external IP lead (`48.209.138.168`). |
| **Five hunts** | Investigate five hypotheses | **Executed**: Analyzed PowerShell encoding, rapid reconnaissance tracking, parent-child shell pairs, credential loops, and outbound socket connections. |
| **Detection backlog** | Produce at least three candidates | **Delivered**: Formalized three high-fidelity behavioral engineering metrics (**DET-008**, **DET-009**, and **DET-010**) ready for testing pipelines. |
| **Gap register** | Record collection and parsing limits | **Documented**: Recorded systemic visibility limitations regarding missing continuous log streams, unparsed multi-line event payloads, and absence of global proxy/EDR metadata. |


---


## Decisions and Value

### Hunting began with questions, not indicators

Each hunt was defined before SPL was written.

**Value:** Reduces confirmation bias and keeps the work tied to a testable hypothesis.

### Broad searches were kept separate from alerts

The project intentionally uses noisy searches for exploration.

**Value:** Demonstrates the difference between hunting and automated detection.

### Rare activity was not labelled malicious

The dataset is too small for production-quality anomaly baselines.

**Value:** Preserves credibility.

### Negative findings were retained

A hunt with no concerning result still records what was searched, what data supported the conclusion, and what limitations remain.

**Value:** Demonstrates complete analytical reporting.

### Findings were operationalized carefully

Only useful behavioural patterns move into the detection backlog.

**Value:** Prevents exploratory searches from becoming noisy alerts.


---


## Hunt Outcomes

| Hunt | Finding | Verdict | Confidence | Operational output |
|---|---|---|---|---|
| **HUNT-001** | Base64 encoded execution string found in Sysmon Process Create block. | Benign Positive (Authorized Test) | High | Refined regex logic matching shorthand parameters like `-ec`. |
| **HUNT-002** | Rapid 14-second burst execution of `whoami.exe`, `systeminfo.exe`, and `ipconfig.exe`. | Benign Positive (Emulation Test) | High | **DET-008**: Sliding-window transaction analytic candidate. |
| **HUNT-003** | Profiled shell parents; found only standard system binaries. | Negative Finding | High | Parent process exclusion matrix baseline. |
| **HUNT-004** | Sequence of 3 rapid authentication failures preceding immediate success. | Benign User Error | High | Temporal join correlation model for credential logs. |
| **HUNT-005** | PowerShell engine resolving lookups to `aka.ms` and `example.com` domains. | Benign Positive (Testing) | High | **DET-010**: DNS-to-process lineage alerting baseline. |


---


## Detection Opportunities

- DET-008, Multiple Discovery Commands
- DET-009, Office or Browser Launching an Interpreter
- DET-010, PowerShell with Suspicious Network Context
- DET-001 enrichment, success after repeated failures
- Separate PowerShell detections by obfuscation signal

## Main Limitation

The lab uses manually uploaded data from one endpoint across separate windows. It demonstrates hunting methodology and evidence interpretation, not enterprise-scale anomaly detection.
