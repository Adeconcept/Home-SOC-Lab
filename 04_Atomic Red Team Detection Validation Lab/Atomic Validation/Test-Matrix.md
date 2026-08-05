# Atomic Test Matrix

| Validation | Technique | Current test number | Current test name | Command reviewed | Persistent artifacts | Cleanup reviewed | Approved |
|---|---|---:|---|---|---|---|---|
| VAL-001 | T1082 | 1 | System Information Discovery | Yes | None (Read-only) | Not Required | Yes |
| VAL-002 | T1016 | 1 | System Network Configuration Discovery on Windows | Yes | None (Read-only) | Not Required | Yes |
| VAL-003 | T1059.003 | 1 | Run Command Prompt | Yes | None (Standard stdout) | Yes | Yes |
| VAL-004 | T1027 | 2 | PowerShell Execute encoded command | Yes | None (Memory execution) | Not Required | Yes |

## Required Review Fields

### VAL-001: System Information Discovery (T1082)
* **GUID:** `66703791-c902-4560-8770-42b8a91f7667`
* **Source:** Official Red Canary Repository
* **Platform / Executor:** Windows / Command Prompt (`cmd.exe`) via PowerShell harness
* **Exact Command / Arguments:** `systeminfo.exe`
* **Prerequisites / Admin Required:** None / No (Can run as standard user)
* **Artifacts / Network Activity:** None / None
* **Cleanup Command:** None (Read-only query command)
* **Expected Telemetry:** Sysmon Event ID 1 (Process Creation) tracking `systeminfo.exe`
* **Risk Assessment:** Low risk; uses standard, benign built-in Windows diagnostic tools.

### VAL-002: System Network Configuration Discovery (T1016)
* **GUID:** `970ab6a1-0157-4f3f-9a73-ec4166754b23`
* **Source:** Official Red Canary Repository
* **Platform / Executor:** Windows / Command Prompt (`cmd.exe`) via PowerShell harness
* **Exact Command / Arguments:** `ipconfig /all`, `route print`, `arp -a`, `nbtstat -n`
* **Prerequisites / Admin Required:** None / No
* **Artifacts / Network Activity:** None / Local interface network routing queries only
* **Cleanup Command:** None (Read-only environment discovery tools)
* **Expected Telemetry:** Sysmon Event ID 1 sequential process creation events grouped within a 5-minute cluster
* **Risk Assessment:** Low risk; native utility execution with zero modifications to routing tables.

### VAL-003: Windows Command Shell (T1059.003)
* **GUID:** `dda0d4b8-f027-464a-93e5-f5b2b2b1a8d0`
* **Source:** Official Red Canary Repository
* **Platform / Executor:** Windows / PowerShell execution frame
* **Exact Command / Arguments:** `cmd.exe /c "echo Atomic Test"`
* **Prerequisites / Admin Required:** None / No
* **Artifacts / Network Activity:** Standard stdout console output text / None
* **Cleanup Command:** Built-in automation teardown routine via framework wrapper
* **Expected Telemetry:** Sysmon Event ID 1 mapping `cmd.exe` spawned via a `powershell.exe` parent context
* **Risk Assessment:** Low risk; prints static text to the active console wrapper with no sub-script staging.

### VAL-004: Obfuscated Files or Information (T1027)
* **GUID:** `fcb698d2-1c21-4f16-bb7c-2b28cf3b429c`
* **Source:** Official Red Canary Repository
* **Platform / Executor:** Windows / PowerShell execution frame
* **Exact Command / Arguments:** `powershell.exe -Command Write-Output "Atomic Test"` (Passed as a Base64 string via `-EncodedCommand`)
* **Prerequisites / Admin Required:** None / No
* **Artifacts / Network Activity:** None (Volatile memory processing) / None
* **Cleanup Command:** None required; completely volatile string processing leaving no file fragments
* **Expected Telemetry:** Sysmon Event ID 1 containing explicit `-EncodedCommand` or `-e` command-line runtime flags
* **Risk Assessment:** Low risk; decodes into a completely harmless text stream without hidden network calls.

---


## Approval Rule

A test is approved only when its current local definition matches the low-risk objective documented for this project.
