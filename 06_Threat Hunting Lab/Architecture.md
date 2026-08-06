# Architecture

```mermaid
flowchart LR
    A[Windows 11 ARM] --> B[Windows Security Logs]
    A --> C[Sysmon Telemetry]
    B --> D[CSV Export]
    C --> D
    D --> E[Splunk endpoint Index]
    E --> F[Data Health Checks]
    F --> G[Baselines]
    G --> H[Hypothesis-Driven Hunts]
    H --> I[Lead Investigation]
    I --> J[Verdict]
    J --> K[Detection Candidate or No Action]
```

---

## Architecture Decision

The project reuses known investigation, detection-test, Atomic Red Team, and network-analysis datasets.

**Reason:** The behaviour is already understood, allowing hunt logic to be checked against known activity.

**Limitation:** Batch exports do not prove what occurred outside the exported windows.
