# Analyst Response Playbook

## DET-001: Multiple Failed Logons

1. Confirm the account, host, failure count, and time window.
2. Identify source address, workstation, logon type, and failure reason where available.
3. Check whether a successful logon followed.
4. Determine whether other accounts were targeted.
5. Compare the activity with known service, VPN, or stored-credential behaviour.
6. Contact the account owner or system owner when needed.
7. Escalate when unauthorized success, widespread targeting, or unusual source activity is present.

## DET-002: Encoded PowerShell

1. Review the complete command line.
2. Identify the user, image path, and parent process.
3. Determine whether the script or tool is approved.
4. Review nearby child processes, files, DNS activity, and connections.
5. Check whether Office or another unusual process launched PowerShell.
6. Compare the event with deployment or administration schedules.
7. Escalate when execution remains unexplained or related activity is suspicious.

## DET-003: PowerShell Network or DNS Activity

1. Confirm whether the evidence is a network connection or only a DNS query.
2. Identify destination domain, IP, port, and protocol where available.
3. Review the complete PowerShell command.
4. Determine whether the destination is approved and expected.
5. Check for downloaded or created files.
6. Review related process execution.
7. Escalate suspicious external communications or unexplained downloads.

## Analyst Guardrail

A detection match is an investigation lead. Record the evidence that supports the final verdict and avoid conclusions based only on the rule name.
