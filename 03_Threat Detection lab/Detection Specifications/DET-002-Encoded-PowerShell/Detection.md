# DET-002: Encoded PowerShell Execution

## Summary

Identifies PowerShell process creation containing `-EncodedCommand` or `-enc` arguments and extracts fields needed for triage.


---



## Status

| Field | Value |
|---|---|
| Status | Validated |
| Version | 3.0 |
| Severity | Medium |
| Confidence | Medium |
| Data source | Sysmon |
| Event ID | 1 |
| ATT&CK | T1059.001 PowerShell |



---



## Detection Hypothesis

PowerShell with encoded-command arguments may indicate command obfuscation and should be reviewed. The behaviour can also occur in legitimate administration and software deployment.


---


## Detection Logic

1. Select Sysmon process creation events.
2. Require PowerShell in the message.
3. Match `-EncodedCommand` or `-enc`.
4. Extract user, command line, image, and parent process.
5. Prioritize unusual Office parent processes.

Query: [Query.spl](https://github.com/Adeconcept/Home-SOC-Lab/blob/76b5bd8a49933fcc9f316349f91e9df478d75eba/03_Threat%20Detection%20lab/Detection%20Specifications/DET-002-Encoded-PowerShell/Query.spl)


---


## Decision Record

| Decision | Why | Value |
|---|---|---|
| Do not alert on all PowerShell | PowerShell is common administrative tooling | Reduces excessive noise |
| Match `-EncodedCommand` and `-enc` | More precise indicators of encoded execution | Improves detection focus |
| Do not use broad `-e` matching | It may match unrelated arguments | Avoids preventable false positives |
| Extract parent process | Parent context changes investigation priority | Supports faster triage |
| Raise priority for Office parent processes | Office spawning PowerShell is less expected | Helps analysts focus on unusual execution chains |
| Keep base severity Medium | Encoding alone does not prove malicious intent | Maintains balanced classification |


---




## Known False Positives

- Administrative automation
- Software deployment systems
- Endpoint management tooling
- Login or startup scripts
- Security products
- Approved testing
- Base64 used for data handling rather than concealment


---



## Limitations

- Message-based matching depends on command-line visibility.
- Attackers can use other obfuscation methods.
- Legitimate tools may use encoded commands.
- Parent-process priority does not prove malicious activity.

## Evidence

![Query](https://github.com/Adeconcept/Home-SOC-Lab/blob/76b5bd8a49933fcc9f316349f91e9df478d75eba/03_Threat%20Detection%20lab/Screenshots/11_DET_002_encoded.png)

*Figure 1. SPL query for event ID 1 with Powershell econded command.*




