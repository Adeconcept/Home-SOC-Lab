# Week 8 Project Report

## Project

**Atomic Red Team Detection Validation Lab**

## Purpose

Test whether selected attacker-like behaviours are generated, collected, ingested, parsed, detected, investigated, and cleaned up safely.

## Validation Summary

| Validation | Security question | Result | Decision |
|---|---|---|---|
| VAL-001 | Is system-information discovery visible? | [Add actual result] | Draft or defer DET-004 |
| VAL-002 | Is network discovery visible as a sequence? | [Add actual result] | Draft or defer DET-005 |
| VAL-003 | Is command-shell and file activity reconstructable? | [Add actual result] | Draft or defer DET-006 |
| VAL-004 | Does DET-002 detect the selected obfuscation method? | [Add actual result] | Keep, tune, or add DET-007 |

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

## Detection Improvement Outcome

| Candidate | Behaviour | Status |
|---|---|---|
| DET-004 | Clustered or unusual system discovery | Backlog |
| DET-005 | Multiple network-discovery commands | Backlog |
| DET-006 | Shell script execution from a risky path | Backlog |
| DET-007 | Potentially obfuscated PowerShell | [Drafted or Not Required] |

## Quality Reporting

Use actual counts:

- Tests executed successfully: `[Add count]`
- Tests with Sysmon telemetry: `[Add count]`
- Tests ingested into Splunk: `[Add count]`
- Existing detections triggered: `[Add count]`
- Detection gaps identified: `[Add count]`
- Cleanup checks passed: `[Add count]`
- Security controls blocked activity: `[Add count]`

## Operational Value

The project demonstrates how controlled emulation can confirm telemetry, test detections, identify blind spots, prioritize improvements, support investigation, and verify endpoint recovery.
