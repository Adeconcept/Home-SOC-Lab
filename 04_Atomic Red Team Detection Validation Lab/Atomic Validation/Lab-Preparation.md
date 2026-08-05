# Lab Preparation

## Baseline Checks

| Check | Expected | Actual |
|---|---|---|
| Hostname | SOC-WIN11 | SOC-WIN11 |
| Platform | Windows 11 ARM | Windows 11 ARM64 (Microsoft.PowerShell_7.6.4.0_arm64) |
| Sysmon service | Running | Running (Verified via Sysmon Event ID 1 active collection) |
| Recent Sysmon events | Present | Present (Populating local Event Viewer and Splunk index) |
| Defender | Enabled | Enabled (With targeted exclusion for `C:\AtomicRedTeam`) |
| Snapshot | PRE-ATOMIC-TESTING | PRE-ATOMIC-TESTING (Created cleanly before framework installation) |
| Free disk space | Several GB available | 45.2 GB available (Sufficient allocation for logging storage) |
| Time zone | Documented | UTC-07:00 (Pacific Daylight Time) |


---


## Baseline Evidence

- **Host/User**: `SOC-WIN11\Adekola Durodola`
- **Time/Date**: 2026-08-04 10:53 AM (Baseline Snapshot Creation)
- **Operating System / Architecture**: Windows 11 Enterprise / ARM64 architecture
- **Processes / Services**: Core Windows processes running alongside the `Sysmon` logging service. 
- **TCP Connections**: Standard outbound loopback and local network listening sockets active.
- **Sysmon Health**: Healthy; confirmed logging active file and process parameters natively.
- **Free Space**: 45.2 GB remaining on primary system partition (`C:\`).
- **Snapshot Status**: `PRE-ATOMIC-TESTING` verified as the active virtual machine baseline recovery point.

---


## Installation Decision

- **Framework Engine**: Installed via the official Microsoft PowerShell Gallery utilizing the command: `Install-Module -Name invoke-atomicredteam,powershell-yaml -Scope CurrentUser -Force`. The unreviewed `IEX` installer was safely bypassed due to network connection anomalies.
- **Atomics Payload Repository**: Downloaded manually as an archive package from the official Red Canary GitHub source to bypass web requests blocks, extraction verified locally.
- **Defender Posture**: Maintained real-time protection across the operating system while applying a highly targeted folder exclusion exclusively to `C:\AtomicRedTeam`.



---

## Framework Verification

| Item | Value |
|---|---|
| PowerShell version | 7.6.4.0 (ARM64 release profile) |
| Module version | 2.3.0 |
| Module path | `C:\Program Files\WindowsApps\Microsoft.PowerShell_7.6.4.0_arm64__8wekyb3d8bbwe\` / Local user path |
| Atomic definitions path | `C:\AtomicRedTeam\atomics` |
| `Invoke-AtomicTest` available | Yes |


---



## Readiness Verdict

**Status:** Ready

**Reason:** The core testing engine is successfully imported, local path configurations are pointing correctly to the manual `atomics` folder, and baseline execution capabilities have been validated.
