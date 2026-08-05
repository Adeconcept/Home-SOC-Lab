# MITRE ATT&CK Coverage

## Week 8 Validation Coverage

| Technique | Evidence claim | Status note |
|---|---|---|
| **T1082 System Information Discovery** | Telemetry validated | Detection gap; do not mark as fully detected. |
| **T1016 System Network Configuration Discovery** | Telemetry validated | Detection gap; multi-command sequence detection candidate only. |
| **T1059.003 Windows Command Shell** | Telemetry validated | Detection gap; context-based candidate only to avoid baseline noise. |
| **T1027 Obfuscated Files or Information** | Detection validated | Record DET-002 pass; successfully matched explicit Base64 argument encoding flags. |
| **T1110.001 Password Guessing** | Detection validated | Retained coverage from Week 7 active baseline rules. |
| **T1059.001 PowerShell** | Detection validated | Retained coverage from Week 7 active baseline rules. |

---


## Coverage Language

I Used:

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
