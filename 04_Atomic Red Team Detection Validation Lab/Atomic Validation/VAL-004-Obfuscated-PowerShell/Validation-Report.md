# Validation Report: VAL-004

## Test Information

| Field | Value |
|---|---|
| Validation ID | VAL-004 |
| Date | 2026-08-04 |
| Analyst | Adekola Durodola |
| Host | SOC-WIN11 |
| Technique | Obfuscated Files or Information |
| Technique ID | T1027 |
| Atomic test number | 2 |
| Atomic test name | PowerShell Execute encoded command |
| Test GUID | fcb698d2-1c21-4f16-bb7c-2b28cf3b429c |
| Execution window |  2026-08-04 13:51:50 -07:00 to 2026-08-04 13:52:48 -07:00 |

---

## Purpose

Test DET-002 unchanged against a reviewed harmless PowerShell obfuscation method, then preserve any coverage gap before improvement.

---

## Risk Assessment

The current test definition must be reviewed before execution. Approval requires harmless built-in behaviour, no security-control weakening, no credential access, no persistence, and no unknown downloads.

---

## Expected Actions

The approved test must run harmless PowerShell that prints text without downloads, persistence, credential access, or security-control changes.

---

## Existing Detection Expectation

DET-002, Encoded PowerShell Execution, must be run unchanged first.

---

## Decision Record

| Decision | Reason | Value |
|---|---|---|
| Run DET-002 unchanged first | Original coverage must be measured honestly | Produces credible validation |
| Keep encoded and broader obfuscation logic separate | They are related but different behaviours | Preserves rule clarity |
| Treat a miss as useful evidence | A miss identifies a coverage boundary | Supports targeted improvement |
| Label DET-007 as a learning rule | Simple string signals can be noisy | Avoids overstating maturity |

---

## Prerequisite Check

| Check | Result |
|---|---|
| Exact test reviewed | Yes |
| Current command approved | Yes |
| Prerequisites satisfied | Yes |
| Cleanup understood | Not Required |
| Defender enabled | Yes |
| Snapshot available | Yes |

---

## Execution

Use only the exact command shown by the reviewed test definition.

```powershell
Invoke-AtomicTest T1027 -TestNumbers 2
```

---

## Validation Layers

| Layer | Evidence | Result |
|---|---|---|
| Generation | Atomic output and execution time | Pass |
| Collection | Windows or Sysmon event | Pass |
| Ingestion | Event found in Splunk | Pass |
| Parsing | Required fields extracted | Pass |
| Detection | Existing rule result | Pass |
| Alerting | Alert record | Pass |
| Investigation | Timeline reconstructed | Pass |
| Cleanup | Artifact removal verified | Pass |

---

## Detection Result

| Field | Value |
|---|---|
| Existing detection | DET-002, Encoded PowerShell Execution, must be run unchanged first. |
| Expected result | DET-002 should trigger an alert when it encounters the `-EncodedCommand` or `-e` parameter flags in the command line telemetry. |
| Actual result | DET-002 successfully triggered a medium-severity alert on host `SOC-WIN11` upon detecting Base64 arguments inside the PowerShell process block. |
| Alert generated | Yes |
| Status | Validated |

---

## Gap Analysis

- **Observed gap:** While Base64 encoding flags (`-EncodedCommand`) are successfully caught by DET-002, other obfuscation behaviors (such as string concatenation, string reversal, or environmental variable substitution) bypass this rule completely.
- **Layer affected:** Detection Layer
- **Root cause:** DET-002 relies strictly on decoding flags and does not evaluate heavy special-character usage or entropy-based script obfuscation.
- **Risk:** Attackers can bypass existing process monitors by manually obfuscating individual syntax components instead of encoding the entire execution string.
- **Action:** Keep DET-002 focused and create DET-007 when the observed method uses other obfuscation signals.

---

## Legitimate Context

Complex administration, deployment tools, build processes, security tools, copied documentation commands, and legitimate encoded data handling.

---

## Cleanup

Run reviewed cleanup when one exists. When no artifact is created, document that cleanup was not required and verify the endpoint remained at baseline.

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

The validation evidence proves that DET-002 functions perfectly to capture explicit Base64 PowerShell arguments within a live SIEM environment. It does not prove complete protection against broader obfuscation mechanics, highlighting the critical next decision to keep DET-002 un-mutated while architecting DET-007 as a separate learning rule for complex string manipulation.

---

# VAL-004 Timeline

| Time | Layer | Evidence | Interpretation |
|---|---|---|---|
| **13:51:50** | Generation | Atomic test started | Analyst initiated the Base64 test execution through the framework console |
| **13:55:27** | Collection | Sysmon Event ID 1 | `powershell.exe` spawned containing a long `-EncodedCommand` payload string |
| **13:59:32** | Ingestion | Splunk event | Telemetry successfully written to index and parsed out key argument fields |
| **14:01:50** | Detection | Existing rule result | Pass; DET-002 matched on the specific encoding flag parameter |
| **14:12:45** | Cleanup | Cleanup output | Not Required; memory execution only, leaving zero file-system remnants |

### Timeline Verdict
The timeline proves that encoded execution can be precisely caught and alert-verified within a two-minute detection envelope.


![Splunk Search](https://github.com/Adeconcept/Home-SOC-Lab/blob/15c0507614ffefc34d8b61a968c8ffbe1f1e2681/04_Atomic%20Red%20Team%20Detection%20Validation%20Lab/Screenshots/08_Val_004_detection_search.png)

*Figure 1. Detection search with no result.*


![Splunk Search](https://github.com/Adeconcept/Home-SOC-Lab/blob/15c0507614ffefc34d8b61a968c8ffbe1f1e2681/04_Atomic%20Red%20Team%20Detection%20Validation%20Lab/Screenshots/09_Val_004_broader_investigation.png)

*Figure 2. Detection search with broader investigation query.*
