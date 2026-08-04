# DET-001: Multiple Failed Windows Logons for One Account

## Summary

Identifies five or more failed Windows logons for the same account and host within a ten-minute bucket.


---



## Status

| Field | Value |
|---|---|
| Status | Testing, change to Validated after evidence is added |
| Version | 1.0 |
| Severity | Low |
| Confidence | Medium when the account and host are extracted correctly |
| Data source | Windows Security |
| Event ID | 4625 |
| ATT&CK | T1110.001 Password Guessing |


---



## Detection Hypothesis

Five or more failed logons for one account within ten minutes may indicate password guessing, forgotten credentials, stale stored credentials, or an authentication configuration problem.

The threshold is a lab decision, not an industry standard.


---




## Detection Logic

1. Select Event ID 4625.
2. Extract the failed target account from `Message`.
3. Group events into ten-minute buckets by host and account.
4. Count failures.
5. Return groups with five or more failures.

Query: [Query.spl](Query.spl)


---



## Decision Record

| Decision | Why | Value |
|---|---|---|
| Five failures in ten minutes | Provides a clear boundary test in the lab | Demonstrates threshold logic without claiming a universal rule |
| Group by host and account | Prevents unrelated failures from being merged | Improves result relevance |
| Keep successful logon as enrichment | Success changes risk but is not required for the initial behaviour | Keeps version 1 understandable |
| Initial severity is Low | Repeated failures are common and need context | Reduces overstatement |



---


## Trigger

A result is produced when `failed_attempts >= 5`.


---



## Analyst Context

After a match, review Event IDs 4624 and 4625 for the same account to determine whether a successful logon followed the failures.


---




## Known False Positives

- User forgot a password
- Stored credentials remained on another device
- Scheduled task or service uses an old password
- Mapped drive reconnects with stale credentials
- VPN or application repeatedly retries
- Authorized testing


---



## Limitations

- Source IP and logon type may remain embedded in `Message`.
- Ten-minute buckets may split activity that crosses a bucket boundary.
- A match does not confirm malicious intent.
- Production thresholds require account and environment baselining.

---

## Evidence

- Base 4625 events
- Extracted `TargetUser`
- Four-failure no-trigger result
- Five-failure trigger result
- Authentication timeline
- Alert or saved-report configuration


![Initial 4625 reveal](https://github.com/Adeconcept/Home-SOC-Lab/blob/2fa6c1ca7d3b402ad2754856e0186fed9cce1f93/03_Threat%20Detection%20lab/Screenshots/08_DET_001_targetuser_extract.png)

*Figure 1. Windows Event ID 4625 events associated with the controlled laboratory account during the documented investigation window.*





![Failed logon events metadata](https://github.com/Adeconcept/Home-SOC-Lab/blob/2fa6c1ca7d3b402ad2754856e0186fed9cce1f93/03_Threat%20Detection%20lab/Screenshots/06_DET_001_extract_data.png)

*Figure 2. Reavealing account failure metadata.*






![Failed logon & Successful Logon events](https://github.com/Adeconcept/Home-SOC-Lab/blob/2fa6c1ca7d3b402ad2754856e0186fed9cce1f93/03_Threat%20Detection%20lab/Screenshots/07_DET_001_targetuser_account_logon.png)

*Figure 3. Failed logon & Successful Logon events.*
