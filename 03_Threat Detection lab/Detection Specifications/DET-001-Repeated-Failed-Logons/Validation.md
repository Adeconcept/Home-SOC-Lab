# DET-001 Validation

## Safe Test

Use only the dedicated `labtest` account. Do not test the primary user account and do not exceed a configured lockout threshold.

## Test Record

| Test ID | Activity | Expected | Actual | Result |
|---|---|---|---|---|
| DET001-N1 | One failed logon | No result | [Add actual] | [Pass or Fail] |
| DET001-B1 | Four failed logons in ten minutes | No result | [Add actual] | [Pass or Fail] |
| DET001-P1 | Five failed logons in ten minutes | Detection result | [Add actual] | [Pass or Fail] |
| DET001-R1 | Repeat the five-failure test | Detection result | [Add actual] | [Pass or Fail] |

## Evidence Fields

| Field | Value |
|---|---|
| Test account | labtest |
| Host | [Add actual] |
| Start time | [Add actual] |
| End time | [Add actual] |
| Failure count | [Add actual] |
| TargetUser extracted | [Yes or No] |
| Alert created | [Yes, No, or Trial Restricted] |

## Enrichment Query

```spl
index=endpoint
source="week7-det001-security.csv"
Message="*labtest*"
(Id="4624" OR Id="4625")
| sort 0 _time
| table _time host Id Message
```

## Validation Verdict

**Status:** [Validated, Needs Tuning, or Failed]

**Evidence-based conclusion:** [Add one sentence describing only what the results prove]
