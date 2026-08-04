# Limitations

## Test Scope

- Four low-risk atomic tests do not represent a complete intrusion.
- One test does not validate every implementation of an ATT&CK technique.
- Upstream test numbers and definitions may change.

## Telemetry

- Sysmon visibility depends on the active configuration.
- Event ID 11 may not be recorded for every file.
- CSV exports can omit events outside the selected time window.
- Batch upload does not demonstrate real-time monitoring.

## Detection

- Discovery and command-shell behaviour has many legitimate uses.
- String-based obfuscation detection can be noisy and evadable.
- A search result is not automatically a production-ready alert.
- A missed alert may be caused by test, collection, ingestion, parsing, or rule logic.

## Alerting

- Splunk trial permissions may restrict scheduled alerts.
- No-alert results must be interpreted at each validation layer.

## Safety

- Results apply only to the reviewed tests in the isolated lab.
- Tests outside the documented scope were not evaluated.
