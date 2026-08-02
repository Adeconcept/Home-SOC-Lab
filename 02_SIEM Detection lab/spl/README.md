# SPL Query Library

> **Project:** SIEM Detection Lab: Windows Event Investigation with Splunk

## Purpose

This directory contains the Splunk Search Processing Language (SPL) queries developed during the SIEM Detection Lab.

Each query was written to answer a specific investigative question rather than simply retrieve events. The queries support the incident reports contained within this repository and demonstrate how Windows Security Logs and Sysmon telemetry can be used during security investigations.

---

# Query Index

| File                    | Purpose                               | Related Case |
| ----------------------- | ------------------------------------- | ------------ |
| `failed-logons.spl`     | Identify failed authentication events | SOC-2026-CASE-001 |
| `successful-logons.spl` | Review successful authentication      | SOC-2026-CASE-001 |
| `powershell.spl`        | Detect PowerShell execution           | SOC-2026-CASE-002 |
| `process-creation.spl`  | Review Sysmon process creation        | SOC-2026-CASE-002 |
| `dns-events.spl`        | Review DNS requests                   | SOC-2026-CASE-003 |
| `network-events.spl`    | Review outbound network connections   | SOC-2026-CASE-003 |

---

# Query Development Principles

Each query was designed to:

* Answer a specific investigative question.
* Return meaningful results.
* Be easy to modify.
* Support evidence-based investigations.
* Reduce unnecessary noise.

The queries are intentionally simple because the goal of this lab was to learn investigation methodology before introducing more advanced correlation searches and detection logic.

---

# Future Enhancements

As the Home SOC Lab expands, this query library will include:

* Correlation searches
* Scheduled alerts
* Risk-based detections
* Detection tuning
* Threat hunting queries
* MITRE ATT&CK tagged searches
* Sigma rule equivalents
