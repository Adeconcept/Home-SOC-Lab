# Detection Library

> **Project:** SIEM Detection Lab: Windows Event Investigation with Splunk
>
> **Purpose:** Document the detection logic, investigation intent, and analyst guidance developed during this lab.

---

## Overview

A detection is not evidence of an attack.

A detection is an indication that activity matching a predefined pattern has occurred.

The analyst's responsibility is to determine whether that activity represents:

* Normal behaviour
* Misconfiguration
* Benign administrative activity
* Suspicious behaviour
* Confirmed malicious activity

The detections below were developed using Windows Security Logs and Sysmon telemetry collected from the Home SOC Lab.

---

## Detection Summary

| Detection ID | Detection Name         | Data Source      | MITRE ATT&CK |
| ------------ | ---------------------- | ---------------- | ------------ |
| DET-001      | Repeated Failed Logons | Windows Security | T1110        |
| DET-002      | Successful Logons      | Windows Security | N/A          |
| DET-003      | PowerShell Execution   | Sysmon           | T1059.001    |
| DET-004      | Process Creation       | Sysmon           | T1059        |
| DET-005      | DNS Activity           | Sysmon DNS       | T1071        |
| DET-006      | Network Connections    | Sysmon Network   | T1071        |

---

## DET-001

### Detection Name

Repeated Failed Logons

---

### Objective

Identify repeated authentication failures that may indicate password guessing, brute-force activity, or user error.

---

### Data Source

Windows Security Log

Event ID 4625

---

### Threat Rationale

Repeated failed authentication attempts are common during:

* Incorrect password entry
* Password spraying
* Brute-force attacks
* Account enumeration

Although this behaviour may indicate malicious intent, it is not independently sufficient to confirm an attack.

---

### SPL Query

```spl
index=main EventCode=4625
| stats count by Account_Name
```

---

### Analyst Question

Which accounts experienced repeated failed authentication attempts?

---

### Expected Result

A list of accounts together with the number of failed logon events observed.

---

### Investigation Guidance

Review:

* Authentication timeline
* Source workstation
* Logon type
* User account
* Successful authentication immediately afterwards

---

### False Positive Considerations

Common causes include:

* Typing mistakes
* Forgotten passwords
* Recently changed passwords
* Laboratory testing

---

### MITRE ATT&CK

Technique

T1110

Brute Force

Potential only.

---

### Future Detection Improvement

Generate an alert when:

* Five failed logons occur within five minutes.

---

## DET-002

### Detection Name

Successful Logons

---

### Objective

Review successful authentication activity.

---

### Data Source

Windows Security

Event ID 4624

---

### Threat Rationale

Successful authentication may indicate:

* Normal user activity
* Privileged account access
* Lateral movement
* Compromised credentials

Context determines whether further investigation is required.

---

### SPL Query

```spl
index=main EventCode=4624
```

---

### Analyst Question

Which users successfully authenticated during the investigation window?

---

### Expected Result

List of successful logon events.

---

### Investigation Guidance

Review:

* User account
* Host
* Logon type
* Authentication package
* Time of authentication

---

### False Positives

Normal user logons.

---

### Future Detection Improvement

Correlate successful logons immediately following multiple failed logons.

---

## DET-003

### Detection Name

PowerShell Execution

---

### Objective

Identify PowerShell process execution.

---

### Data Source

Sysmon

Event ID 1

---

### Threat Rationale

PowerShell is a legitimate administration tool that is also commonly abused by attackers.

Execution alone does not indicate malicious activity.

Investigation should focus on:

* Command line
* Parent process
* User account
* Timeline

---

### SPL Query

```spl
index=main powershell
```

---

### Analyst Question

Was PowerShell executed?

---

### Expected Result

PowerShell process creation events.

---

### Investigation Guidance

Review:

* Parent process
* Command line
* User
* Timestamp
* Encoded commands
* Execution frequency

---

### False Positive Considerations

* System administration
* Scripting
* Automation
* Laboratory testing

---

### MITRE ATT&CK

Technique

T1059.001

PowerShell

Potential only.

---

### Future Detection Improvement

Alert when:

* Encoded commands are observed.
* PowerShell launches from Microsoft Office applications.
* PowerShell launches from scripting engines.

---

## DET-004

### Detection Name

Process Creation

---

### Objective

Review new process execution recorded by Sysmon.

---

### Data Source

Sysmon

Event ID 1

---

### Threat Rationale

Nearly every user action results in a process being created.

Understanding process relationships is essential during investigations.

---

### SPL Query

```spl
index=main EventCode=1
```

---

### Analyst Question

Which processes were created during the investigation?

---

### Expected Result

List of newly created processes.

---

### Investigation Guidance

Review:

* Parent-child relationships
* Process path
* User
* Command line
* Execution order

---

### False Positive Considerations

Normal operating system activity.

---

### Future Detection Improvement

Alert on:

* Office spawning PowerShell.
* CMD spawning PowerShell.
* Explorer spawning suspicious executables.

---

## DET-005

### Detection Name

DNS Activity

---

### Objective

Review DNS queries generated by the endpoint.

---

### Data Source

Sysmon DNS Events

---

### Threat Rationale

DNS activity often precedes outbound network communication.

Unexpected DNS requests may indicate:

* Malware communication
* Command and control
* Domain generation algorithms

---

### SPL Query

```spl
index=main dns
```

---

### Analyst Question

Which domains were queried?

---

### Expected Result

DNS query activity.

---

### Investigation Guidance

Review:

* Queried domain
* Frequency
* Associated process
* Timeline

---

### False Positive Considerations

Normal web browsing.

Software updates.

Cloud applications.

---

### Future Detection Improvement

Alert on:

* Newly observed domains.
* High-frequency DNS requests.
* Suspicious top-level domains.

---

## DET-006

### Detection Name

Network Connections

---

### Objective

Review outbound endpoint network activity.

---

### Data Source

Sysmon Network Events

---

### Threat Rationale

Network connections may reveal:

* Application communication
* Malware beaconing
* Remote administration
* External services

---

### SPL Query

```spl
index=main network
```

---

### Analyst Question

Which outbound connections occurred?

---

### Expected Result

Network connection events.

---

### Investigation Guidance

Review:

* Source process
* Destination
* Port
* Protocol
* Timeline

---

### False Positive Considerations

Normal browsing.

Software updates.

Windows services.

---

### Future Detection Improvement

Alert when:

* Connections are made to known malicious IP addresses.
* Rare ports are used.
* Unexpected processes establish external connections.

---

## Detection Engineering Principles

During this lab I learned several important detection engineering concepts.

A useful detection should:

* Answer a specific security question.
* Be based on reliable telemetry.
* Produce actionable results.
* Minimize false positives.
* Support investigations rather than replace them.

Most importantly, a detection is only the beginning of an investigation. Analysts must always validate alerts using additional evidence before drawing conclusions.

---

## Future Detection Roadmap

As this Home SOC Lab expands, these detections will be enhanced with:

* Sigma Rules
* Correlation Searches
* Risk-Based Alerting
* Detection Tuning
* MITRE ATT&CK Coverage Matrix
* Threat Hunting Queries
* Automated Response Workflows
