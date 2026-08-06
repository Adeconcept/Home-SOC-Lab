# Process Baseline

## Purpose

Establish a limited view of which processes, users, and parent processes appear in the available Sysmon data.

## SPL

```spl
index=endpoint
sourcetype="csv:sysmon"
Id="1"
| rex field=Message "Image:\s+(?<Image>[^\r\n]+)"
| rex field=Message "ParentImage:\s+(?<ParentImage>[^\r\n]+)"
| rex field=Message "User:\s+(?<User>[^\r\n]+)"
| stats count values(ParentImage) AS parent_processes by Image User
| sort - count
```

---


## Questions

- Which processes appear most often?
- Which appear once?
- Which users ran them?
- Which parents launched them?
- Which command interpreters are common?


---


## Results

| Observation | Evidence | Interpretation |
|---|---|---|
| **Most frequent process** | `svc.exe`, `WmiPrvSE.exe`, `MicrosoftupdateEdge.exe`, and `cmd.exe` | Indicates active script scheduling or wrapper activity (`svc.exe`), heavy structural query automation (`WmiPrvSE.exe`), native administrative execution (`cmd.exe`), and a highly active application binary (`MicrosoftupdateEdge.exe`) running routine background workloads. |
| **Rare process** | `whoami.exe`, `systeminfo.exe`, `ipconfig.exe` | `Rare in this dataset only`. These administrative discovery binaries executed only once. This low frequency forms a classic host and network reconnaissance anomaly that warrants a closer look. |
| **Common parent-child pair** | `explorer.exe` -> `cmd.exe` | This pairing represents a standard user opening a command prompt manually from the Windows desktop GUI. It establishes a trusted baseline for interactive user sessions. |
| **Unusual lead selected** | `MicrosoftupdateEdge.exe` running alongside administrative shells | The high-frequency appearance of a blended binary name (`MicrosoftupdateEdge.exe`) near standard interpreters provides an excellent hunting lead to verify if it is an authorized system optimization or an execution masquerading attempt. |


---


## Limitation

A low count is not proof of malicious activity. The dataset is too small and controlled to establish a production process baseline.
