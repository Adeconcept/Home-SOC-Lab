# VAL-001 Timeline

| Time | Layer | Evidence | Interpretation |
|---|---|---|---|
| 12:56:44 | Generation | Atomic test started | Reviewed test initiated by analyst via Invoke-AtomicTest T1082 -TestNumbers 1|
| 12:57:15 | Collection | Sysmon Event ID 1 | systeminfo.exe executed with parent process powershell.exe|
| 12:57:45 | Ingestion | Splunk event | Event reached the endpoint index and fields (Image, CommandLine, User) parsed correctly |
| 12:57:45 | Detection | Existing rule result | No rule expected; activity did not meet behavioral anomaly thresholds |
| 12:58:11 | Cleanup | Cleanup output | Not Required; target commands are read-only and did not modify system state |

## Timeline Verdict

The timeline proves that the discovery activity can be fully reconstructed using Sysmon process creation logs across the generation, collection, and ingestion layers.
