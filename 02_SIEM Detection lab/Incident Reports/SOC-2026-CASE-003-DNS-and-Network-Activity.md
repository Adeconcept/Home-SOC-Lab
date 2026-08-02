# Incident Report: Process, DNS and Network Activity Investigation

> **Project:** SIEM Detection Lab: Windows Event Investigation with Splunk
>
> **Case ID:** SOC-2026-CASE-003

---

# Case Summary

| Field                | Value                                           |
| -------------------- | ----------------------------------------------- |
| Case ID              | SOC-2026_CASE-003                               |
| Investigation        | Process, DNS and Network Activity               |
| Status               | Closed                                          |
| Classification       | Benign Positive                                 |
| Severity             | Low                                             |
| Confidence           | Medium                                          |
| Analyst              | Adekola Durodola                                |
| Investigation Window | 30 Minutes                                      |
| Data Sources         | Sysmon, Windows Security Logs, Splunk           |
| Primary Events       | Process Creation, DNS Query, Network Connection |
| MITRE ATT&CK         | T1071 (Potential), T1059 (Context Only)         |
| Last Updated         | July 2026                                       |

---

# Executive Summary

Applications rarely perform a single action in isolation. A user launches a process, that process resolves a domain through DNS, and then establishes one or more network connections. Viewed individually, each event appears ordinary. Viewed together, they reveal how an application behaves over time.

This investigation examined the relationship between process creation, DNS resolution, and outbound network activity recorded by Sysmon and analysed in Splunk.

The objective was to determine whether the observed sequence represented expected application behaviour or activity requiring escalation.

The investigation found that the process execution, DNS request, and subsequent network communication followed the documented laboratory testing and matched a normal application workflow. No indicators of persistence, lateral movement, or command-and-control behaviour were identified.

---

# Investigation Objective

Determine whether the observed endpoint activity represents:

* Expected application behaviour
* Legitimate network communication
* Software updates
* Command-and-control traffic
* Suspicious outbound connections

---

# Alert

Routine telemetry review identified process creation followed by DNS activity and outbound network communication.

Although this sequence is expected for many legitimate applications, the combination of events warranted review to ensure no suspicious behaviour was present.

---

# Initial Hypothesis

Several explanations were considered before analysing the evidence.

Possible causes included:

* Normal application startup
* Browser activity
* Operating system communication
* Software update
* Laboratory testing
* Suspicious command-and-control communication

The investigation remained neutral until supported by telemetry.

---

# Investigation Scope

| Field                | Value                                           |
| -------------------- | ----------------------------------------------- |
| Host                 | Windows 11 ARM                                  |
| User                 | Controlled Lab User                             |
| Investigation Window | 30 Minutes                                      |
| Primary Events       | Process Creation, DNS Query, Network Connection |
| Supporting Logs      | Windows Security Log                            |
| SIEM                 | Splunk                                          |

---

# Investigation Methodology

```text
Suspicious Activity

↓

Review Process Creation

↓

Identify Executable

↓

Review DNS Requests

↓

Review Network Connections

↓

Correlate Timeline

↓

Validate Against Test Notes

↓

Determine Investigation Outcome
```

---

# Evidence Collected

| Time  | Source           | Event         | Relevance                           |
| ----- | ---------------- | ------------- | ----------------------------------- |
| 11:03 | Sysmon           | Event ID 1    | Application process created         |
| 11:04 | Sysmon           | DNS Event     | Domain resolved successfully        |
| 11:05 | Sysmon           | Network Event | Outbound connection established     |
| 11:06 | Windows Security | User Session  | Interactive session remained active |

---

# SPL Queries

## Query 1

**Question**

Which processes were created during the investigation?

```spl
index=main EventCode=1
| table _time Image ParentImage User
```

---

## Query 2

**Question**

Which DNS queries were recorded?

```spl
index=main dns
```

---

## Query 3

**Question**

Which outbound network connections occurred?

```spl
index=main network
```

---

## Query 4

**Question**

Can the process, DNS request, and network activity be correlated within the investigation window?

```spl
index=main
| sort _time
```

---

# Timeline

```text
11:03

Application Started

↓

11:04

DNS Query

↓

11:05

Outbound Connection

↓

11:06

User Session Continued
```

---

# Timeline Analysis

The reconstructed timeline showed a logical sequence of events:

1. An application was launched.
2. The application requested DNS resolution.
3. DNS successfully resolved the destination.
4. The application established an outbound network connection.
5. No further suspicious behaviour was observed.

This sequence is consistent with expected application behaviour.

---

# Event Correlation

Rather than analysing each event independently, the investigation correlated telemetry across multiple sources.

| Source            | Evidence Provided   |
| ----------------- | ------------------- |
| Sysmon Event ID 1 | Process creation    |
| Sysmon DNS        | Domain resolution   |
| Sysmon Network    | Outbound connection |
| Windows Security  | User context        |

The combined evidence produced a much clearer understanding of endpoint behaviour than any individual log source.

---

# Process Relationship Analysis

Observed sequence:

```text
Application

↓

DNS Resolution

↓

Outbound Connection
```

This workflow is commonly observed when applications communicate with external services.

The process responsible for the DNS request was the same process that established the subsequent network connection.

No unexpected intermediary processes were identified.

---

# DNS Analysis

The DNS activity was reviewed to determine:

* Which process generated the request.
* Which domain was queried.
* Whether the request matched expected laboratory activity.
* Whether repeated or unusual DNS patterns existed.

The DNS request was consistent with the expected behaviour of the application executed during the laboratory exercise.

No evidence of excessive DNS activity or suspicious domain generation patterns was identified.

---

# Network Activity Analysis

The outbound connection was reviewed to determine:

* Associated process
* Connection timing
* Sequence relative to DNS resolution
* Overall investigation context

The connection occurred immediately after successful DNS resolution and matched expected application behaviour.

No unexpected destinations or unusual communication patterns were identified within the available telemetry.

---

# Evidence Confidence Matrix

| Evidence                | Confidence | Reason                                        |
| ----------------------- | ---------- | --------------------------------------------- |
| Sysmon Process Creation | High       | Native endpoint telemetry                     |
| Sysmon DNS Events       | Medium     | Available within controlled lab scope         |
| Sysmon Network Events   | Medium     | Correlated successfully with process activity |
| Windows Security Log    | High       | Confirmed user context                        |
| Laboratory Test Notes   | High       | Activity matched documented testing           |

---

# Analyst Decision Log

| Decision                          | Reason                                            |
| --------------------------------- | ------------------------------------------------- |
| Reviewed process creation first   | Establish origin of activity                      |
| Correlated DNS requests           | Determine external communication sequence         |
| Reviewed outbound connection      | Validate network behaviour                        |
| Compared activity with test notes | Confirm authorised execution                      |
| Closed investigation              | Evidence supported expected application behaviour |

---

# Analysis

Applications routinely generate DNS requests before establishing outbound network connections.

Viewed individually, process creation, DNS activity, and network communication each provide only a partial view of endpoint behaviour.

By correlating these events into a single timeline, the investigation confirmed that:

* The process executed normally.
* DNS resolution occurred immediately afterwards.
* The application successfully established an expected outbound connection.
* The activity matched the documented laboratory exercise.

No evidence suggested persistence, lateral movement, privilege escalation, or command-and-control communication.

The available telemetry therefore supports normal endpoint behaviour.

---

# MITRE ATT&CK Context

## Potential Techniques

**T1071 – Application Layer Protocol**

Outbound network communication may be associated with application layer communication used by legitimate software or attackers.

In this investigation, the observed behaviour matched authorised laboratory activity.

No adversary behaviour was confirmed.

---

# Detection Opportunities

Future detections could include:

* Rare outbound destinations.
* High-frequency DNS requests.
* Unexpected parent-child process relationships.
* Network communication immediately following suspicious process execution.
* Correlation of PowerShell execution with external network connections.

These detections should be supported by additional context before escalation.

---

# Investigation Outcome

| Field          | Result          |
| -------------- | --------------- |
| Classification | Benign Positive |
| Severity       | Low             |
| Confidence     | Medium          |
| Status         | Closed          |

---

# Recommended Action

No incident response action is required.

Continue monitoring endpoint telemetry for:

* Unusual process execution chains.
* Unexpected DNS requests.
* Network connections to unknown or high-risk destinations.
* Repeated activity outside expected user behaviour.

---

# Limitations

The investigation was performed within a controlled laboratory environment.

The following telemetry was unavailable:

* Firewall logs
* Proxy logs
* Packet captures
* Endpoint Detection and Response (EDR)
* Threat intelligence enrichment

These additional sources would improve confidence during production investigations but were not required to answer the objectives of this lab.

---

# Lessons Learned

This investigation demonstrated that meaningful endpoint analysis depends on correlating related events rather than examining them in isolation.

By reconstructing the sequence from process creation to DNS resolution and finally to network communication, it became much easier to understand application behaviour and distinguish expected activity from behaviour that might require escalation.

The investigation also reinforced the importance of considering multiple telemetry sources together. Correlating Sysmon events with Windows Security logs provided stronger evidence than relying on a single event type alone.

---

# Skills Demonstrated

* Endpoint Telemetry Analysis
* Sysmon Investigation
* DNS Analysis
* Network Activity Investigation
* Event Correlation
* Timeline Reconstruction
* Splunk Investigation
* Evidence-Based Decision Making
* MITRE ATT&CK Mapping

---

# Screenshots


![DNS activity](Screenshots/case_003_01_dns_events.png)

*Figure 1. DNS query generated by the investigated process during the documented laboratory activity.*

---

![Network connection timeline](Screenshots/case_003_02_network_connections.png)

*Figure 2. Correlated process creation, DNS resolution, and outbound network activity reconstructed into a chronological investigation timeline.*


---

# Reviewer Notes

This investigation demonstrates the importance of event correlation during endpoint analysis.

While process creation, DNS activity, and network communication are all expected behaviours for many legitimate applications, reviewing these events together provides valuable context that helps analysts distinguish normal operations from behaviour that may warrant escalation.

The available telemetry, documented laboratory testing, and correlated timeline support a **Benign Positive** conclusion.

Future investigations could increase confidence by incorporating additional telemetry such as firewall logs, proxy logs, packet captures, and threat intelligence enrichment.
