# Hunt Report: HUNT-005 PowerShell Followed by DNS or Network Activity

---


## Security Question

Did PowerShell produce DNS or outbound network activity shortly after execution?


---


## Hypothesis

If PowerShell communicated externally, process and network telemetry should show PowerShell associated with DNS queries or outbound connections near process start.


---


## MITRE ATT&CK Context

- T1059.001, PowerShell
- Additional mapping depends on destination and purpose

A normal request to `example.com` does not establish command-and-control behaviour.

---


## Required Data

- Sysmon Event ID 1
- Sysmon Event ID 22
- Sysmon Event ID 3 when available
- Sysmon Event ID 11 for optional file context


---


## Search Strategy

1. Review PowerShell DNS events.
2. Review PowerShell connections.
3. Identify low-frequency destinations.
4. Correlate process, DNS, network, and file activity.
5. Compare the sequence with known test notes.


---


## Leads Identified

| Time | User | Destination | Evidence type | Result |
|---|---|---|---|---|
| Chronological Sync | `Adekola` | `://example.com` / `aka.ms` | DNS Query (Sysmon ID 22) | **Benign Positive**: Administrative loop and framework verification. |
| Chronological Sync | `Adekola` | `48.209.138.168` | External IP Mapping via `svc.exe` parent tree | **Benign Positive**: Cloud service connectivity validation. |


---

## Findings

Correlating process execution logs with name resolution histories revealed clear outbound administrative loops. The `powershell.exe` engine generated standard lookup queries (Sysmon Event ID 22) for `aka.ms` (legitimate Microsoft optimization endpoint) and `://example.com` (routine syntax testing loop) shortly after initialization. 

While Event ID 3 (Network Connections) was restricted within some log slices, temporal cross-layer mapping with Event ID 22 and localized parent tree traces connected this activity to an outbound communications profile interacting with the rare external IP destination `48.209.138.168` via the `svc.exe` process runtime stack. 

The domain selections and infrastructure hooks align completely with documented lab testing schedules, showing no indicators of malicious secondary staging or unauthorized payload extraction.


---


## Alternative Explanations

- Authorized connectivity testing
- Administrative downloads
- API automation
- Software deployment
- Cloud-management modules


---


## Verdict

- **Classification:** Benign positive (Authorized connectivity validation)
- **Confidence:** High
- **Reason:** The domains resolved and targets utilized correspond accurately with the laboratory's active framework configuration profiles. The operations executed entirely inside an approved user context.

---


## Detection Opportunity

**DET-010: PowerShell with Suspicious Network Context**

Higher-value context may include a new domain, direct external IP, encoded execution, unusual parent process, or file creation after communication.

To implement candidate **DET-010**, construct a Splunk query targeting non-standard domains or standalone external IPs initiated directly from an active administrative shell instance:

```spl
index=sysmon EventCode=22 Image="*powershell.exe" 
| search NOT (QueryName IN ("*.microsoft.com", "*.windows.com", "*.aka.ms"))
```

---


## Limitation

DNS evidence does not prove a successful connection. Event ID 3 availability must be documented.
