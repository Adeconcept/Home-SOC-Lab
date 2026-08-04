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
| DET-001 | [Deployed or Designed] | [Add screenshot] | [Add note] |
| DET-002 | [Deployed or Designed] | [Add screenshot] | [Add note] |
| DET-003 | [Deployed or Designed] | [Add screenshot] | [Add note] |

