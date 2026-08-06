# Network Baseline

## Purpose

Establish a limited view of DNS queries and destinations associated with processes.


---


## DNS SPL

```spl
index=endpoint
sourcetype="csv:sysmon"
Id="22"
| rex field=Message "QueryName:\s+(?<QueryName>[^\r\n]+)"
| rex field=Message "Image:\s+(?<Image>[^\r\n]+)"
| stats count dc(QueryName) AS unique_domains values(QueryName) AS domains by Image
| sort - count
```

---


## Connection SPL

```spl
index=endpoint
sourcetype="csv:sysmon"
Id="3"
| rex field=Message "Image:\s+(?<Image>[^\r\n]+)"
| rex field=Message "DestinationIp:\s+(?<DestinationIp>[^\r\n]+)"
| rex field=Message "DestinationPort:\s+(?<DestinationPort>[^\r\n]+)"
| stats count dc(DestinationIp) AS unique_destinations values(DestinationPort) AS ports by Image
| sort - count
```


---


## Results

| Observation | Evidence | Interpretation |
|---|---|---|
| **Most active DNS process** | `svc.exe` | An executive binary or potential wrapper engine is generating the highest volume of name resolution traffic, making it a critical baseline focal point to ensure it isn't masking beaconing behavior. |
| **PowerShell domains** | `://example.com`, `aka.ms` | Indicates standard administrative optimization checks (`aka.ms`) running alongside test loops or staging strings (`example.com`). This serves as a vital baseline to separate routine telemetry from suspicious callbacks. |
| **Network Event ID 3 available** | **No** (limited or omitted in some log subsets) | The system must rely on auxiliary telemetry (Sysmon ID 22 and Wireshark pcap traces) to infer connection attempts. The analyst cannot directly pivot from a process name to a raw outbound firewall socket inside Splunk. |
| **Rare destination lead** | `48.209.138.168` | `Rare in this dataset only`. This external IP address stands out as a unique destination in this isolated log subset. It represents an excellent hunting lead for investigating direct socket mappings or third-party cloud hosting connections. |

---


## Limitation

When Event ID 3 is unavailable, the baseline is limited to DNS and supporting other packet evidence. A DNS query does not prove a successful network connection.
