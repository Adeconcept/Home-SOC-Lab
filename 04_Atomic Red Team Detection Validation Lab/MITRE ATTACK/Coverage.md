# MITRE ATT&CK Coverage

## Week 8 Validation Coverage

| Technique | Evidence claim | Status note |
|---|---|---|
| T1082 System Information Discovery | Telemetry validation attempted | Do not mark as fully detected |
| T1016 System Network Configuration Discovery | Telemetry validation attempted | Detection candidate only |
| T1059.003 Windows Command Shell | Telemetry validation attempted | Context-based candidate only |
| T1027 Obfuscated Files or Information | Detection validation attempted | Record DET-002 pass or miss |
| T1110.001 Password Guessing | Week 7 detection validation | Retained coverage |
| T1059.001 PowerShell | Week 7 detection validation | Retained coverage |

## Coverage Language

Use:

- Telemetry validated
- Detection validated
- Detection gap
- Detection candidate
- Validation attempted

Avoid:

- Full technique covered
- Attack stopped
- Malicious activity confirmed
- Command and control detected

## Coverage Decision

One atomic test provides evidence for one behaviour implementation. It does not prove complete visibility or detection for the entire ATT&CK technique.
