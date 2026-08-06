# Detection Candidate Backlog

## DET-008: Multiple Discovery Commands from One User

### Hypothesis

Several distinct discovery utilities executed by one account on one host within five minutes may represent reconnaissance.

### Data

Sysmon Event ID 1

### Required Fields

Host, user, process, parent process, command line, and timestamp.

### Potential False Positives

Help-desk troubleshooting, administrator diagnostics, deployment scripts, and authorized security testing.

### ATT&CK

T1082 and T1016

### Status

Candidate, requires positive, negative, repeat, and threshold testing.

---

## DET-009: Office or Browser Launching a Command Interpreter

### Hypothesis

An Office application or browser launching PowerShell, Command Prompt, or Windows Script Host may indicate application abuse or user-driven execution.

### Data

Sysmon Event ID 1

### Potential False Positives

Approved Office automation, browser helper applications, and enterprise software workflows.

### Status

Create only when the parent-child relationship is supported by hunt evidence.

---

## DET-010: PowerShell with Suspicious Network Context

### Hypothesis

PowerShell network activity becomes higher priority when it follows encoded or obfuscated execution, uses an unusual destination, connects directly to an external IP, or creates a file afterward.

### Data

Sysmon Event IDs 1, 3, 11, and 22.

### Potential False Positives

Administrative downloads, software deployment, API automation, cloud-management modules, and authorized testing.

### Status

Candidate, requires destination baselining and correlation testing.

---

## Existing Detection Improvements

### DET-001

Consider successful login after repeated failures, source IP, logon type, multiple accounts, and post-authentication processes.

### PowerShell Detections

Keep encoded arguments, dynamic execution, character construction, Base64 decoding, and unusual parent-child context as separate tuning paths rather than one oversized rule.
