# Validation Report: VAL-003

## Test Information

| Field | Value |
|---|---|
| Validation ID | VAL-003 |
| Date | 2026-08-04 |
| Analyst | Adekola Durodola |
| Host | SOC-WIN11 |
| Technique | Windows Command Shell |
| Technique ID | T1059.003 |
| Atomic test number | 1 |
| Atomic test name | Run Command Prompt |
| Test GUID | dda0d4b8-f027-464a-93e5-f5b2b2b1a8d0 |
| Execution window | 2026-08-04 13:42:10 -07:00 to 2026-08-04 13:45:15 -07:00 |


---

## Purpose

Validate visibility for a reviewed harmless command-shell test and determine whether process and file activity can be reconstructed.

---

## Risk Assessment

The current test definition must be reviewed before execution. Approval requires harmless built-in behaviour, no security-control weakening, no credential access, no persistence, and no unknown downloads.

---

## Expected Actions

The approved test should create or display harmless text and may create a temporary batch, command, or text file.

---

## Existing Detection Expectation

No alert on ordinary `cmd.exe` is expected.

---

## Decision Record

| Decision | Reason | Value |
|---|---|---|
| Do not alert on every `cmd.exe` | Command Prompt is common | Avoids unusable noise |
| Review Event ID 11 when available | File activity adds context | Helps reconstruct the sequence |
| Require risky path or execution context | Context separates routine use from higher-interest activity | Improves precision |


---


## Prerequisite Check

| Check | Result |
|---|---|
| Exact test reviewed | Yes |
| Current command approved | Yes |
| Prerequisites satisfied | Yes |
| Cleanup understood | Yes |
| Defender enabled | Yes |
| Snapshot available | Yes |


---


## Execution

```powershell
Invoke-AtomicTest T1059.003 -TestNumbers 1
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
| Existing detection | No alert on ordinary `cmd.exe` is expected. |
| Expected result | Sysmon Event ID 1 captures the creation of `cmd.exe` along with its complete argument parameters. |
| Actual result | Sysmon tracked `cmd.exe /c "echo Atomic Test"` executing under the `powershell.exe` parent context. |
| Alert generated | Not Expected |
| Status | Validated |

---


## Gap Analysis

- **Observed gap:** Native `cmd.exe` process execution cannot be alert-baited safely because standalone execution metrics overlap entirely with legitimate help-desk and administrative automation.
- **Layer affected:** Detection Layer
- **Root cause:** The detection suite intentionally suppresses raw command shell warnings to keep the operational SIEM environment clear of baseline telemetry flooding.
- **Risk:** An adversary could run scripts within normal administrative paths unchecked unless secondary behavioral hooks are crossed.
- **Action:** Draft DET-006 only when a risky path, script extension, unusual parent, or follow-on behaviour exists.



---


## Legitimate Context

Batch administration, logon scripts, installation, software packaging, help-desk activity, and authorized testing.


---


## Cleanup

Run the exact reviewed cleanup command and verify the documented file path returns `False` with `Test-Path`.


---


## Cleanup Verification

| Check | Expected | Actual |
|---|---|---|
| Test file absent | True or Not Applicable | True |
| Unexpected process absent | True | True |
| Defender enabled | True | True |
| Sysmon running | True | True |
| Snapshot restore required | No | No |


--


## Verdict

The forensic evidence confirms that Sysmon completely monitors and parses transient Windows command shell structures when launched inside PowerShell. It does not validate a threat event standalone, verifying that tracking rules must hinge on malicious variables like suspicious execution paths (e.g., `\AppData\Local\Temp\`) or unusual child processes rather than simple binary execution. 


---

# VAL-003 Timeline

| Time | Layer | Evidence | Interpretation |
|---|---|---|---|
| **13:42:10** | Generation | Atomic test started | Execution command initiated by analyst via the framework core |
| **13:42:35** | Collection | Sysmon Event ID 1 | `cmd.exe` spawned, outputting the atomic telemetry string |
| **13:43:10** | Ingestion | Splunk event | The endpoint log details populated within the active Splunk monitoring panel |
| **13:44:00** | Detection | Existing rule result | Not Expected; verified that no noise alert triggered for standard execution |
| **13:45:15** | Cleanup | Cleanup output | Completed; script wrapper executed the default teardown routine cleanly |

### Timeline Verdict
The validation proves that transient command shell activity can be entirely audited and sequenced via parent-child process relationships across your collection layout.



![Splunk Search](https://github.com/Adeconcept/Home-SOC-Lab/blob/78ee2aa1b5138b82b0c8af7214bc8e99c95dc045/04_Atomic%20Red%20Team%20Detection%20Validation%20Lab/Screenshots/05_Val_003_spl_prcoess_%26_file_creation_query.png)

*Figure 1. Splunk search for process & file creation.*


![Process field extraction](https://github.com/Adeconcept/Home-SOC-Lab/blob/78ee2aa1b5138b82b0c8af7214bc8e99c95dc045/04_Atomic%20Red%20Team%20Detection%20Validation%20Lab/Screenshots/06_Val_003_process_fields_extraction.png)

*Figure 2. Process field extraction.*


![ FIle creation theory](https://github.com/Adeconcept/Home-SOC-Lab/blob/78ee2aa1b5138b82b0c8af7214bc8e99c95dc045/04_Atomic%20Red%20Team%20Detection%20Validation%20Lab/Screenshots/07_Val_003_file_created_query.png)

*Figure 3. FIle creation theory.*

