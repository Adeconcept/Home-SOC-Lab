# DET-003 Tuning

## Current Version

Isolates `pwsh.exe` execution blocks within Sysmon Event ID 22 logs to track script-driven DNS query name resolutions. 

## Tuning Options

| Option | Value | Caution |
|---|---|---|
| Approved internal destination allowlist | Removes known management traffic | Must be maintained and narrowly scoped |
| Increase priority for direct IP connections | Direct IP use may be less expected | Legitimate tools may still use direct IPs |
| Increase priority for rare domains | Focuses review on unusual destinations | Requires historical baselining |
| Increase priority after encoded execution | Adds behavioural context | Requires reliable time correlation |
| Increase priority when a file is created | Supports download investigation | File creation may be normal |
| Reduce priority for signed approved scripts | Uses trust context | Signed software can still be abused |


## Decision

* **Immediate Action:** No destination domains are excluded from the detection block until their business owner, exact purpose, and expected baseline behaviors are completely documented.
* **Architecture Note:** Due to laboratory telemetry restrictions surrounding Sysmon Event ID 3 (Network Connections), the detection logic was intentionally adapted to monitor Sysmon Event ID 22 (DNS Queries). This maintains equivalent visibility into outbound PowerShell communication vectors.
