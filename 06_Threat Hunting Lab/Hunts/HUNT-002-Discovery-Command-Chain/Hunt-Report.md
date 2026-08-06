# Hunt Report: HUNT-002 Rapid Discovery-Command Sequence

---


## Security Question

Did one host execute several system and network discovery commands within a short period?


---


## Hypothesis

If an operator performed reconnaissance, several discovery utilities may execute from the same host and user within a narrow time window.


---


## MITRE ATT&CK Context

- T1082, System Information Discovery
- T1016, System Network Configuration Discovery


---


## Required Data

Sysmon Event ID 1 with timestamp, host, user, parent process, image, and command line.


---


## Expected Behaviour

Week 8 Atomic Red Team activity may create a known system and network discovery chain.


---


## Search Strategy

1. Identify relevant discovery commands.
2. Group distinct discovery types into five-minute windows.
3. Review user, parent, timing, and follow-on activity.
4. Compare the sequence with Week 8 execution notes.


---


## Threshold Decision

Two distinct discovery types within five minutes is a lab hunting threshold, not a production standard.


---


## Leads Identified

| Window | User | Discovery types | Commands | Result |
|---|---|---|---|---|
| Rolling 14-second window | `Adekola` | Host Identity, Local Architecture, Network Topology | `whoami`, `systeminfo`, `ipconfig` | Authorized Test (Emulation activity confirmed) |


---


## Alternative Explanations

- Help-desk troubleshooting
- Administrator diagnostics
- Deployment scripts
- VPN troubleshooting
- Authorized security testing


---


## Verdict

- **Classification:** Authorized test (Benign positive emulation)
- **Confidence:** High
- **Reason:** The activity exactly matches the documented framework execution matrix for tactical endpoint evaluation. The source parent shell originated within an explicit administrative workspace configuration.


---


## Detection Opportunity

**DET-008: Multiple Discovery Commands from One User**

Do not alert on a single `hostname` or `ipconfig` command.


---


## Recommended Action

Promote this pattern to the **detection engineering backlog as candidate DET-008**. Construct a correlation filter utilizing a sliding transaction index that flags an alert only if a single user context generates three or more unique enumeration utility commands inside a 60-second threshold block.
