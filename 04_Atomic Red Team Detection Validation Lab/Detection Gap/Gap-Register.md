# Detection Gap Register

## Summary

| Gap | Validation | Finding | Proposed action | Priority | Status |
|---|---|---|---|---|---|
| GAP-001 | VAL-001 | System discovery telemetry visible, no existing detection | Draft contextual DET-004 | Low | Backlog |
| GAP-002 | VAL-002 | Network discovery visible, no sequence detection | Draft DET-005 | Medium | Backlog |
| GAP-003 | VAL-003 | Command shell visible, broad detection would be noisy | Define risky-path DET-006 | Medium | Backlog |
| GAP-004 | VAL-004 | DET-002 may miss non-encoded obfuscation | Draft or tune DET-007 | Medium | Backlog |

---


## GAP-001: System Discovery Not Detected

- **Technique:** T1082
- **Validation:** VAL-001
- **Telemetry:** Sysmon Event ID 1 process creation captured `systeminfo.exe` spawned via `powershell.exe` at 12:57:15.
- **Detection:** Miss (Not Expected); zero alerts generated in the active SIEM repository.
- **Root cause:** Current detection library does not cover system discovery
- **Risk:** Discovery can support post-compromise reconnaissance
- **Improvement:** Detect clusters of discovery commands from an unusual parent or user
- **Priority:** Low
- **Status:** Backlog


---

## GAP-002: Network Discovery Sequence Not Detected

- **Technique:** T1016
- **Validation:** VAL-002
- **Telemetry:** Sysmon Event ID 1 tracked rapid sequential execution of `ipconfig /all`, `route print`, `arp -a`, and `nbtstat -n` between 13:20:10 and 13:20:45.
- **Detection:** Miss (Not Expected); logs parsed successfully in Splunk but did not trigger an alert condition.
- **Root cause:** No rule groups multiple discovery utilities
- **Risk:** A sequence may reveal active reconnaissance
- **Improvement:** Detect two or more distinct discovery commands within five minutes
- **Priority:** Medium
- **Status:** Backlog


---


## GAP-003: Command Shell Requires Context

- **Technique:** T1059.003
- **Validation:** VAL-003
- **Telemetry:** Sysmon Event ID 1 captured `cmd.exe /c "echo Atomic Test"` spawned via an administrative PowerShell parent process wrapper at 13:42:35.
- **Detection:** Miss (Not Expected); binary execution logged natively but bypassed alert triggers to prevent baseline alert fatigue.
- **Root cause:** Ordinary command-shell use is too common for a broad alert
- **Risk:** Script execution from risky paths may remain unnoticed
- **Improvement:** Add path, script extension, parent process, and follow-on context
- **Priority:** Medium
- **Status:** Backlog


---


## GAP-004: Obfuscated PowerShell Outside DET-002 Scope

- **Technique:** T1027
- **Validation:** VAL-004
- **Telemetry:** Sysmon Event ID 1 successfully collected the full `-EncodedCommand` payload string inside the PowerShell process metrics block at 14:10:32.
- **Detection:** Pass; existing rule DET-002 matched the explicit Base64 argument encoding flags.
- **Root cause:** DET-002 is highly effective for explicit encoding flags, but leaves a logic gap for non-encoded obfuscation styles (such as string reversal or character manipulation).
- **Risk:** Obfuscation not using encoded arguments may not match DET-002
- **Improvement:** Keep DET-002 focused and develop DET-007 for broader signals
- **Priority:** Medium
- **Status:** Backlog
