# Phishing Investigation Report

## Case Summary

| Field         | Details                            |
| ------------- | ---------------------------------- |
| Alert Type    | Suspected Phishing                 |
| Environment   | TryHackMe SOC Simulation           |
| Analyst Role  | SOC Level 1                        |
| Severity      | High            |
| Affected User | jdoe@company.thm                 |
| Verdict       | True Positive |
| Escalation    | Yes                     |

---

## 1. Alert Summary

A phishing-related alert was received involving a suspicious email sent to `jdoe@company.thm `.

The purpose of the investigation was to determine whether the message represented a genuine phishing attempt and whether additional user or endpoint compromise had occurred.

---

## 2. Initial Hypothesis

The email may represent a phishing attempt designed to:

`Impersonate a trusted corporate supplier and redirect the user to a malicious web domain to harvest corporate login credentials or execute unauthorized scripts.`

Further evidence was required before confirming the alert.

---

## 3. Email Analysis

### Sender

**Display Name:** `Amazon Web Services Support`
**Sender Address:** `support@amz-security-update.com`
**Reply-To:** `support@amz-security-update.com`
**Return-Path:** `bounce@amz-security-update.com`

### Observations

* The display name mimics a known vendor, but the root domain (amz-security-update.com) does not belong to the official brand organization.
* The message contains generic headers and was sent from an external IP space not associated with the legitimate enterprise service.
* A sense of artificial urgency was established regarding account suspension if immediate action was not taken.



### Authentication

| Control | Result                | Analyst Interpretation |
| ------- | --------------------- | ---------------------- |
| SPF     | Fail | The sending mail server IP address is not explicitly authorized within the sender's DNS SPF record.     |
| DKIM    | Fail | The cryptographic body signature is missing entirely or fails verification parameters.`     |
| DMARC   | Fail | Due to alignment failures across both SPF and DKIM policies, the message triggers an explicit DMARC validation failure.    |

---

## 4. Message Analysis

### Subject

`URGENT: Action Required - Security Vulnerability Detected on Your Corporate Account`

### Social Engineering Indicators

* **Urgency:** Demands immediate remediation within a fixed 24-hour time constraint to avoid complete loss of service.
* **Credential request:** Instructs the recipient to input active workspace credentials on an external form page.
* **Brand impersonation:** Uses corporate logos and stylized templates copying Amazon Web Services communication schemas.
* **Financial pressure:** N/A
* **Unexpected attachment:** N/A

### Analyst Observation

The email creates artificial urgency by claiming an active workspace account is flagged for immediate termination. It provides a hyperlinked button routing outward to a non-standard interface designed to harvest user directory passwords.
---

## 5. URL / Domain Analysis

| Indicator  | Observation | Assessment                          |
| ---------- | ----------- | ----------------------------------- |
| amz-security-update.com | Newly registered domain domain with anonymous registrar records and poor web reputation scores. | Malicious |
| https://amz-security-update.com   | The endpoint acts as an explicit lookalike login portal mimicking standard Single Sign-On (SSO) prompt pages. | Malicious |



### Analyst Interpretation

The external domain mimics standard platform utilities but has zero functional or operational ties to corporate networks. OSINT reputation tracking tools flag this specific path as a distributed framework targeting identity and credential harvesting infrastructure.


---

## 6. Attachment Analysis

**Filename:** N/A
**File Type:** N/A
**Hash:** N/A

### Findings

No static files or email attachments were linked to this notification instance. The attack vectors are strictly driven via embedded hyperlink configurations.

---

## 7. User Activity

**Link Clicked:** Yes
**Attachment Opened:** N/A
**Credentials Submitted:** Yes
**Suspicious Authentication Activity:** Yes

### Assessment

Network telemetry logs confirmed that the targeted user clicked the malicious redirection pathway. Log correlation within the active workspace directory verified that a successful single sign-on authentication event occurred from an anomalous external IP block immediately following the link access window.
---

# 8. Indicators

| Type   | Indicator | Finding     |
| ------ | --------- | ----------- |
| Sender | support@amz-security-update.com | External adversary address mimicking vendor structures. |
| Domain | amz-security-update.com | Newly created domain setup with malicious scoring records. |
| URL    | https://amz-security-update.com | Credential harvesting landing node page. |
| IP     | 198.51.100.45 | Malicious sender host/unauthorized sign-in origin location. |
| File   | N/A | No malicious binary packages delivered via email body structure. |
| Hash   | N/A | No static hash indicators collected for this field. |




---

# 9. Evidence Summary

| Evidence        | Observation     | Significance       |
| --------------- | --------------- | ------------------ |
| Email sender    | The domain amz-security-update.com was used. | Clearly isolates an external identity spoofing mechanism trying to pass as a trusted vendor. |
| Header          | Complete structural SPF, DKIM, and DMARC enforcement failures. | Proves that the incoming transmission origin lacks authentic alignment validations. |
| Message content | High-urgency threats about service isolation paired with a credential link. | Matches established social engineering playbooks aimed at causing cognitive panic. |
| URL / Domain    | The destination site acts as a lookalike infrastructure clone. | Validates an active threat vector aimed at extracting environment access tokens. |
| User activity   | Proxy logging verified navigation followed by an anomalous corporate sign-in. | Proves credential exposure occurred, creating an active host exploitation scenario. |

---

# 10. Verdict

## [TRUE POSITIVE

### Reasoning

This alert represents a definitive phishing attack that has resulted in verified user compromise. The incoming message explicitly failed core authentication standards including SPF, DKIM, and DMARC checks while utilizing brand lookalike domains to hide its origin. Host audit trails and internal firewall proxy logs confirm that the endpoint user engaged with the malicious URL and submitted domain credentials. Finally, consecutive directory events show an unauthorized login originating from the malicious infrastructure IP right after the submission time frame.

---

# 11. Escalation Recommendation

**Escalation:** Yes

### Reason

The alert must be immediately escalated to Tier 2 (L2) Incident Response handlers due to active account compromise. Because corporate authentication logs indicate unauthorized access following the link interaction, this case is no longer just a passive email delivery issue; it has evolved into a dynamic threat containment scenario requiring active session revocation.

### Recommended Actions

* Revoke all active session tokens and force a global password reset for jdoe@company.thm.
* Enable temporary account isolation and apply explicit multi-factor authentication (MFA) challenge triggers.
* Implement a global block rule across the corporate email gateway and web proxy configurations for the domain amz-security-update.com.

---

# 12. Analyst Conclusion

A targeted credential-harvesting phishing attack successfully bypassed baseline message filters by leveraging external lookalike domain infrastructures. The affected employee clicked the embedded URL link and inadvertently exposed active corporate access parameters to external adversaries, leading to subsequent unauthorized domain authentication entries. The case is classified as a True Positive and is formally escalated to Tier 2 handlers for comprehensive session isolation, indicator sweeping across corporate networks, and forensic account remediation.
