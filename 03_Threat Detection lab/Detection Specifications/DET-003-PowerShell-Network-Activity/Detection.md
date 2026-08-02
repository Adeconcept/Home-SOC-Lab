# DET-003: PowerShell Network Activity

## Summary

Identifies PowerShell associated with Sysmon network connection telemetry. When Event ID 3 is unavailable, the fallback detection must be renamed **PowerShell DNS Query Activity** and use Event ID 22.

## Status

| Field | Value |
|---|---|
| Status | Testing or Needs Telemetry |
| Version | 1.0 |
| Severity | Medium |
| Confidence | Medium with Event ID 3, lower with DNS-only evidence |
| Data source | Sysmon |
| Event IDs | 1 and 3, or 22 for DNS fallback |
| ATT&CK | T1059.001 PowerShell |

## Detection Hypothesis

PowerShell creating an outbound connection shortly after execution may require review, especially when the destination, command, or parent process is unusual.

## Telemetry Decision

Run this first:

```spl
index=endpoint
sourcetype="csv:sysmon"
Id="3"
| stats count
```

- If Event ID 3 is available, use the network query.
- If Event ID 3 is unavailable but Event ID 22 is present, use the DNS query and rename the detection.
- If neither is available, record a telemetry gap and do not claim validation.

Primary query: [Query.spl](Query.spl)

DNS fallback: [DNS-Fallback.spl](DNS-Fallback.spl)

## Decision Record

| Decision | Why | Value |
|---|---|---|
| Require Event ID 3 for connection claims | Network connections need network telemetry | Prevents unsupported conclusions |
| Use Event ID 22 only as a named DNS fallback | DNS queries are not proof of an outbound connection | Preserves technical accuracy |
| Keep severity Medium | PowerShell frequently performs legitimate web activity | Avoids over-prioritizing common administration |
| Map primarily to T1059.001 | Network activity alone does not prove command and control | Keeps ATT&CK mapping defensible |
| Extract destination context | Destination helps determine expected or unusual activity | Improves analyst triage |

## Known False Positives

- Administrative scripts downloading approved files
- Software installation and update scripts
- Cloud-management modules
- API automation
- Security testing
- Approved deployment activity

## Limitations

- Event ID 3 may not be enabled.
- DNS evidence does not confirm a successful connection.
- A connection to an external destination does not prove malicious intent.
- Uploaded CSV data does not provide real-time process correlation.
- Destination reputation is not included.

## Evidence

Add:

- Event ID 3 availability result
- Safe PowerShell web request
- Matching Event ID 3 or 22
- Extracted destination
- Nearby process activity
- Alert or saved-report configuration
