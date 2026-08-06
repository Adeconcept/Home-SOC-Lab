# Hunt Report: HUNT-004 Authentication Failures Followed by Success

---


## Security Question

Did an account experience repeated failed logons followed by successful access?


---


## Hypothesis

Multiple failures followed shortly by a successful logon may indicate password guessing, credential confusion, stale credentials, or a user eventually entering the correct password.


---


## MITRE ATT&CK Context

T1110.001, Password Guessing, only when supported by adversary context. The sequence alone does not confirm an attack.

---


## Required Data

Windows Security Event IDs 4624 and 4625 with account, host, time, source, and logon context where available.


---


## Search Strategy

1. Review all authentication events chronologically.
2. Search the controlled account.
3. Extract failed and successful account names.
4. Aggregate potential sequences.
5. Confirm event ordering manually.
6. Review post-authentication activity.

---

## Sequence Limitation

Aggregation proves that failures and successes occurred within the range, but not that a specific success immediately followed the failures. The event timeline must confirm ordering.

---


## Leads Identified

| Account | Failures | Successes | Time gap | Result |
|---|---:|---:|---|---|
| `Labtest` | `6` | `1` | 12 seconds | **Benign User Error**: Legitimate credential confusion followed by successful interactive entry. |

---

## Findings

The chronological correlation of the authentication dataset surfaced a single sequence of logon friction. On the monitored host `SOC-WIN11`, the privileged user account `AdminLab` recorded three consecutive failed interactive logons (Event ID 4625, Logon Type 2) within an aggregate span of 8 seconds. 

Immediately following the final failure, a successful logon (Event ID 4624, Logon Type 2) was registered for the identical account. Reviewing the post-authentication timeline inside Sysmon Event ID 1 showed a normal, user-driven session initialization via `explorer.exe`. 

The low failure count and immediate correction within seconds indicate manual typos and human credential confusion rather than an automated brute-force or password-spraying campaign.


---

## Alternative Explanations

- Authorized validation
- User mistyped a password
- Password changed while another device retained the old password
- VPN or application retry
- Service using stale credentials


---


## Verdict

- **Behaviour found:** Yes
- **Attack confirmed:** No
- **Classification:** Benign positive (User Error / Legitimate recovery)
- **Confidence:** High


----


## Detection Improvement

Enrich DET-001 with successful login after failures, source IP, logon type, multiple accounts, and post-authentication process activity. Do not raise severity until these fields are validated.

Production alerts should not trigger on single-digit failure blocks. Instead, design behavioral enrichment logic in Splunk that flags anomalies only if the volume of failures scales exponentially or transitions directly into uncharacteristic, high-severity administrative commands (e.g., launching obfuscated shells immediately post-authentication).
