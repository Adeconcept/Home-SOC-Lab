# Alert Configuration

## Design Choice

The lab data is uploaded in batches, so scheduled alerts are more appropriate than real-time alerts. A real-time configuration would imply continuous telemetry that the lab does not provide.


---



## Alert Matrix

| Detection | Schedule | Search window | Trigger | Suggested suppression |
|---|---|---|---|---|
| DET-001 | Every 10 minutes | Last 15 minutes | Results greater than 0 | Same host and user for 30 minutes |
| DET-002 | Every 5 minutes | Last 10 minutes | Results greater than 0 | Same host, user, and command line for 30 minutes |
| DET-003 | Every 5 minutes | Last 10 minutes | Results greater than 0 | Review after observing duplicate behaviour |


---


## Required Alert Metadata

- Detection ID and name
- Description
- Owner
- Data source
- Schedule and search window
- Trigger condition
- Severity
- MITRE technique
- Analyst action
- Known false positives
- Version
- Last validation date




![Alert Evidence](https://github.com/Adeconcept/Home-SOC-Lab/blob/e6169920a4e8dabc42b54a2b9617a7fd44fbb425/03_Threat%20Detection%20lab/Screenshots/05_Alert_metada.png)


---



## Splunk Trial Fallback

When scheduled alerts are restricted:

1. Save the SPL query as a report.
2. Record the intended schedule and trigger.
3. Capture the saved-report configuration.
4. Mark the alert as `Designed, not deployed due to trial limitation`.
5. Do not claim that a triggered alert was created.


---



## Evidence Table

| Alert | Deployment status | Trigger evidence | Notes |
|---|---|---|---|
| DET-001 | Deployed | ![DET-001 Evidence](https://github.com/Adeconcept/Home-SOC-Lab/blob/e6169920a4e8dabc42b54a2b9617a7fd44fbb425/03_Threat%20Detection%20lab/Screenshots/01_DET_001_Alert_Evidence.png) | Successfully validated brute-force threshold. Caught 6 brute-force login attempts within a 10-minute window on host SOC-WIN11. |
| DET-002 | Deployed | ![DET-002 Evidence](https://github.com/Adeconcept/Home-SOC-Lab/blob/e6169920a4e8dabc42b54a2b9617a7fd44fbb425/03_Threat%20Detection%20lab/Screenshots/02_DET_002_Alert_Evidence.png) | Tuned query to Version 3 to include parent process mapping. Prioritizes alerts if executed by Microsoft Office processes. |
| DET-003 | Deployed | ![DET-003 Evidence](https://github.com/Adeconcept/Home-SOC-Lab/blob/e6169920a4e8dabc42b54a2b9617a7fd44fbb425/03_Threat%20Detection%20lab/Screenshots/03_DET_003_Alert_Evidence.png) | Detects execution behavior of PowerShell initiating network lookups. Leverages Sysmon ID 22 to surface outbound domain resolutions spawned specifically by powershell.exe script instances. |

