# Hunt Report: HUNT-001 Potential PowerShell Obfuscation


---


## Hunt Information

| Field | Value |
|---|---|
| **Analyst** | Adekola Durodola |
| **Status** | Completed |
| **Investigation period** | Q3 2026 (Lab Capture Windows) |
| **Systems reviewed** | SOC-WIN11 |


---


## Security Question

Did PowerShell execute with obfuscation patterns that were not limited to `-EncodedCommand` or `-enc`?


---


## Hypothesis

If PowerShell was used to obscure command intent, Sysmon Event ID 1 may contain dynamic execution, Base64 conversion, character construction, backticks, or unusual string manipulation.


---


## MITRE ATT&CK Context

- T1059.001, PowerShell
- T1027, Obfuscated Files or Information


---


## Required Data

| Data | Required fields | Available |
|---|---|---|
| Sysmon Event ID 1 | Time, host, user, image, parent, command line | Yes |


---


## Expected Behaviour

Known SIEM and Threat detection lab data PowerShell tests may appear. These should be classified using their documented test timelines.


---


## Search Strategy

1. Review all PowerShell process events.
2. Categorize selected obfuscation signals.
3. Investigate matching events chronologically.
4. Review nearby process, file, DNS, and network activity.
5. Compare leads with authorized test records.


---


## Leads Identified

| Lead | Signal | User | Parent | Source | Investigation result |
|---|---|---|---|---|---|
| `powershell.exe` | Base64 Dynamic Block (`SQBu...`) | `Adekola` | `MicrosoftupdateEdge.exe` | Sysmon ID 1 | Verified as authorized endpoint emulation framework test testing WMI hook queries. |

---


## Findings

The broad SPL regex hunt surfaced a highly unique execution signature under a non-standard application parent path. 

The command wrapper initiated a heavily packed payload utilizing the `-EncodedCommand` block structure alongside hidden window execution switches (`-w hidden`). CyberChef parsing and decoding of the base64 blob string string revealed a structured array layout utilizing backtick escape sequences and string fragmentation (`$a='Get-'+'WmiObject'`) designed to manually validate and stress-test localized logging detection thresholds. 

Cross-referencing the execution timestamps against the laboratory control logs confirmed this event correlated precisely with scheduled adversary-emulation tests.


---


## Alternative Explanations

- Authorized detection validation
- Administrative scripting
- Software deployment
- Security tooling
- Legitimate encoded data handling


---


## Verdict

- **Classification:** Benign positive (Authorized emulation testing)
- **Confidence:** High
- **Reason:** Chronological event matching aligns perfectly with documented framework validation windows. The behavior mimicking evasion was safely initiated from an administrative session profile (`AdminLab`).


---


## Detection Opportunities

Keep separate logic for encoded arguments, dynamic execution, character construction, Base64 decoding, and unusual parent-child context.


---


## Telemetry Gaps

Multi-line message logs within the target CSV export required intensive runtime regex slicing. This structural layout introduces severe ingestion processing overhead and poses an engineering failure risk if upstream indentation schema updates break the parsing logic.


---

## Recommended Action

Create a **separate candidate detection rule** focusing tightly on obfuscated patterns initialized by non-standard app parents (`MicrosoftupdateEdge.exe`) rather than modifying the broader existing framework policy rules.
