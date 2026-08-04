# Validation Report: VAL-001

## Test Information

| Field | Value |
|---|---|
| Validation ID | VAL-001 |
| Date | [Add actual date] |
| Analyst | Adekola Durodola |
| Host | SOC-WIN11 |
| Technique | System Information Discovery |
| Technique ID | T1082 |
| Atomic test number | [Confirm locally] |
| Atomic test name | [Add current test name] |
| Test GUID | [Add current GUID] |
| Execution window | [Add start and end time] |

## Purpose

Determine whether a reviewed system-information discovery test is visible and reconstructable from endpoint telemetry.

## Risk Assessment

The current test definition must be reviewed before execution. Approval requires harmless built-in behaviour, no security-control weakening, no credential access, no persistence, and no unknown downloads.

## Expected Actions

The exact reviewed test should read system details through built-in Windows or PowerShell commands.

## Expected Telemetry

- Sysmon Event ID 1 process creation
- Command line, image, parent process, user, and timestamp
- Additional event IDs only when supported by the active Sysmon configuration

## Existing Detection Expectation

No existing alert is expected.

## Decision Record

| Decision | Reason | Value |
|---|---|---|
| Preserve the initial gap | Week 7 did not cover system discovery | Creates an honest baseline |
| Avoid alerting on every discovery command | These commands are common | Prevents noisy alerting |
| Prefer command clusters or unusual parent context | Combined behaviour is more informative | Improves triage |

## Prerequisite Check

| Check | Result |
|---|---|
| Exact test reviewed | [Yes or No] |
| Current command approved | [Yes or No] |
| Prerequisites satisfied | [Yes or No] |
| Cleanup understood | [Yes or Not Required] |
| Defender enabled | [Yes or No] |
| Snapshot available | [Yes or No] |

## Execution

Use only the exact command shown by the reviewed test definition.

```powershell
Invoke-AtomicTest T1082 -TestNumbers [CONFIRMED-NUMBER]
```

## Validation Layers

| Layer | Evidence | Result |
|---|---|---|
| Generation | Atomic output and execution time | [Pass, Fail, or Blocked] |
| Collection | Windows or Sysmon event | [Pass or Fail] |
| Ingestion | Event found in Splunk | [Pass or Fail] |
| Parsing | Required fields extracted | [Pass or Fail] |
| Detection | Existing rule result | [Pass, Miss, or Not Expected] |
| Alerting | Alert record | [Pass, Fail, Not Expected, or Restricted] |
| Investigation | Timeline reconstructed | [Pass or Fail] |
| Cleanup | Artifact removal verified | [Pass or Fail] |

## Detection Result

| Field | Value |
|---|---|
| Existing detection | No existing alert is expected. |
| Expected result | [Add expected result] |
| Actual result | [Add actual result] |
| Alert generated | [Yes, No, Not Expected, or Restricted] |
| Status | [Validated, Gap Identified, Needs Telemetry, or Failed] |

## Gap Analysis

- **Observed gap:** [Add actual gap]
- **Layer affected:** [Add layer]
- **Root cause:** [Add evidence-based cause]
- **Risk:** [Add one sentence]
- **Action:** Draft DET-004 only when useful context can be defined.

## Legitimate Context

Help-desk diagnostics, administrator troubleshooting, inventory scripts, software installation, and authorized security testing.

## Cleanup

Run the reviewed cleanup command when one exists. Read-only discovery may require no cleanup, but the framework result must still be recorded.

## Cleanup Verification

| Check | Expected | Actual |
|---|---|---|
| Test file absent | True or Not Applicable | [Add actual result] |
| Unexpected process absent | True | [Add actual result] |
| Defender enabled | True | [Add actual result] |
| Sysmon running | True | [Add actual result] |
| Snapshot restore required | No | [Add actual result] |

## Verdict

[Add a concise conclusion explaining what the evidence proves, what it does not prove, and the next detection decision.]

## Evidence

Add the current test details, risk review, prerequisite output, execution, local telemetry, Splunk result, detection result, and cleanup verification.
