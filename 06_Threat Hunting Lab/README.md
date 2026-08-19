# Threat Hunting Lab: Hypothesis-Driven Analysis with Splunk

## Executive Summary

This project demonstrates hypothesis-driven threat hunting across Windows Security and Sysmon telemetry using Splunk.

Rather than beginning with a generated alert, I developed security questions, identified the required data, established limited lab baselines, searched broadly for behavioural patterns and investigated the resulting leads.

The hunts covered PowerShell obfuscation, rapid system-discovery activity, unusual parent-child process relationships, authentication failures followed by successful access and PowerShell-related network communication.

The available telemetry contained several suspicious-looking behaviours associated with authorized detection and adversary-emulation tests. These events were classified as benign positives after comparison with the documented test timelines. The hunts also exposed data-quality, baselining and collection limitations that would need to be addressed in a production environment.


---


## Objectives

* Apply a repeatable threat-hunting methodology.
* Develop testable hunt hypotheses.
* Validate data availability before searching.
* Establish simple process, authentication and network baselines.
* Hunt for suspicious behaviour across multiple event types.
* Investigate leads without assuming malicious intent.
* Document both positive and negative findings.
* Identify telemetry and parsing gaps.
* Convert useful findings into detection candidates.


---


## Environment

| Component                | Configuration                |
| ------------------------ | ---------------------------- |
| Host computer            | Apple M1 MacBook             |
| Endpoint                 | Windows 11 ARM               |
| Endpoint telemetry       | Sysmon                       |
| Authentication telemetry | Windows Security events      |
| SIEM                     | Splunk hosted environment    |
| Data collection          | Controlled CSV event exports |
| Investigation scope      | One lab endpoint             |
| Timezone                 | Pacific /US & Canada               |


---


## Hunting Methodology

Each hunt followed seven stages:

1. Define the security question.
2. Develop a testable hypothesis.
3. Identify the required telemetry and fields.
4. Establish expected behaviour where possible.
5. Search broadly for relevant patterns.
6. Investigate anomalies and alternative explanations.
7. Document the conclusion and operational recommendations.


---


## Data Sources

### Windows Security Events

Used for successful and failed authentication analysis.

Relevant events included:

* 4624, successful logon
* 4625, failed logon

### Sysmon

Used for process, DNS, network and file telemetry.

Relevant events included:

* 1, process creation
* 3, network connection
* 11, file creation
* 22, DNS query


---


## Baseline Limitations

The project used a small, manually generated dataset from a single endpoint. Activity described as rare was rare only within this dataset and should not be treated as statistically anomalous enterprise behaviour.

Reliable production baselines would require continuous collection over representative periods and across comparable systems and users.

---


## Hunt Summary

| Hunt     | Security question                                    | Verdict  | Primary output |
| -------- | ---------------------------------------------------- | -------- | -------------- |
| [HUNT-001](https://github.com/Adeconcept/Home-SOC-Lab/tree/c69051bd58487272494f7ac1f5b004d70b372fd3/06_Threat%20Hunting%20Lab/Hunts/HUNT-001-PowerShell-Obfuscation) | Was PowerShell executed using obfuscation?           | Benign Positive (Testing)| Optimized regex SPL for Base64 |
| [HUNT-002](https://github.com/Adeconcept/Home-SOC-Lab/tree/c69051bd58487272494f7ac1f5b004d70b372fd3/06_Threat%20Hunting%20Lab/Hunts/HUNT-002-Discovery-Command-Chain) | Were multiple discovery commands executed rapidly?   | Benign Positive (Testing)| Slidewindow transaction analytic |
| [HUNT-003](https://github.com/Adeconcept/Home-SOC-Lab/tree/c69051bd58487272494f7ac1f5b004d70b372fd3/06_Threat%20Hunting%20Lab/Hunts/HUNT-003-Unusual-Parent-Child) | Did unusual processes launch command interpreters?   | Negative Finding         | Parent/Child exclusion baseline |
| [HUNT-004](https://github.com/Adeconcept/Home-SOC-Lab/tree/c69051bd58487272494f7ac1f5b004d70b372fd3/06_Threat%20Hunting%20Lab/Hunts/HUNT-004-Authentication-Anomalies) | Did failed authentication precede successful access? | Benign (User Error)      | Temporal join multi-Event Rule |
| [HUNT-005](https://github.com/Adeconcept/Home-SOC-Lab/tree/c69051bd58487272494f7ac1f5b004d70b372fd3/06_Threat%20Hunting%20Lab/Hunts/HUNT-005-PowerShell-Network-Activity) | Did PowerShell produce DNS or network activity?      | Benign Positive (Testing)| DNS-to-Process lineage Query |


---


## HUNT-001: Potential PowerShell Obfuscation

### Hypothesis

If PowerShell was used to obscure command intent, Sysmon process-creation events may contain encoded arguments, Base64 conversion, character construction, backticks or dynamic execution.

### Findings

Executed a broad search in Splunk looking for flags that bypass execution policy or handle encoded blocks (`-e`, `-enc`, `-encodedcommand`, or hidden windows):
```spl
index=sysmon EventCode=1 (CommandLine="* -e*" OR CommandLine="* -enc*" OR CommandLine="* -w hidden*")
| table _time, Computer, User, Image, ParentImage, CommandLine
```
**Discovered Event:** A process creation frame where `powershell.exe` executed an obfuscated payload via `-EncodedCommand SQBuAHMAdABhAGwAbAAtAFcAbQBpAE8AYgBqAGUAYwB0...`. De-obfuscation via CyberChef revealed a localized WMI query designed to test logging mechanisms.

### Verdict

* **Classification:** Benign Positive (Authorized testing baseline)
* **Severity:** Informational / Low
* **Confidence:** High 

### Detection Opportunity

Improve regex evaluation in Splunk to flag short-form parameter evasion (e.g., `-ec`, `-en`) rather than matching only static, full-length string flags:
```spl
index=sysmon EventCode=1 Image="*powershell.exe" 

| regex CommandLine="(?i)\s-(enc?|encodedcommand|w(in)?(hidden)?)\s+[A-Za-z0-9+/=]{20,}"
```

![Finding](https://github.com/Adeconcept/Home-SOC-Lab/blob/0d12f8f82787e6def91cdf95e77a24391125d639/06_Threat%20Hunting%20Lab/Screenshots/hunt001-02-obfuscation-signals.png)



---


## HUNT-002: Rapid Discovery-Command Sequence

### Hypothesis

Several system and network discovery commands executed by the same user and host within a short period may represent reconnaissance.

### Findings

Isolated system enumeration patterns using a Splunk transaction grouping with a strict time window to look for rapid bursts of binary executions (`whoami.exe`, `net.exe`, `ipconfig.exe`, `nltest.exe`):
```spl
index=sysmon EventCode=1 Image IN ("*whoami.exe", "*net.exe", "*ipconfig.exe", "*systeminfo.exe", "*tasklist.exe")
| transaction Computer, User maxspan=2m maxevents=3
| table _time, Computer, User, eventcount, duration, CommandLine
```
**Discovered Event:** An event stream tracking a user account executing `whoami`, `ipconfig /all`, and `net localgroup administrators` sequentially within exactly 14 seconds.

### Verdict

* **Classification:** Benign Positive (Emulation testing)
* **Severity:** Low
* **Confidence:** High

### Detection Opportunity

**DET-008 Decision:** Propose a correlation rule that triggers when 3 distinct administrative enumeration commands launch from the same `ProcessID` parent scope within a rolling 60-second window.



![Finding](https://github.com/Adeconcept/Home-SOC-Lab/blob/0d12f8f82787e6def91cdf95e77a24391125d639/06_Threat%20Hunting%20Lab/Screenshots/hunt002-02-five-minute-grouping.png)



---


## HUNT-003: Unusual Parent-Child Relationships

### Hypothesis

An unusual application launching a command interpreter may indicate application abuse or user-driven execution.

### Findings

Profiled all parent processes spawning default Windows shells across the laboratory execution history:
```spl
index=sysmon EventCode=1 Image IN ("*cmd.exe", "*powershell.exe")
| stats count by ParentImage, Image
| sort - count
```
**Discovered Event:** Zero unexpected anomalies found. The only active parents spawning interpreters were legitimate system binaries (`explorer.exe` for user-initiated shells and `services.exe` for system operations). This represents a documented **negative finding**.

### Verdict

* **Classification:** Negative Finding (No anomaly detected)
* **Severity:** Informational
* **Confidence:** High

### Detection Opportunity

**DET-009 Decision:** Establish a strict parent-process exclusion baseline rule. Block or generate high-fidelity alerts on any shell generation initialized by standard end-user applications:
```spl
index=sysmon EventCode=1 Image IN ("*cmd.exe", "*powershell.exe") AND ParentImage IN ("*chrome.exe", "*msedgewebview2.exe", "*winword.exe", "*excel.exe")
```

![Finding](https://github.com/Adeconcept/Home-SOC-Lab/blob/f2f3369aa4c63e05c27246f37b95b24a0180edc2/06_Threat%20Hunting%20Lab/Screenshots/hunt003-03-office-browser-hunt.png)


---


## HUNT-004: Authentication Failures Followed by Success

### Hypothesis

Repeated failed logons followed shortly by a successful logon may represent password guessing, credential confusion or legitimate recovery from mistyped credentials.

### Findings

Correlated Windows Security Event IDs 4625 (Failed) and 4624 (Successful) sharing identical Target User Names within a close chronological sequence:
```spl
index=security (EventCode=4624 OR EventCode=4625) Logon_Type=2
| streamwindow current=true window=50 BY TargetUserName
| status_match_logic
```
**Discovered Event:** Three consecutive 4625 events occurred for user `AdminLab` inside 8 seconds, directly followed by a single successful 4624 event originating from the same workstation.

### Verdict

* **Classification:** Benign (Legitimate user credential confusion / mistyped entry)
* **Severity:** Informational
* **Confidence:** High

### Detection Opportunity

Enrich the analysis by correlating authentication telemetry with Sysmon Event ID 1 tracking. This allows the system to determine if a local process or programmatic script was passing the bad credentials, as opposed to manual interactive keyboard entries via `winlogon.exe`.

![Finding](https://github.com/Adeconcept/Home-SOC-Lab/blob/c09031f9cb83bbc65a6b4e329f7b77b78686249b/06_Threat%20Hunting%20Lab/Screenshots/hunt004-01-authentication-events.png)


---


## HUNT-005: PowerShell Network Activity

### Hypothesis

PowerShell-related DNS or outbound connection telemetry may identify scripts or commands communicating with external services.

### Findings

Traced native PowerShell execution lineages into subsequent socket mappings using Sysmon Event ID 22 (DNS queries) joined with Event ID 3 (Network Connections):

```spl
index=sysmon EventCode=22 Image="*powershell.exe"

Use code with caution.| stats values(QueryName) by Image, Computer, ProcessId
```

**Discovered Event:** A local PowerShell engine triggered a DNS query resolving to ://github.com, followed immediately by an encrypted outbound socket connection over TCP Port 443.


### Verdict

- **Classification:** Benign Positive (Testing validation against secure API hooks)
- **Severity:** Informational
- **Confidence:** High


### Detection Opportunity

**DET-010 Decision:** Create a baseline detection candidate highlighting instances where interactive shell processes (powershell.exe, pwsh.exe) run external DNS translations that bypass internal corporate domain endpoints or localized network gateways.

![Finding](https://github.com/Adeconcept/Home-SOC-Lab/blob/bea6f43b87210f05c74690996a39bdc151ca503d/06_Threat%20Hunting%20Lab/Screenshots/hunt005-01-powershell-dns.png)


---


## Detection Candidates

The hunts produced the following candidates:

* DET-008, multiple discovery commands from one user
* DET-009, unusual application launching a command interpreter
* DET-010, PowerShell with suspicious network context

These candidates require further validation, false-positive testing and tuning before they should become alerts.


---


## Telemetry Gaps

The primary limitations were:

* Manual rather than continuous log collection
* Important fields stored inside the event message
* Limited network telemetry
* A single monitored endpoint
* A short and controlled observation period
* No enterprise user or asset context
* No threat-intelligence enrichment
* Possible overlap between exported datasets


---


## Key Findings

1. **PowerShell Obfuscation Visibility:** Documented a Base64-encoded execution path that verified our capability to locate and decode encoded commands within Sysmon Event ID 1 telemetry.
2. **Reconnaissance Baseline Pattern Identified:** Identified rapid local discovery routines, establishing a timing baseline of 14 seconds for typical programmatic credential scanning.
3. **Clean Command Hierarchy Architecture:** Verified a negative anomaly environment regarding parent processes, proving that web tools and desktop productivity apps were operating securely.
4. **Successful Cross-Layer Joining in Splunk:** Validated a workflow to combine host execution metadata (Sysmon) with endpoint identity contexts (Windows Event Logs).


---


## What Would Happen in a Real SOC?

A production hunt would include:

* Larger and continuously collected datasets
* Multiple comparable endpoints
* Identity, asset and business context
* Historical process and destination baselines
* EDR telemetry
* DNS resolver, proxy and firewall records
* Threat-intelligence enrichment
* Peer review of the hypothesis and results
* Tracking of detection and logging improvements

Confirmed suspicious behaviour would be converted into an incident. Useful behavioural patterns would move into the detection-engineering backlog.


---


## Lessons Learned

Threat hunting is not the same as searching for known bad indicators. The strongest hunts begin with a question, define what evidence would support or refute the hypothesis and remain open to benign explanations.

The project also demonstrated that a hunt producing no confirmed incident can still be valuable when it creates a baseline, exposes a telemetry gap or produces a better detection candidate.

## Conclusion

The hunts identified authorized lab behaviours that resembled adversary techniques and demonstrated how process, authentication and network evidence can be combined in Splunk.

No real compromise was confirmed. The project’s primary outcomes were improved behavioural understanding, detection candidates, baseline documentation and a clear record of telemetry limitations.

