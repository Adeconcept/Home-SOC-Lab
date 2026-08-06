# Authentication Baseline

---


## Purpose

Summarize successful and failed Windows logons before hunting for authentication sequences.

---


## SPL

```spl
index=endpoint
sourcetype="csv:windows_security"
(Id="4624" OR Id="4625")
| stats count by Id
```

```spl
index=endpoint
sourcetype="csv:windows_security"
(Id="4624" OR Id="4625")
| rex field=Message "Account For Which Logon Failed:[\s\S]*?Account Name:\s+(?<FailedUser>[^\r\n]+)"
| rex field=Message "New Logon:[\s\S]*?Account Name:\s+(?<SuccessfulUser>[^\r\n]+)"
| eval Account=coalesce(FailedUser, SuccessfulUser)
| stats count by Account Id
| sort Account Id
```


---


## Results

| Observation | Evidence | Interpretation |
|---|---|---|
| **Successful logons** | `151` events (Event ID 4624) | High baseline volume indicating a stable, authorized testing session. This establishes the normal operating background traffic for your endpoint. |
| **Failed logons** | `11` events (Event ID 4625) | Low baseline volume representing isolated credential friction. This provides a clean, low-noise environment for your multi-event sequential hunt. |
| **Accounts observed** | `Adekola`, `Labtest`, `SYSTEM`, and localized machine accounts | The data is dominated by highly privileged service accounts and targeted administrative access, matching an expected adversary-emulation profile. |
| **Extraction gaps** | Unparsed sub-fields or `UNKNOWN` values across some message blocks | Static regex patterns can fail if Windows Event structure variations alter indentation. This creates an engineering impact where silent parsing gaps mask true user identity context. |


---

## Limitation

Separate event formats may require different field extraction. Counts may be influenced by overlapping CSV uploads.
