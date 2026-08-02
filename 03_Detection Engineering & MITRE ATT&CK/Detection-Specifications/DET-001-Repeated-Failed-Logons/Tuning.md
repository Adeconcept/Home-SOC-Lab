# DET-001 Tuning

## Current Version

Five or more failed logons for the same account and host within ten minutes.

## Tuning Options

| Option | Use when | Risk |
|---|---|---|
| Different thresholds for user and service accounts | Baseline shows different normal behaviour | Incorrect grouping may hide abuse |
| Add source IP | The field is reliably available | Missing or malformed fields can reduce coverage |
| Raise priority after a successful logon | A success follows repeated failures | Success may still be legitimate |
| Raise priority when multiple accounts are targeted | One source targets several users | Requires reliable source identification |
| Suppress duplicate user and host alerts for 30 minutes | Repeat alerts create no new information | Long suppression may hide continued activity |
| Exclude an approved scanner account | Ownership and scope are documented | Broad exclusions may hide compromised tools |

## Decision

No exclusion should be added only to make the alert quiet. Every exclusion must have a named owner, reason, scope, and review date.
