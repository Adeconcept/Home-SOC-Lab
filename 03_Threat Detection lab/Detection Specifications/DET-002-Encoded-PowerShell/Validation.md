# DET-002 Validation

## Harmless Positive Test

```powershell
$text = 'Write-Output "DET-002 validation successful"'

$encoded = [Convert]::ToBase64String(
    [Text.Encoding]::Unicode.GetBytes($text)
)

powershell.exe -NoProfile -EncodedCommand $encoded
```

The command prints harmless text and creates the expected process telemetry.

## Test Record

| Test ID | Activity | Expected | Actual | Result |
|---|---|---|---|---|
| DET002-N1 | Normal PowerShell without encoding | No detection result | System authorized PowerShell activity | Pass ✅ |
| DET002-P1 | Harmless encoded PowerShell | Detection result | Detection result | Pass ✅ |
| DET002-C1 | Confirm user, command line, and parent fields | Fields populated | Fields populated (User, CommandLine, ParentImage visible) | Pass ✅ |
| DET002-R1 | Repeat encoded test | Detection result | Detection result | Pass ✅ |

## Evidence Fields

| Field | Value |
|---|---|
| Host | SOC-WIN11 |
| User | Adekola |
| Parent process | pwsh.exe |
| Command line visible | Yes |
| Test start time | 8/2/26 3:24:02.000 PM |
| Detection severity | Medium |
| Alert created | Yes |

## Validation Verdict

**Status:** Validated

**Evidence-based conclusion:** The test results prove that the tuned query accurately isolates encoded PowerShell execution blocks while capturing full process context fields and dynamically elevating alert severity for high-risk parent processes.
