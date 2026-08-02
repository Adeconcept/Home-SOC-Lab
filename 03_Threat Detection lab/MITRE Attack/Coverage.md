# MITRE ATT&CK Coverage

## Coverage Summary

| Detection | Tactic | Technique | Mapping rationale |
|---|---|---|---|
| DET-001 | Credential Access | T1110.001 Password Guessing | Multiple incorrect passwords were tested against one controlled account |
| DET-002 | Execution | T1059.001 PowerShell | PowerShell process creation includes encoded-command arguments |
| DET-003 | Execution | T1059.001 PowerShell | PowerShell execution is associated with network or DNS telemetry |

## Mapping Decisions

### DET-001

The activity is mapped to **Password Guessing**, not Password Spraying. The test targets one account with multiple incorrect passwords.

### DET-002

The detection identifies encoded PowerShell execution. Encoding may be used for legitimate automation, so the mapping describes execution behaviour rather than malicious intent.

### DET-003

The detection remains mapped primarily to PowerShell. A single external connection or DNS query does not provide enough context to claim command and control.

## Coverage Boundaries

This project does not claim complete coverage of:

- Credential Access
- Execution
- Command and Control

It covers two specific sub-techniques using the telemetry available in the lab.

## Navigator Files

- `week7-detection-coverage.json`, editable ATT&CK Navigator layer
- `week7-detection-coverage.png`, add the exported Navigator screenshot after creation

## Evidence to Add

Add the exported ATT&CK Navigator PNG using:

```text
mitre-01-attack-coverage.png
```

Caption:

> ATT&CK Navigator layer showing the two sub-techniques covered by the Week 7 detection lab.
