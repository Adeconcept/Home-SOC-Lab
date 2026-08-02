# DET-003 Tuning

## Current Version

Identify PowerShell in Sysmon Event ID 3, or use an accurately named Event ID 22 DNS fallback.

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

No destination is excluded until its owner, purpose, and expected behaviour are documented.
