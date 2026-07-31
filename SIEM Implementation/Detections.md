# Detection Library

---

## Detection 1

_Name:_ 
PowerShell Execution

_Purpose:_ 
Identify PowerShell activity.

_SPL:_
index=main powershell

_Why It Matters_
PowerShell is frequently abused by attackers for execution, persistence, and post-exploitation.


---

## Detection 2

_Name:_ 
Successful & failed Logons

_Purpose:_ 
Review and identify successful & failed authentication events.

_SPL:_
index=main EventCode=4624 AND 4625


---

## Detection 3

_Name:_ 
Process Creation

_Purpose:_ 
Review Sysmon process creation events

_SPL:_
index=main id=1
