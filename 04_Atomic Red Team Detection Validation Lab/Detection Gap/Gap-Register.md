# Detection Gap Register

## Summary

| Gap | Validation | Finding | Proposed action | Priority | Status |
|---|---|---|---|---|---|
| GAP-001 | VAL-001 | System discovery telemetry visible, no existing detection | Draft contextual DET-004 | Low | Backlog |
| GAP-002 | VAL-002 | Network discovery visible, no sequence detection | Draft DET-005 | Medium | Backlog |
| GAP-003 | VAL-003 | Command shell visible, broad detection would be noisy | Define risky-path DET-006 | Medium | Backlog |
| GAP-004 | VAL-004 | DET-002 may miss non-encoded obfuscation | Draft or tune DET-007 | Medium | [Add status] |

## GAP-001: System Discovery Not Detected

- **Technique:** T1082
- **Validation:** VAL-001
- **Telemetry:** [Add actual result]
- **Detection:** [Add actual result]
- **Root cause:** Current detection library does not cover system discovery
- **Risk:** Discovery can support post-compromise reconnaissance
- **Improvement:** Detect clusters of discovery commands from an unusual parent or user
- **Priority:** Low
- **Status:** Backlog

## GAP-002: Network Discovery Sequence Not Detected

- **Technique:** T1016
- **Validation:** VAL-002
- **Telemetry:** [Add actual result]
- **Detection:** [Add actual result]
- **Root cause:** No rule groups multiple discovery utilities
- **Risk:** A sequence may reveal active reconnaissance
- **Improvement:** Detect two or more distinct discovery commands within five minutes
- **Priority:** Medium
- **Status:** Backlog

## GAP-003: Command Shell Requires Context

- **Technique:** T1059.003
- **Validation:** VAL-003
- **Telemetry:** [Add actual result]
- **Detection:** [Add actual result]
- **Root cause:** Ordinary command-shell use is too common for a broad alert
- **Risk:** Script execution from risky paths may remain unnoticed
- **Improvement:** Add path, script extension, parent process, and follow-on context
- **Priority:** Medium
- **Status:** Backlog

## GAP-004: Obfuscated PowerShell Outside DET-002 Scope

- **Technique:** T1027
- **Validation:** VAL-004
- **Telemetry:** [Add actual result]
- **Detection:** [Pass or Miss]
- **Root cause:** [Add actual cause]
- **Risk:** Obfuscation not using encoded arguments may not match DET-002
- **Improvement:** Keep DET-002 focused and develop DET-007 for broader signals
- **Priority:** Medium
- **Status:** [Backlog, Drafted, or Not Required]
