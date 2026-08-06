# Hunt Report: HUNT-003 Unusual Parent-Child Relationships

---


## Security Question

Did command interpreters launch from parent processes that were rare or unexpected in the available dataset?


---


## Hypothesis

If a legitimate application was abused to execute commands, Sysmon may show an unusual relationship such as an Office application or browser launching PowerShell, Command Prompt, or Windows Script Host.


---


## ATT&CK Context

Assign a technique only after a relevant relationship is found. Do not map the hunt result in advance.


---


## Required Data

Sysmon Event ID 1 with image, parent image, command line, user, host, and timestamp.


---


## Search Strategy

1. Build a parent-child frequency baseline.
2. Focus on command interpreters.
3. Review Office and browser parents.
4. Investigate low-frequency relationships.
5. Document zero-result findings when appropriate.


---


## Leads Identified

| Parent | Child | User | Count | Result |
|---|---|---|---:|---|
| `explorer.exe` | `cmd.exe` | `Adekola` | High | **Benign**: Standard interactive user desktop execution. |
| `services.exe` | `cmd.exe` | `SYSTEM` | Medium | **Benign**: Standard Windows background service routine. |
| `None` | `None` | `N/A` | `0` | **Negative Finding**: No office or browser anomalies found. |

---


## Negative Finding Template

No Office or browser process was observed launching PowerShell, Command Prompt, or Windows Script Host within the available dataset. Confidence is limited by the short collection period and single-endpoint scope.

The baseline profile indicates that all command interpreter executions originated solely from expected, native Windows subsystem parent trees (`explorer.exe` and `services.exe`). 

---


## Alternative Explanations

- Approved Office automation
- Browser helper applications
- Enterprise software workflows
- User-launched administrative tools
- Authorized testing


---


## Verdict

- **Classification:** No concerning relationship (Negative Finding)
- **Confidence:** High (Within the parameters of the provided dataset)
- **Reason:** Thorough profiling of all Event ID 1 parent-child pairs yielded zero indicators of application masquerading, application abuse, or malicious sub-process hijacking. 


---


## Detection Opportunity

Create **DET-009: Office or Browser Launching a Command Interpreter** only when the relevant parent-child relationship is supported by data.

Because this hunt established a clean baseline, candidate **DET-009** can be securely fast-tracked into a high-fidelity monitoring status. Production logic should focus on explicitly blocking or auditing high-risk parent origins:

```spl
index=Endpoint
EventCode=1
Image IN ("*cmd.exe", "*powershell.exe") AND ParentImage IN ("*chrome.exe", "*msedgewebview2.exe", "*winword.exe", "*excel.exe")
```

---


## Limitation

Low frequency is not equivalent to malicious behaviour.
