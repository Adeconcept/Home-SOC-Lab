# DET-007: Potentially Obfuscated PowerShell Command Line

## Status

**Draft learning detection**

## Purpose

Identify selected command-line characteristics that may indicate PowerShell obfuscation beyond `-EncodedCommand` and `-enc`.

## Detection Hypothesis

PowerShell containing dynamic execution, backticks, Base64 decoding functions, or character construction may require investigation, especially with an unusual parent process or network activity.

## Signals

- Encoded arguments
- `Invoke-Expression` or `iex`
- Backtick usage
- `FromBase64String`
- `[char]` construction

## Decision

DET-002 remains focused on encoded-command arguments. DET-007 is separate because broader obfuscation signals have different false-positive behaviour.

## Limitations

- Simple string matching is easy to evade.
- Legitimate scripts may contain the same features.
- Signal presence does not prove malicious intent.
- Production use requires baselining and stronger context.

## Validation

Use VAL-004 to record whether the current atomic command matches and why.
