# Lab Preparation

## Baseline Checks

| Check | Expected | Actual |
|---|---|---|
| Hostname | SOC-WIN11 | [Add actual result] |
| Platform | Windows 11 ARM | [Add actual result] |
| Sysmon service | Running | [Add actual result] |
| Recent Sysmon events | Present | [Add actual result] |
| Defender | Enabled | [Add actual result] |
| Snapshot | PRE-WEEK8-ATOMIC-TESTING | [Add actual result] |
| Free disk space | Several GB available | [Add actual result] |
| Time zone | Documented | [Add actual result] |

## Baseline Evidence

Record host, user, time, operating system, architecture, processes, services, TCP connections, Sysmon health, free space, and snapshot status.

Do not upload full raw baseline exports when they contain sensitive or irrelevant data.

## Installation Decision

Use only the official PowerShell Gallery module or official Red Canary repositories.

Do not use an unreviewed `IEX` installer and do not add broad Defender exclusions.

## Framework Verification

| Item | Value |
|---|---|
| PowerShell version | [Add actual result] |
| Module version | [Add actual result] |
| Module path | [Add actual result] |
| Atomic definitions path | [Add actual result] |
| `Invoke-AtomicTest` available | [Yes or No] |

## Readiness Verdict

**Status:** [Ready, Blocked, or Needs Remediation]

**Reason:** [Add one evidence-based sentence]
