# Project Report

## Project

**Atomic Red Team Detection Validation Lab**

## Purpose

Test whether selected attacker-like behaviours are generated, collected, ingested, parsed, detected, investigated, and cleaned up safely.


---


## Validation Summary

| Validation | Security question | Result | Decision |
|---|---|---|---|
| VAL-001 | Is system-information discovery visible? | Telemetry validated; Sysmon collected `systeminfo.exe` but no alert triggered. | Defer standalone alerting; track context in backlog. |
| VAL-002 | Is network discovery visible as a sequence? | Telemetry validated; multiple utilities were logged in sequence but missed by alerts. | Draft DET-005 sequence behavioral correlation. |
| VAL-003 | Is command-shell and file activity reconstructable? | Telemetry validated; `cmd.exe` process execution logged cleanly under PowerShell. | Defer broad rule; draft risky-path DET-006. |
| VAL-004 | Does DET-002 detect the selected obfuscation method? | Detection validated; Base64 encoding argument flags matched perfectly. | Keep DET-002; add broader string rule DET-007 to backlog. |


--


## Decisions and Value

### Safety was treated as a technical requirement

A clean VM snapshot, scope restrictions, reviewed commands, and cleanup verification were required before validation.

**Value:** Demonstrates controlled, authorized, and reversible security testing.

### Test numbers were not trusted blindly

The current test name, command, prerequisites, artifacts, and cleanup were reviewed locally.

**Value:** Reduces the risk of executing a changed or different test.

### No-alert outcomes were decomposed

Generation, collection, ingestion, parsing, detection, alerting, investigation, and cleanup were assessed separately.

**Value:** Prevents a rule gap from being confused with failed collection or ingestion.

### Existing coverage was preserved before tuning

DET-002 was tested unchanged before improvement.

**Value:** Produces credible evidence of what the detection library actually covered.

### Detection candidates require context

Discovery and command-shell tools are common, so candidates use combinations, unusual parents, risky paths, or follow-on behaviour.

**Value:** Demonstrates signal-quality judgment.


---


## Detection Improvement Outcome

| Candidate | Behaviour | Status |
|---|---|---|
| DET-004 | Clustered or unusual system discovery | Backlog |
| DET-005 | Multiple network-discovery commands | Backlog |
| DET-006 | Shell script execution from a risky path | Backlog |
| DET-007 | Potentially obfuscated PowerShell | Backlog |


---


## Quality Reporting

- Tests executed successfully: `4`
- Tests with Sysmon telemetry: `4`
- Tests ingested into Splunk: `4`
- Existing detections triggered: `1`
- Detection gaps identified: `3`
- Cleanup checks passed: `4`
- Security controls blocked activity: `0` *(Windows Defender real-time bypass applied successfully)*



---


## Operational Value

This project demonstrates how controlled emulation can confirm telemetry, test detections, identify blind spots, prioritize improvements, support investigation, and verify endpoint recovery.
