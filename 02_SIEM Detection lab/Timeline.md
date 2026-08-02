# Investigation Timeline Methodology

> **Project:** SIEM Detection Lab: Windows Event Investigation with Splunk
>
> **Purpose:** Define how security events were reconstructed into chronological timelines to support evidence-based investigations.

---

## Why Timelines Matter

Individual security events rarely provide enough context to determine what actually happened on a system.

A single failed logon, PowerShell execution, or DNS request may appear suspicious when viewed in isolation. However, when these events are placed into chronological order, they begin to tell a coherent story.

Timeline reconstruction allows analysts to answer questions such as:

* What happened first?
* Which event triggered the next?
* Did multiple events belong to the same activity?
* Does the sequence support the initial hypothesis?
* Is the observed behaviour consistent with normal user activity?

Building accurate timelines is one of the most important skills in security investigations because it transforms isolated log entries into a complete narrative.

---

## Timeline Construction Methodology

Each investigation followed the same timeline development process.

```text
Identify Alert

↓

Collect Relevant Events

↓

Normalize Timestamps

↓

Sort Events Chronologically

↓

Correlate Related Events

↓

Validate Against Test Notes

↓

Analyze Event Sequence

↓

Draw Investigation Conclusions
```

This structured process ensured that every timeline was built consistently and supported by evidence.

---

## Timeline Components

Every timeline included the following information where available.

| Field        | Purpose                                |
| ------------ | -------------------------------------- |
| Timestamp    | Determine when activity occurred       |
| Event Source | Identify where the evidence originated |
| Event ID     | Classify the activity                  |
| User Account | Associate activity with an identity    |
| Host         | Identify the affected endpoint         |
| Process      | Determine which application executed   |
| Description  | Explain why the event matters          |

---

## Timeline Confidence

Not every timeline is complete.

The quality of an investigation depends on the completeness of the available telemetry.

Each timeline should therefore be interpreted alongside a confidence rating.

| Confidence | Meaning                                                                |
| ---------- | ---------------------------------------------------------------------- |
| High       | Multiple independent log sources support the event sequence.           |
| Medium     | Timeline is supported by available logs but lacks complete visibility. |
| Low        | Significant telemetry is unavailable or incomplete.                    |

For this project:

* Case 001: High
* Case 002: High
* Case 003: Medium

---

## Case 001 Timeline

### Investigation

Repeated Failed Logons

#### Objective

Determine whether repeated authentication failures represented user error or potential brute-force activity.

| Time  | Event            | Source                  | Observation                             |
| ----- | ---------------- | ----------------------- | --------------------------------------- |
| 09:42 | Failed Logon     | Windows Security (4625) | First authentication failure recorded.  |
| 09:43 | Failed Logon     | Windows Security (4625) | Same account generated another failure. |
| 09:44 | Failed Logon     | Windows Security (4625) | Third consecutive failure observed.     |
| 09:45 | Successful Logon | Windows Security (4624) | User authenticated successfully.        |

#### Timeline Analysis

The sequence showed several failed authentication attempts followed immediately by a successful logon using the same account.

No additional hosts were involved, no account lockout occurred, and the sequence matched the documented laboratory testing.

The evidence supports a **Benign Positive** classification.

---

## Case 002 Timeline

### Investigation

PowerShell Execution

#### Objective

Determine whether observed PowerShell activity represented normal administration or suspicious execution.

| Time  | Event             | Source            | Observation                      |
| ----- | ----------------- | ----------------- | -------------------------------- |
| 10:15 | User Logon        | Windows Security  | Interactive session established. |
| 10:16 | explorer.exe      | Sysmon Event ID 1 | Windows shell launched.          |
| 10:17 | powershell.exe    | Sysmon Event ID 1 | PowerShell executed.             |
| 10:18 | Command Execution | Sysmon            | Lab command executed.            |

#### Timeline Analysis

PowerShell execution occurred immediately after an interactive user session.

The parent-child process relationship indicated that `explorer.exe` launched `powershell.exe`, which is consistent with expected user behaviour.

The command execution matched the documented laboratory activity.

No evidence suggested persistence, privilege escalation, or post-exploitation.

---

## Case 003 Timeline

### Investigation

Process, DNS and Network Activity

#### Objective

Determine whether endpoint process execution resulted in unusual DNS or network behaviour.

| Time  | Event              | Source            | Observation                      |
| ----- | ------------------ | ----------------- | -------------------------------- |
| 11:03 | Process Creation   | Sysmon Event ID 1 | Application started.             |
| 11:04 | DNS Query          | Sysmon DNS        | Domain successfully resolved.    |
| 11:05 | Network Connection | Sysmon Network    | Outbound connection established. |

#### Timeline Analysis

The observed sequence followed a typical application workflow:

Process Creation

↓

DNS Resolution

↓

Outbound Network Connection

The telemetry matched expected application behaviour within the controlled lab environment.

No anomalous communication patterns or unexpected destinations were identified.

---

## Event Correlation

Throughout the investigations, related events were correlated across multiple log sources.

| Windows Security | Sysmon           | Investigation Value                                 |
| ---------------- | ---------------- | --------------------------------------------------- |
| Event ID 4624    | Process Creation | Link authentication to process execution.           |
| Event ID 4625    | PowerShell       | Determine activity following failed authentication. |
| User Account     | DNS Activity     | Associate network requests with a user session.     |

Correlating multiple data sources provides stronger evidence than relying on a single event.

---

## Parent-Child Process Relationships

Understanding parent-child process relationships is an important part of endpoint investigations.

Example:

```text
explorer.exe
      │
      ▼
powershell.exe
```

The parent process provides valuable context.

For example:

Expected:

* `explorer.exe → powershell.exe`

Potentially suspicious:

* `winword.exe → powershell.exe`

The parent process alone does not determine malicious activity, but it is an important investigative clue.

---

## Authentication Sequence Analysis

Authentication events should be evaluated as sequences rather than isolated entries.

Example:

```text
4625

↓

4625

↓

4625

↓

4624
```

This sequence may indicate:

* User typing errors
* Recently changed passwords
* Laboratory testing
* Password guessing

The correct explanation depends on the surrounding evidence.

---

## DNS and Network Correlation

DNS events frequently precede outbound network connections.

Example:

```text
Application Started

↓

DNS Query

↓

IP Address Resolved

↓

Outbound Connection
```

Reviewing these events together helps analysts understand how applications interact with external resources.

---

## Common Timeline Pitfalls

Investigators should avoid:

* Ignoring timezone differences.
* Mixing local and UTC timestamps.
* Reviewing events outside the investigation window.
* Assuming chronological order without validating timestamps.
* Building conclusions from incomplete telemetry.

Recognizing these pitfalls improves the reliability of timeline reconstruction.

---

## Timeline Best Practices

The following practices were used throughout this project:

* Normalize timestamps before analysis.
* Correlate events across multiple log sources.
* Keep the investigation window focused.
* Validate event sequences against laboratory test notes.
* Document assumptions and limitations.
* Support conclusions with evidence rather than isolated events.

---

## Lessons Learned

Timeline reconstruction proved to be one of the most valuable investigation techniques used during this project.

Rather than focusing on individual events, arranging telemetry into a chronological sequence made it easier to understand user activity, correlate evidence across multiple data sources, and validate investigation hypotheses.

This approach reinforced an important lesson: security events are most meaningful when viewed as part of a larger sequence rather than as isolated indicators.

The techniques documented here will continue to be applied in future projects involving threat hunting, detection engineering, and incident response.
