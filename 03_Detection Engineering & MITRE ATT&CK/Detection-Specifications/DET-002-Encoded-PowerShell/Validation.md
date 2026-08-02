# DET-002 Validation

## Harmless Positive Test

```powershell
$text = 'Write-Output "Week 7 DET-002 validation successful"'

$encoded = [Convert]::ToBase64String(
    [Text.Encoding]::Unicode.GetBytes($text)
)

powershell.exe -NoProfile -EncodedCommand $encoded
```

The command prints harmless text and creates the expected process telemetry.

## Test Record

| Test ID | Activity | Expected | Actual | Result |
|---|---|---|---|---|
| DET002-N1 | Normal PowerShell without encoding | No detection result | [Add actual] | [Pass or Fail] |
| DET002-P1 | Harmless encoded PowerShell | Detection result | [Add actual] | [Pass or Fail] |
| DET002-C1 | Confirm user, command line, and parent fields | Fields populated | [Add actual] | [Pass or Fail] |
| DET002-R1 | Repeat encoded test | Detection result | [Add actual] | [Pass or Fail] |

## Evidence Fields

| Field | Value |
|---|---|
| Host | [Add actual] |
| User | [Add actual] |
| Parent process | [Add actual] |
| Command line visible | [Yes or No] |
| Test start time | [Add actual] |
| Detection severity | [Add actual] |
| Alert created | [Yes, No, or Trial Restricted] |

## Validation Verdict

**Status:** [Validated, Needs Tuning, or Failed]

**Evidence-based conclusion:** [Add one sentence describing only what the results prove]
