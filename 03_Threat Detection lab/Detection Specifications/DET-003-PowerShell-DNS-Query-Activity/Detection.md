# DET-003: PowerShell DNS Query Activity

## Summary

Identifies anomalous process behavior where `powershell.exe` initiates outbound network activity. Following a recorded telemetry gap for Sysmon Event ID 3 (Network Connections), this detection was actively modified to leverage Sysmon Event ID 22 to track script-driven DNS query resolutions.


---


## Status

| Field | Value |
|---|---|
| Status | Validated |
| Version | 1.0 |
| Severity | Medium |
| Confidence | Medium (Optimized via Sysmon ID 22 field extraction) |
| Data source | Sysmon |
| Event IDs | 22 (DNS Fallback implemented due to ID 3 telemetry gap) |
| ATT&CK | T1059.001 PowerShell (Execution), T1071.004 DNS (Command and Control) |




---



## Detection Hypothesis

PowerShell executing outbound domain name resolutions shortly after process creation indicates script-driven external communication. This requires immediate analyst triage to separate routine system administration from active Command and Control (C2) beaconing or data exfiltration.


---



## Telemetry Decision

Run this first:

```spl
index=endpoint
sourcetype="det:sysmon"
Id="3"
| stats count
```

Following the architectural playbook rules, the deployment path was adapted to use the **DNS Query Activity** model.

### Implemented Production Query (Sysmon Event ID 22)


```spl
index=endpoint
source="det-sysmon.csv"
Id="22"
Message="*powershell.exe*"
| rex field=Message "Image:\s+(?<Image>[^\r\n]+)"
| rex field=Message "QueryName:\s+(?<QueryName>[^\r\n]+)"
| rex field=Message "User:\s+(?<User>[^\r\n]+)"
| table _time host User Image QueryName
```

Primary query: [Query.spl](https://github.com/Adeconcept/Home-SOC-Lab/blob/b8d0e730a0476895682854e2bd772ec48360505c/03_Threat%20Detection%20lab/Detection%20Specifications/DET-003-PowerShell-DNS-Query-Activity/Query.spl)

DNS fallback: [DNS-Fallback.spl](https://github.com/Adeconcept/Home-SOC-Lab/blob/b8d0e730a0476895682854e2bd772ec48360505c/03_Threat%20Detection%20lab/Detection%20Specifications/DET-003-PowerShell-DNS-Query-Activity/DNS-Fallback.spl)



---




## Decision Record

| Decision | Why | Value |
|---|---|---|
| Pivot to Event ID 22 DNS Logging | Event ID 3 network connection logs were completely unavailable | Maintains endpoint script visibility despite host telemetry gaps |
| Rename to DNS Query Activity | DNS queries show intent to connect but do not guarantee a completed TCP/UDP handshake | Preserves technical accuracy in analyst reporting |
| Keep severity Medium | PowerShell frequently performs legitimate external web activity and API lookups | Avoids over-prioritizing common administrative noise |
| Map primarily to T1059.001 & T1071.004 | The script name drives the detection, while the DNS payload represents potential C2 | Keeps the enterprise ATT&CK framework defensible |
| Extract QueryName Destination Context | The requested domain string is the key indicator needed to evaluate reputation | Improves analyst triage speed during live incidents |



---



## Known False Positives

- Administrative scripts downloading approved internal dependencies
- Native Windows/Microsoft cloud-management modules and telemetry updates
- IT service desk automation tools and cloud API lookups
- Third-party software update scripts executing via scheduled tasks



---



## Limitations

- Event ID 3 network connection stream remains completely disabled on endpoints.
- DNS query evidence does not mathematically confirm a successful data transfer.
- Static CSV log data prevents real-time, interactive process tree tracking.
- External destination domain reputation scoring is not natively embedded inside the table view.



---



## Evidence


![Initial 4625 reveal](https://github.com/Adeconcept/Home-SOC-Lab/blob/2fa6c1ca7d3b402ad2754856e0186fed9cce1f93/03_Threat%20Detection%20lab/Screenshots/08_DET_001_targetuser_extract.png)

*Figure 1. Windows Event ID 4625 events associated with the controlled laboratory account during the documented investigation window.*


![Initial 4625 reveal](https://github.com/Adeconcept/Home-SOC-Lab/blob/2fa6c1ca7d3b402ad2754856e0186fed9cce1f93/03_Threat%20Detection%20lab/Screenshots/08_DET_001_targetuser_extract.png)

*Figure 2. Windows Event ID 4625 events associated with the controlled laboratory account during the documented investigation window.*




---


### Evidence Fields
* **Host:** `SOC-WIN11`
* **Extracted Destination Domain:** `example.com`
* **Alert Status:** Active / Saved Search Configured
