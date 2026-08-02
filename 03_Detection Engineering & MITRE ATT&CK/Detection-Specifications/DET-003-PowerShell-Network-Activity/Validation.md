# DET-003 Validation

## Harmless Positive Test

```powershell
Test-NetConnection example.com -Port 443

Invoke-WebRequest `
    -Uri "https://example.com" `
    -UseBasicParsing |
Select-Object StatusCode
```

`example.com` is used only for controlled connectivity testing.

## Test Record

| Test ID | Activity | Expected | Actual | Result |
|---|---|---|---|---|
| DET003-T1 | Check Event ID 3 availability | Count greater than zero, or gap recorded | [Add actual] | [Pass or Gap] |
| DET003-N1 | Normal browser traffic | No PowerShell result | [Add actual] | [Pass or Fail] |
| DET003-P1 | PowerShell web request | Network or DNS result | [Add actual] | [Pass or Fail] |
| DET003-C1 | Extract destination | Destination populated | [Add actual] | [Pass or Fail] |
| DET003-R1 | Repeat PowerShell request | Consistent result | [Add actual] | [Pass or Fail] |

## Evidence Fields

| Field | Value |
|---|---|
| Event ID 3 available | [Yes or No] |
| Event ID 22 available | [Yes or No] |
| Detection name used | [Network Activity or DNS Query Activity] |
| Host | [Add actual] |
| Destination IP or domain | [Add actual] |
| Destination port | [Add actual or Not Available] |
| Alert created | [Yes, No, or Trial Restricted] |

## Validation Verdict

**Status:** [Validated, Needs Telemetry, Needs Tuning, or Failed]

**Evidence-based conclusion:** [Add one sentence describing only what the results prove]
