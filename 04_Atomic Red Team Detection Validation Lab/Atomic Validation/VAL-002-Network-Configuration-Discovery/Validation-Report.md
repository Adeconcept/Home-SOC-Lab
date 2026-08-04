# Validation Report: VAL-002

## Test Information

| Field | Value |
|---|---|
| Validation ID | VAL-002 |
| Date | 2026-08-04 |
| Analyst | Adekola Durodola |
| Host | SOC-WIN11 |
| Technique | System Network Configuration Discovery |
| Technique ID | T1016 |
| Atomic test number | 1 |
| Atomic test name |  System Network Configuration Discovery on Windows |
| Test GUID | 970ab6a1-0157-4f3f-9a73-ec4166754b23 |
| Execution window |  2026-08-04 13:19:54 -07:00 to 2026-08-04 13:22:24 -07:00|

---

## Purpose

Determine whether a reviewed network-configuration discovery test is visible and whether several commands can be grouped into one investigation lead.


---


## Risk Assessment

The current test definition must be reviewed before execution. Approval requires harmless built-in behaviour, no security-control weakening, no credential access, no persistence, and no unknown downloads.


---


## Expected Actions

The reviewed test should read local network configuration through built-in utilities or PowerShell cmdlets.


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
| Group several commands within five minutes | One command is often normal | Adds behavioural context |
| Preserve user and parent context | Actor and launch context affect risk | Supports triage |
| Exclude firewall-modification tests | Scope is read-only discovery | Maintains safety |


---


## Prerequisite Check

| Check | Result |
|---|---|
| Exact test reviewed | Yes |
| Current command approved | Yes |
| Prerequisites satisfied | [Yes |
| Cleanup understood | Not required |
| Defender enabled | Yes |
| Snapshot available | Yes |



---


## Execution

```powershell
Invoke-AtomicTest T1016 -1
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
| Alerting | Alert record | Not expected |
| Investigation | Timeline reconstructed | Pass |
| Cleanup | Artifact removal verified | Pass |


---


## Detection Result

| Field | Value |
|---|---|
| Existing detection | No existing alert is expected. |
| Expected result | Sysmon collects separate process events that can be grouped into a single behavioral cluster. |
| Actual result | Sysmon Event ID 1 captured a rapid sequence of commands (ipconfig /all, route print, arp -a) within a 1-minute window under powershell.exe |
| Alert generated | Yes |
| Status | Validated |


---


## Gap Analysis

- **Observed gap:** Standalone network discovery commands fail to trigger alerts because single execution parameters reflect normal user/admin behavior.
- **Layer affected:** Detection Layer
- **Root cause:** The correlation engine lacks a sequencing or threshold rule to track command grouping (e.g., more than 3 network tools executed within 5 minutes by an unusual parent).
- **Risk:** Attackers can rapidly map local network topology and routing tables without crossing threshold boundaries.
- **Action:** Draft DET-005 as a behavioural sequence, not a single-command alert.


---


## Legitimate Context

Network troubleshooting, VPN support, administrator diagnostics, installation scripts, and authorized assessment.


---


## Cleanup

Run cleanup when the current definition provides it. Read-only discovery may not create persistent artifacts.


---


## Cleanup Verification

| Check | Expected | Actual |
|---|---|---|
| Test file absent | True or Not Applicable | True |
| Unexpected process absent | True | True |
| Defender enabled | True | True |
| Sysmon running | True | True |
| Snapshot restore required | No | [No |


---


## Verdict

The telemetry proves that all system network configuration discovery commands are successfully recorded by Sysmon and parsed by Splunk. It does not prove malicious intent on an individual basis, validating that future detections must prioritize command clustering (grouping multiple network utilities within 5 minutes) rather than signaling on isolated executions.


---

## Evidence

- **Execution Command:** ipconfig /all, route print, arp -a, and nbtstat -n run programmatically via the atomic engine.
- **Local Telemetry:** Sysmon Event ID 1 logged sequentially matching the target timestamp window.
- **Splunk Verification:** Target logs tracked in the corporate index environment under host SOC-WIN11.


![Splunk Search](https://github.com/Adeconcept/Home-SOC-Lab/blob/2c39758f4b1940c99bd1cd26fff27e797dc01102/04_Atomic%20Red%20Team%20Detection%20Validation%20Lab/Screenshots/03_Val_002_network_request_query.png)

*Figure 1. Splunk search for ipconfig and other network related query.*


![Extracting process details]([https://github.com/Adeconcept/Home-SOC-Lab/blob/af04dbfb616f3c757f180d9821938387be860a4e/04_Atomic%20Red%20Team%20Detection%20Validation%20Lab/Screenshots/01_Val_001_extract%20data.png](https://github.com/Adeconcept/Home-SOC-Lab/blob/2c39758f4b1940c99bd1cd26fff27e797dc01102/04_Atomic%20Red%20Team%20Detection%20Validation%20Lab/Screenshots/04_Val_002_discovery_chain_query.png))

*Figure 2. Building a discovery chain query.*


