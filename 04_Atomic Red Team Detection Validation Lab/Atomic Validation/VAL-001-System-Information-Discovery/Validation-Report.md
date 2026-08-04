# Validation Report: VAL-001

## Test Information

| Field | Value |
|---|---|
| Validation ID | VAL-001 |
| Date | 2026-08-04 |
| Analyst | Adekola Durodola |
| Host | SOC-WIN11 |
| Technique | System Information Discovery T1082 |
| Technique ID | T1082 |
| Atomic test number | 1 |
| Atomic test name |  System Information Discovery |
| Test GUID |  66703791-c902-4560-8770-42b8a91f7667 |
| Execution window | 2026-08-04 12:56:44 -07:00 to 2026-08-04 12:58:11 -07:00 |


---


## Purpose

Determine whether a reviewed system-information discovery test is visible and reconstructable from endpoint telemetry.

---


## Risk Assessment

The current test definition must be reviewed before execution. Approval requires harmless built-in behaviour, no security-control weakening, no credential access, no persistence, and no unknown downloads.

---


## Expected Actions

The exact reviewed test should read system details through built-in Windows or PowerShell commands.


---


## Expected Telemetry

- Sysmon Event ID 1 process creation
- Command line, image, parent process, user, and timestamp
- Additional event IDs only when supported by the active Sysmon configuration

---

## Existing Detection Expectation

No existing alert is expected.

---


## Decision Record

| Decision | Reason | Value |
|---|---|---|
| Preserve the initial gap | Week 7 did not cover system discovery | Creates an honest baseline |
| Avoid alerting on every discovery command | These commands are common | Prevents noisy alerting |
| Prefer command clusters or unusual parent context | Combined behaviour is more informative | Improves triage |


---

## Prerequisite Check

| Check | Result |
|---|---|
| Exact test reviewed | Yes |
| Current command approved | Yes |
| Prerequisites satisfied | Yes |
| Cleanup understood | Not required |
| Defender enabled | Yes |
| Snapshot available | Yes |


---


## Execution


```powershell
Invoke-AtomicTest T1082 - 1
```

---


## Validation Layers

| Layer | Evidence | Result |
|---|---|---|
| Generation | Atomic output and execution time | Pass |
| Collection | Windows or Sysmon event | Pass |
| Ingestion | Event found in Splunk | Pass |
| Parsing | Required fields extracted | Pass |
| Detection | Existing rule result | Not Expected |
| Alerting | Alert record | Not Expected |
| Investigation | Timeline reconstructed | Pass |
| Cleanup | Artifact removal verified | Pass |

---


## Detection Result

| Field | Value |
|---|---|
| Existing detection | No existing alert is expected. |
| Expected result | Sysmon captures execution telemetry without generating a high-severity alert. |
| Actual result | Sysmon Event ID 1 captured systeminfo.exe spawned via PowerShell under a normal administrative context. |
| Alert generated | Yes |
| Status | Validated |

---

## Gap Analysis

- **Observed gap:** No gap identified for telemetry collection; logs are populated as expected. However, an alert gap exists if trying to detect system profiling by malicious actors.
- **Layer affected:** Detection Layer
- **Root cause:**  The commands utilized are natively benign and lack an unusual parent process tree or cluster context to split them from daily administrator noise.
- **Risk:** Attackers can freely map the host OS structure without triggering legacy alert thresholds.
- **Action:** Draft DET-004 only when useful context can be defined.

---

## Legitimate Context

Help-desk diagnostics, administrator troubleshooting, inventory scripts, software installation, and authorized security testing.

---

## Cleanup

Run the reviewed cleanup command when one exists. Read-only discovery may require no cleanup, but the framework result must still be recorded.

---


## Cleanup Verification

| Check | Expected | Actual |
|---|---|---|
| Test file absent | True or Not Applicable | True |
| Unexpected process absent | True | True |
| Defender enabled | True | True |
| Sysmon running | True | True |
| Snapshot restore required | No | No |

---

## Verdict

The evidence proves that Sysmon accurately collects and ingests execution telemetry for standard system discovery commands. It does not prove malicious intent due to the high volume of legitimate administrative baseline noise, meaning a standalone alert rule remains unfeasible without a behavioral correlation cluster.

## Evidence

- **Atomic Command:** systeminfo.exe executed natively inside Invoke-AtomicTest engine.
- **Local Telemetry:** Windows Security log and Sysmon Log entry verified for Process Creation (Event ID 1) with an execution timestamp match of 2026-08-04 12:57:15.
- **SIEM Check:** Verified inside Splunk interface using index hunting against host SOC-WIN11.



![Process creation query](https://github.com/Adeconcept/Home-SOC-Lab/blob/af04dbfb616f3c757f180d9821938387be860a4e/04_Atomic%20Red%20Team%20Detection%20Validation%20Lab/Screenshots/00_Val_001_Process_creation.png)

*Figure 1. Process creation query*


![Extracting process details](https://github.com/Adeconcept/Home-SOC-Lab/blob/af04dbfb616f3c757f180d9821938387be860a4e/04_Atomic%20Red%20Team%20Detection%20Validation%20Lab/Screenshots/01_Val_001_extract%20data.png)

*Figure 2. Extracting process details.*


![Search for likely discovery commands:](https://github.com/Adeconcept/Home-SOC-Lab/blob/af04dbfb616f3c757f180d9821938387be860a4e/04_Atomic%20Red%20Team%20Detection%20Validation%20Lab/Screenshots/02_Val_001_messgae_quesry.png)

*Figure 3. Search for likely discovery commands:*
