# Detection Coverage Update

| Behaviour | Existing coverage | Hunt output | Next action |
|---|---|---|---|
| **Encoded PowerShell** | DET-002 | `Benign Positive`: Validated visibility of `-EncodedCommand` telemetry blocks inside Sysmon ID 1. | **Keep and tune** to avoid alert fatigue from authorized administration frameworks. |
| **Broader PowerShell obfuscation** | Partial | `Benign Positive`: Identified short-form parameters and dynamic execution flags during scripting routines. | Create **separate candidate logic** utilizing advanced regex matching for hidden windows and short flags (`-ec`). |
| **System and network discovery chain** | None | `Benign Positive`: Caught a rapid, sequential 14-second reconnaissance burst (`whoami`, `systeminfo`, `ipconfig`). | Deploy **DET-008** as a correlation rule using a sliding-window transaction index. |
| **Unusual interpreter parent** | Partial contextual review | `Negative Finding`: Zero anomalous parent processes spawned administrative shells during this window. | Maintain baseline exclusions; promote **DET-009 when supported** on enterprise log sources. |
| **Failed logons followed by success** | DET-001 enrichment only | `Benign User Error`: Documented a cluster of 3 rapid failures preceding an immediate logon success. | **Validate enrichment** logic by cross-referencing process lineage via Sysmon ID 1 telemetry. |
| **PowerShell network context** | DET-003 or DNS variant | `Benign Positive`: Linked native execution paths directly to `://example.com` DNS lookups. | Deploy **DET-010** to flag non-whitelisted administration scripts connecting to external APIs. |

## Coverage Principle

A hunt result creates knowledge. It does not automatically prove that a stable detection exists.
