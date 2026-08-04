# Detection Improvement Backlog

| Candidate | Hypothesis | Required telemetry | Main tuning need | Priority |
|---|---|---|---|---|
| DET-004 | Multiple system-discovery commands from an unusual user or parent within five minutes may indicate reconnaissance | Sysmon Event ID 1 | Baseline inventory and support tools | Low |
| DET-005 | Two or more distinct network-discovery commands by one user or host within five minutes may indicate reconnaissance | Sysmon Event ID 1 | Exclude only documented diagnostics | Medium |
| DET-006 | Command shell executing `.bat` or `.cmd` from Temp, Downloads, or AppData may require review | Sysmon Event IDs 1 and 11 | Path normalization and installer context | Medium |
| DET-007 | PowerShell with dynamic execution, backticks, Base64 decoding, or character construction may indicate obfuscation | Sysmon Event ID 1 | Reduce false positives from complex administration | Medium |

## Prioritization Criteria

A candidate moves forward when:

1. Required telemetry is consistently available.
2. Test behaviour is reproducible.
3. The query can be explained clearly.
4. Legitimate activity can be baselined.
5. The rule adds value beyond a broad hunting search.
6. Positive and negative tests can be defined.
