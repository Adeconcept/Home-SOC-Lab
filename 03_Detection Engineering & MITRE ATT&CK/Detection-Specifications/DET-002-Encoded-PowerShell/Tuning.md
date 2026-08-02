# DET-002 Tuning

## Version History

| Version | Logic | Problem or improvement |
|---|---|---|
| 1.0 | All PowerShell execution | Too broad for alerting |
| 2.0 | Require `-EncodedCommand` or `-enc` | Removes most normal interactive PowerShell |
| 3.0 | Extract parent context and assign dynamic severity | Improves analyst prioritization |

## Current Tuning Decision

Office parent processes such as Word, Excel, or Outlook receive higher priority. Other matching activity remains Medium severity.

## Future Tuning Options

- Approved script hash allowlist
- Trusted management host allowlist
- Lower severity for a documented deployment account
- Higher severity for hidden-window flags
- Higher severity for execution from temporary directories
- Higher severity when network activity follows
- Higher severity when files are created

## Guardrail

Do not exclude all administrator PowerShell. Administrator accounts and management tools can also be compromised.
