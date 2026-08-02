# Incident Report: PowerShell Execution Investigation

> **Project:** SIEM Detection Lab: Windows Event Investigation with Splunk
>
> **Case ID:** SOC-2026-CASE-002

---

# Case Summary

| Field                | Value                                 |
| -------------------- | ------------------------------------- |
| Case ID              | SOC-2026_CASE-002                     |
| Investigation        | PowerShell Execution                  |
| Status               | Closed                                |
| Classification       | Benign Positive                       |
| Severity             | Low                                   |
| Confidence           | High                                  |
| Analyst              | Adekola Durodola                      |
| Investigation Window | 20 Minutes                            |
| Data Sources         | Sysmon, Windows Security Logs, Splunk |
| Primary Event        | Sysmon Event ID 1                     |
| MITRE ATT&CK         | T1059.001 – PowerShell (Observed)     |
| Last Updated         | July 2026                             |

---

# Executive Summary

During routine review of endpoint telemetry, Sysmon recorded the execution of **PowerShell**. Because PowerShell is widely used by both system administrators and attackers, every execution should be evaluated within its operational context rather than assumed to be malicious.

The objective of this investigation was to determine whether the observed PowerShell activity represented legitimate administrative use or behaviour requiring escalation.

Using Sysmon process creation telemetry together with Windows Security events, the investigation reconstructed the execution timeline, reviewed parent-child process relationships, examined the command-line arguments, and correlated the activity with documented laboratory testing.

The investigation concluded that the PowerShell execution was initiated by the logged-in laboratory user, matched the documented test activities, and did not exhibit behaviours that would justify escalation.

---

# Investigation Objective

Determine whether the recorded PowerShell execution represents:

* Normal administrative activity
* Laboratory testing
* Automation
* Suspicious execution
* Potential post-exploitation

---

# Alert

A Sysmon **Event ID 1 (Process Creation)** recorded the execution of **powershell.exe**.

PowerShell is commonly monitored because it is capable of executing scripts, interacting with the operating system, and automating administrative tasks. These same capabilities also make it attractive to attackers.

---

# Initial Hypothesis

Several explanations were considered before reviewing the evidence.

Possible causes included:

* Manual administrative activity
* User-initiated PowerShell session
* Laboratory testing
* Automated script execution
* Post-exploitation activity

No hypothesis was accepted until supported by telemetry.

---

# Investigation Scope

| Field                | Value                |
| -------------------- | -------------------- |
| Host                 | Windows 11 ARM       |
| User                 | Controlled Lab User  |
| Investigation Window | 20 Minutes           |
| Event Reviewed       | Sysmon Event ID 1    |
| Supporting Logs      | Windows Security Log |
| SIEM                 | Splunk               |

---

# Investigation Methodology

```text id="r2c5mf"
PowerShell Alert

↓

Review Sysmon Event ID 1

↓

Identify Parent Process

↓

Review Command Line

↓

Correlate Authentication Events

↓

Build Timeline

↓

Validate Against Lab Activity

↓

Determine Investigation Outcome
```

---

# Evidence Collected

| Time  | Source           | Event            | Relevance                            |
| ----- | ---------------- | ---------------- | ------------------------------------ |
| 10:15 | Windows Security | Event ID 4624    | User authenticated successfully      |
| 10:16 | Sysmon           | Process Creation | explorer.exe started                 |
| 10:17 | Sysmon           | Process Creation | powershell.exe executed              |
| 10:18 | Sysmon           | Command Line     | User executed documented lab command |

---

# SPL Queries

## Query 1

**Question**

Was PowerShell executed?

```spl id="4q2z4v"
index=main powershell
```

---

## Query 2

**Question**

Which process launched PowerShell?

```spl id="jlwm6r"
index=main EventCode=1 powershell
| table _time ParentImage Image CommandLine User
```

---

## Query 3

**Question**

Did authentication occur before PowerShell execution?

```spl id="d7g0tt"
index=main (EventCode=4624 OR EventCode=1)
| sort _time
```

---

# Timeline

```text id="es2z7w"
10:15

Successful Logon

↓

10:16

explorer.exe Started

↓

10:17

powershell.exe Executed

↓

10:18

PowerShell Command Executed
```

---

# Timeline Analysis

The timeline shows that PowerShell execution occurred immediately after a successful interactive user logon.

The parent process was **explorer.exe**, indicating that PowerShell was launched from the user's desktop session rather than by another application or background service.

The execution time matched the documented laboratory testing schedule.

No additional PowerShell instances, unusual execution chains, or suspicious follow-on activity were observed.

---

# Parent-Child Process Analysis

One of the most valuable pieces of Sysmon telemetry is the parent-child relationship between processes.

Observed sequence:

```text id="hn4gqm"
explorer.exe

↓

powershell.exe
```

This relationship is consistent with an interactive user launching PowerShell manually.

Examples that would require additional investigation include:

```text id="77tezc"
winword.exe

↓

powershell.exe
```

or

```text id="khl08y"
excel.exe

↓

powershell.exe
```

These chains can indicate malicious macro execution or user deception and would typically warrant escalation.

---

# Command-Line Analysis

The command-line arguments associated with the PowerShell process were reviewed.

The observed command matched the documented laboratory activity.

If an encoded command had been observed, additional investigation would have been required because encoded execution reduces immediate readability and is frequently monitored by detection teams.

However, encoded execution alone is **not proof of malicious activity**. Analysts must decode the command, review its contents, and correlate it with surrounding telemetry before reaching a conclusion.

---

# Event Correlation

The investigation correlated multiple telemetry sources.

| Source            | Purpose                                     |
| ----------------- | ------------------------------------------- |
| Windows Security  | Confirm interactive user authentication     |
| Sysmon Event ID 1 | Confirm process creation                    |
| Command Line      | Determine executed action                   |
| Timeline          | Correlate execution with documented testing |

Using multiple evidence sources increased confidence in the investigation outcome.

---

# Evidence Confidence Matrix

| Evidence                | Confidence | Reason                        |
| ----------------------- | ---------- | ----------------------------- |
| Sysmon Process Creation | High       | Native endpoint telemetry     |
| Windows Security Log    | High       | Confirmed user authentication |
| Command-Line Data       | High       | Captured directly by Sysmon   |
| Laboratory Test Notes   | High       | Matched execution timeline    |

---

# Analyst Decision Log

| Decision                         | Reason                                |
| -------------------------------- | ------------------------------------- |
| Reviewed parent process          | Determine how PowerShell was launched |
| Correlated authentication events | Confirm user context                  |
| Examined command line            | Validate executed action              |
| Compared timeline with lab notes | Verify authorised testing             |
| Closed investigation             | Evidence supported benign activity    |

---

# Analysis

PowerShell is one of the most frequently monitored applications in enterprise environments because it provides extensive administrative capabilities and is commonly abused during post-exploitation.

In this investigation, however, the available telemetry supports normal user activity.

The evidence demonstrated that:

* PowerShell was launched after a successful interactive logon.
* The parent process was `explorer.exe`, indicating manual user execution.
* The recorded command matched documented laboratory testing.
* No suspicious parent process relationships were observed.
* No persistence mechanisms or follow-on malicious activity were identified.

Although PowerShell execution is an important detection signal, the surrounding evidence did not support malicious behaviour.

---

# MITRE ATT&CK Context

## Technique

**T1059.001 – PowerShell**

PowerShell execution is commonly associated with adversary command and scripting activity.

Within this investigation, the technique was **observed** but not **maliciously exercised**.

The MITRE mapping reflects the capability demonstrated by the telemetry rather than confirmation of attacker behaviour.

---

# Detection Opportunities

The investigation identified several opportunities to improve future detections.

Potential Splunk alerts include:

* Encoded PowerShell commands
* PowerShell launched by Microsoft Office applications
* PowerShell spawned by script interpreters
* PowerShell executing from unusual directories
* High-frequency PowerShell execution within short time windows

These detections should always be validated through investigation before escalation.

---

# Investigation Outcome

| Field          | Result          |
| -------------- | --------------- |
| Classification | Benign Positive |
| Severity       | Low             |
| Confidence     | High            |
| Status         | Closed          |

---

# Recommended Action

No incident response action is required.

Continue monitoring PowerShell activity and investigate future executions that exhibit:

* Suspicious parent-child relationships
* Encoded command-line arguments
* Unusual execution paths
* Unexpected execution frequency
* Correlation with additional suspicious telemetry

---

# Limitations

The investigation was performed within a controlled laboratory environment.

The following evidence sources were not available:

* Endpoint Detection and Response (EDR)
* PowerShell Script Block Logging
* AMSI telemetry
* Active Directory
* Network proxy logs

While these sources would improve visibility in a production environment, they were not required to answer the investigation objective for this lab.

---

# Lessons Learned

This investigation reinforced that PowerShell is a powerful administrative tool whose presence alone should never be interpreted as malicious.

The most valuable lesson was learning how parent-child process relationships, command-line arguments, and user context provide the additional evidence needed to distinguish expected administrative activity from behaviour that may require escalation.

The investigation also highlighted the importance of approaching every alert with an open hypothesis and allowing the available telemetry to guide the final conclusion.

---

# Skills Demonstrated

* PowerShell Investigation
* Sysmon Analysis
* Process Creation Analysis
* Parent-Child Process Investigation
* Windows Event Correlation
* Splunk Investigation
* Timeline Reconstruction
* MITRE ATT&CK Mapping
* Evidence-Based Analysis

---

# Screenshots


![PowerShell process creation](Screenshots/case_002_01_powershell_process_events.png)

*Figure 1. Sysmon Event ID 1 showing PowerShell process creation during the documented laboratory activity.*

---

![PowerShell command-line details](Screenshots/case_002_02_encoded_command.png)

*Figure 2. Command-line arguments associated with the PowerShell process. Command-line analysis provides valuable context but should always be interpreted alongside additional evidence.*

---

![Parent-child process relationship](Screenshots/case_002_03_parent_process_analysis.png)

*Figure 3. Parent-child process relationship demonstrating explorer.exe launching powershell.exe, consistent with expected interactive user activity.*


---

# Reviewer Notes

PowerShell is frequently associated with attacker activity because of its flexibility and administrative capabilities. However, this investigation demonstrates the importance of avoiding conclusions based solely on process names.

The observed execution matched authorised laboratory testing, the process lineage was consistent with expected user behaviour, and no additional telemetry suggested malicious intent.

This case reinforces a fundamental SOC principle: **investigations should be driven by evidence, not assumptions**.
