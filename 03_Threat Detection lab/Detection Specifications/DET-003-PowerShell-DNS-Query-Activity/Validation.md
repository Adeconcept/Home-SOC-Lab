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


---



## Test Record

| Test ID | Activity | Expected | Actual | Result |
|---|---|---|---|---|
| DET003-T1 | Check Event ID 3 availability | Count greater than zero, or gap recorded | Gap recorded (0 events found) | Pass ✅ |
| DET003-N1 | Normal browser traffic | No PowerShell result | No result (browser traffic omitted) | Pass ✅ |
| DET003-P1 | PowerShell web request | Network or DNS result | Detection result (Sysmon ID 22 caught) | Pass ✅ |
| DET003-C1 | Extract destination | Destination populated | Destination populated (QueryName visible) | Pass ✅ |
| DET003-R1 | Repeat PowerShell request | Consistent result | Consistent result| Pass ✅ |


---



## Evidence Fields

| Field | Value |
|---|---|
| Event ID 3 available | No|
| Event ID 22 available | Yes |
| Detection name used | DNS Query Activity |
| Host | SOC-WIN11 |
| Destination IP or domain | Example.com |
| Destination port | Not Available |
| Alert created | Yes |



---



## Validation Verdict

**Status:** Validated

**Evidence-based conclusion:** The validation testing proves that while network connection logging (Event ID 3) was absent, the detection successfully adapted to isolate outbound PowerShell process behavior by tracking and extracting destination domains from DNS query logs (Event ID 22).
