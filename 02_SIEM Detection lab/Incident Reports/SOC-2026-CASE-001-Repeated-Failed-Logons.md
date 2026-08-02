# Incident Report: Repeated Failed Logons

> **Project:** SIEM Detection Lab: Windows Event Investigation with Splunk
>
> **Case ID:** SOC-2026-CASE-001

---

# Case Summary

| Field                | Value                         |
| -------------------- | ----------------------------- |
| Case ID              | SOC-2026-CASE-001             |
| Investigation        | Repeated Failed Logons        |
| Status               | Closed                        |
| Classification       | Benign Positive               |
| Severity             | Low                           |
| Confidence           | High                          |
| Analyst              | Adekola Durodola              |
| Investigation Window | 15 Minutes                    |
| Data Sources         | Windows Security Logs, Splunk |
| MITRE ATT&CK         | T1110 (Potential)             |
| Last Updated         | July 2026                     |

---

# Executive Summary

During routine monitoring of Windows authentication events, multiple failed logon attempts were identified for the controlled laboratory account. Repeated authentication failures are commonly associated with password guessing or brute-force attacks and therefore warranted further investigation.

The objective of this investigation was to determine whether the observed authentication pattern represented malicious activity or expected user behaviour.

Using Windows Security telemetry indexed in Splunk, authentication events were reviewed, correlated, and reconstructed into a chronological timeline. The investigation confirmed that the failed authentication attempts were generated during authorised laboratory testing and were immediately followed by a successful logon using the same account.

Although repeated failures can contribute to brute-force detection logic, the available evidence supports a **Benign Positive** classification.

---

# Investigation Objective

Determine whether repeated failed authentication events indicate:

* User error
* Incorrect credentials
* Password spraying
* Brute-force activity
* Authorised laboratory testing

---

# Alert

Repeated Windows Event ID **4625** events were identified during log review.

The authentication failures occurred within a short time window and involved the same user account.

---

# Initial Hypothesis

At the start of the investigation, several explanations were considered.

Possible causes included:

* The user entered an incorrect password.
* The account password had recently changed.
* An automated password guessing attempt occurred.
* A brute-force attack was in progress.
* The events were intentionally generated during laboratory testing.

No explanation was treated as fact until supported by evidence.

---

# Investigation Scope

| Item                 | Value                 |
| -------------------- | --------------------- |
| Host                 | Windows 11 ARM        |
| Account              | Controlled Lab User   |
| Investigation Window | 15 Minutes            |
| Event IDs Reviewed   | 4625, 4624            |
| SIEM                 | Splunk                |
| Endpoint Telemetry   | Windows Security Logs |

The investigation focused exclusively on authentication activity occurring during the documented testing period.

---

# Investigation Methodology

The following workflow was applied.

```text id="nh3bg0"
Authentication Alert

↓

Develop Hypothesis

↓

Review Event ID 4625

↓

Review Event ID 4624

↓

Correlate Events

↓

Build Timeline

↓

Validate Against Test Notes

↓

Determine Investigation Outcome
```

---

# Evidence Collected

| Time  | Source           | Event         | Relevance                     |
| ----- | ---------------- | ------------- | ----------------------------- |
| 09:42 | Windows Security | Event ID 4625 | Initial failed authentication |
| 09:43 | Windows Security | Event ID 4625 | Second failed attempt         |
| 09:44 | Windows Security | Event ID 4625 | Third failed attempt          |
| 09:45 | Windows Security | Event ID 4624 | Successful authentication     |

---

# SPL Queries

## Query 1

**Question**

Were failed authentication attempts recorded?

```spl id="mifjlwm"
index=main EventCode=4625
```

---

## Query 2

**Question**

Did the account authenticate successfully afterwards?

```spl id="pmjlwm"
index=main EventCode=4624
```

---

## Query 3

**Question**

How many authentication failures occurred?

```spl id="5rjlwm"
index=main EventCode=4625
| stats count by Account_Name
```

---

# Timeline

```text id="kjsapc"
09:42

Failed Logon

↓

09:43

Failed Logon

↓

09:44

Failed Logon

↓

09:45

Successful Logon
```

---

# Timeline Analysis

The authentication sequence showed three consecutive failed logon attempts followed immediately by a successful authentication.

No additional accounts were involved.

No account lockout occurred.

No authentication attempts originated from unexpected hosts.

The timing closely matched the documented laboratory testing.

---

# Event Correlation

The investigation correlated two Windows Security Event IDs.

| Event ID | Purpose          |
| -------- | ---------------- |
| 4625     | Failed Logon     |
| 4624     | Successful Logon |

Reviewing both events together provided greater context than analysing either event independently.

The successful authentication immediately following the failures significantly reduced confidence that malicious activity had occurred.

---

# Evidence Confidence Matrix

| Evidence               | Confidence | Reason                              |
| ---------------------- | ---------- | ----------------------------------- |
| Windows Security Logs  | High       | Native operating system telemetry   |
| Splunk Search Results  | High       | Indexed successfully                |
| Investigation Timeline | High       | Events correlated correctly         |
| Laboratory Test Notes  | High       | Activity matched documented testing |

---

# Analyst Decision Log

| Decision                                | Reason                                                |
| --------------------------------------- | ----------------------------------------------------- |
| Expanded search window                  | Ensure no earlier authentication attempts were missed |
| Reviewed Event ID 4624                  | Determine whether authentication eventually succeeded |
| Correlated failed and successful logons | Understand the complete authentication sequence       |
| Closed investigation                    | Evidence matched authorised testing activity          |

---

# Analysis

Repeated authentication failures are a common indicator used in brute-force detection logic because attackers frequently attempt multiple passwords against the same account.

However, the presence of failed logons alone is insufficient to conclude that an attack occurred.

In this investigation:

* The failures involved a single controlled laboratory account.
* A successful authentication immediately followed the failed attempts.
* The sequence matched the documented testing timeline.
* No account lockout occurred.
* No unusual source hosts or accounts were observed.

Taken together, the available evidence supports authorised user activity rather than malicious authentication attempts.

This investigation highlights the importance of considering event context before classifying suspicious indicators as security incidents.

---

# MITRE ATT&CK Context

## Potential Technique

**T1110 – Brute Force**

Repeated failed authentication attempts may contribute to brute-force detection logic.

In this investigation, however, the supporting evidence did not indicate an active brute-force attack.

The MITRE mapping is therefore documented as **potentially relevant**, not confirmed.

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

No incident response actions are required.

Recommended follow-up:

* Continue monitoring authentication activity.
* Maintain brute-force detection thresholds.
* Correlate future failed logons with account lockout events and authentication success.
* Investigate repeated failures originating from multiple hosts or external IP addresses if observed.

---

# Limitations

This investigation was conducted within a controlled laboratory environment.

The following evidence was not available:

* Active Directory logs
* External authentication sources
* Network firewall telemetry
* Endpoint Detection and Response (EDR) data

While these sources would provide additional context in a production environment, they were not required to answer the investigation objective for this lab.

---

# Lessons Learned

This investigation reinforced that alerts are starting points rather than conclusions.

Repeated failed logons naturally attract analyst attention because they can indicate password guessing or brute-force activity. However, investigating the surrounding context, including successful authentications, timestamps, and documented testing activities, demonstrated that the observed events were consistent with authorised laboratory behaviour.

The investigation also highlighted the value of correlating multiple events rather than evaluating individual log entries in isolation. Building a simple timeline made it easier to understand the complete authentication sequence and reach a well-supported conclusion.

---

# Skills Demonstrated

* Windows Authentication Analysis
* Splunk Investigation
* Windows Event Log Analysis
* Timeline Reconstruction
* Evidence Correlation
* Hypothesis-Driven Investigation
* MITRE ATT&CK Mapping
* SOC Documentation

---

# Screenshots


![Failed logon events](02_SIEM-Detection-lab/Screenshots/case_001_01_failed_logon_events.png)

*Figure 1. Windows Event ID 4625 events associated with the controlled laboratory account during the documented investigation window.*

---

![Authentication counts](02_SIEM-Detection-lab/Screenshots/case_001_02_logon_counts.png)

*Figure 2. Summary of failed authentication attempts observed for the investigated account.*



---

# Reviewer Notes

Although the observed authentication pattern resembles activity commonly associated with brute-force attacks, the available telemetry, documented laboratory testing, and successful authentication sequence support a **Benign Positive** conclusion.

This case demonstrates the importance of validating detection logic through structured investigation before escalating an alert or declaring a security incident.
