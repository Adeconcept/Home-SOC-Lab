# Phishing Triage Investigation

## Executive Summary

This project documents hands-on phishing triage practice completed in the TryHackMe Phishing Analysis module and SOC simulation environment.

The investigation focused on examining suspicious emails, identifying phishing indicators, analysing sender and message characteristics, investigating URLs and other artifacts, correlating available evidence, determining whether activity was malicious or benign, and making an appropriate escalation recommendation.

The objective was to practise the workflow of a Level 1 SOC analyst rather than simply identify obvious phishing characteristics.

---

## Investigation Objectives

* Triage suspicious email alerts
* Analyse email headers and message content
* Identify phishing and social engineering indicators
* Investigate suspicious URLs, domains, IP addresses, and attachments
* Correlate available evidence
* Determine True Positive or False Positive
* Assess potential user impact
* Recommend escalation or closure
* Document findings in a concise analyst-style report

---

## Environment

| Component          | Details                                                                                  |
| ------------------ | ---------------------------------------------------------------------------------------- |
| Platform           | TryHackMe                                                                                |
| Training           | Phishing Analysis                                                                        |
| Environment        | Simulated SOC / phishing investigation                                                   |
| Role               | SOC Level 1 Analyst                                                                      |
| Investigation Type | Phishing / Email Security                                                                |
| Data Analysed      | Email headers, message body, URLs, domains, attachments and available security telemetry |

---

## Investigation Workflow

```text
Alert Received
      ↓
Initial Triage
      ↓
Email Header Analysis
      ↓
Email Body Analysis
      ↓
Extract Indicators
      ↓
Investigate URLs / Domains / Attachments
      ↓
Correlate Evidence
      ↓
Assess User Interaction
      ↓
Determine Verdict
      ↓
Escalate or Close
```

---

# Key Areas Investigated

### Sender Analysis

Reviewed:

* From address
* Sender domain
* Reply-To address
* Return-Path
* Display name
* Domain inconsistencies
* Sender impersonation

### Email Authentication

Where available, reviewed:

* SPF
* DKIM
* DMARC

Authentication results were treated as supporting evidence rather than the sole basis for determining whether an email was malicious.

### Message Content

The email body was reviewed for:

* Urgency
* Fear or financial pressure
* Requests for credentials
* Unexpected account actions
* Suspicious attachments
* Brand impersonation
* Unusual grammar or formatting
* Requests to bypass normal procedures

### URL and Domain Analysis

URLs were examined for:

* Displayed URL versus actual destination
* Suspicious or unrelated domains
* URL shortening
* Redirect chains
* Domain impersonation
* Lookalike domains
* Reputation findings

### Attachment Analysis

Where attachments were present, I reviewed:

* Filename
* Extension
* File type
* Hash
* Suspicious file behaviour
* Reputation or sandbox results

---

# Indicators

Indicators identified during individual investigations are recorded separately in [`indicators.md`](./indicators.md).

Indicators may include:

* Sender email addresses
* Domains
* URLs
* IP addresses
* File names
* File hashes
* Suspicious attachments
* Email authentication anomalies
* Social engineering indicators

---

# Analyst Findings

The investigation demonstrated that phishing alerts should not be classified based on a single suspicious characteristic.

Instead, I correlated multiple indicators including sender identity, email headers, message content, URLs, domain information, attachments, and available supporting telemetry before determining the final verdict.

Particular attention was given to identifying evidence that either supported or contradicted the initial phishing hypothesis.

---

# Verdict

Each investigated alert was classified as one of the following:

**True Positive**

Evidence confirmed that the email represented malicious or unauthorized activity.

or

**False Positive**

Investigation did not identify sufficient evidence of malicious activity and the email was determined to be legitimate or benign.

Detailed reasoning for individual alerts is recorded in the investigation report.

---

# Escalation Decision

Escalation was considered when:

* A phishing attempt was confirmed
* The user interacted with a malicious URL or attachment
* Credentials may have been submitted
* Malware execution was suspected
* Additional affected users may exist
* Endpoint investigation was required
* Containment or credential reset was required
* The available evidence required deeper Level 2 investigation

Alerts without evidence of compromise or malicious activity could be documented and closed according to the investigation findings.

---

# Recommended Response Actions

Depending on the investigation findings, appropriate defensive actions could include:

* Quarantine the malicious email
* Search for additional recipients
* Block malicious sender addresses or domains
* Block confirmed malicious URLs
* Investigate affected endpoints
* Review user authentication activity
* Reset potentially compromised credentials
* Revoke active sessions where required
* Submit malicious artifacts to security controls
* Update detections using confirmed indicators
* Provide targeted phishing awareness guidance

---

# Skills Demonstrated

* Phishing triage
* Email header analysis
* Email security investigation
* Indicator of Compromise identification
* URL and domain analysis
* Attachment analysis
* Social engineering identification
* Alert classification
* True Positive / False Positive determination
* Incident escalation
* Evidence correlation
* SOC report writing

---

# Key Takeaways

This exercise reinforced several principles of SOC investigation:

1. An alert is an investigation starting point, not proof of compromise.
2. Multiple pieces of evidence should support an analyst's verdict.
3. Email authentication results provide context but should not be analysed in isolation.
4. URLs and attachments require careful investigation before interaction.
5. User activity can significantly change the severity of a phishing incident.
6. True Positive alerts may require containment and additional investigation.
7. Clear documentation allows another analyst to quickly understand what happened and what should happen next.

---

## Training Disclaimer

This project documents cybersecurity training performed in an authorized TryHackMe simulation environment.

Screenshots and reports are provided to demonstrate investigation methodology. Challenge flags, answers, credentials, and unnecessary walkthrough information are intentionally excluded.

