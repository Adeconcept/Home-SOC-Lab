# DET-001: Repeated Failed Logons

## Overview

This detection identifies repeated Windows authentication failures that may indicate password guessing or password spraying activity.

The rule is designed to highlight authentication patterns that exceed normal user behavior while minimizing false positives from occasional mistyped passwords.

---

## Threat Scenario

Password guessing remains one of the most common techniques used during the early stages of an intrusion.

Attackers may attempt multiple authentication requests against one or more accounts before obtaining valid credentials.

Monitoring repeated authentication failures provides analysts with an opportunity to identify suspicious activity before successful compromise occurs.

---

## Detection Strategy

The detection monitors Windows Security Event **4625** and identifies repeated failed authentication attempts occurring within a defined time window.

Rather than alerting on every failed logon, the rule focuses on unusual authentication frequency that warrants analyst review.

---

## Detection Logic

### Data Source

- Windows Security Event Logs

### Primary Event

- Event ID 4625

### SPL

```spl
index=endpoint EventCode=4625

| stats count by Account_Name Source_Network_Address

| where count >= 5
```

---

## Investigation Guidance

When this detection triggers:

1. Identify the affected account.
2. Review the originating host.
3. Check for successful logons following the failures.
4. Review authentication history.
5. Determine whether the activity aligns with normal user behavior.

---

## False Positives

Possible legitimate causes include:

- Forgotten passwords
- Newly onboarded users
- Service account misconfigurations
- Automated scripts
- Password synchronization issues

---

## MITRE ATT&CK

| Tactic | Technique |
|---------|-----------|
| Credential Access | T1110.001 Password Guessing |

---

## Success Criteria

The detection should:

- Identify repeated failed authentication activity.
- Reduce false positives from isolated failures.
- Provide sufficient context for analyst investigation.
- Support early identification of credential attacks.

---

## Related Documentation

- Detection Testing/DET001-Test.md
- SPL/DET001.spl
- MITRE/Coverage.md
- Week 6 Case 001
