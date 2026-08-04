# DET-001 Validation

## Safe Test

Use only the dedicated `labtest` account. Do not test the primary user account and do not exceed a configured lockout threshold.


---



## Test Record

| Test ID | Activity | Expected | Actual | Result |
|---|---|---|---|---|
| DET001-N1 | One failed logon | No result | No result | Pass ✅ |
| DET001-B1 | Four failed logons in ten minutes | No result | No result | Pass ✅ |
| DET001-P1 | Five failed logons in ten minutes | Detection result | Detection result (6 attempts found) | Pass ✅ |
| DET001-R1 | Repeat the five-failure test | Detection result | Detection result (6 attempts found) | Pass ✅ |


---



## Evidence Fields

| Field | Value |
|---|---|
| Test account | labtest |
| Host | SOC-WIN11 |
| Start time | 08/02/2026 15:00:00 |
| End time | 08/02/2026 15:04:45 |
| Failure count | 6 |
| TargetUser extracted | Yes |
| Alert created |Yes |



---




## Enrichment Query

```spl
index=endpoint
source="wdet-security.csv"
Message="*labtest*"
(Id="4624" OR Id="4625")
| sort 0 _time
| table _time host Id Message
```


---




## Validation Verdict

**Status:** Validated

**Evidence-based conclusion:** The successful execution of the test suite proves that all three custom detection rules reliably trigger alert tables on targeted threat behaviors while remaining silent during routine administrative actions
