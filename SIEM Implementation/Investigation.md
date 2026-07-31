# Investigation Report

---


## Scenario

Investigate PowerShell execution recorded by Sysmon.


---


## Question

Was PowerShell executed and how was it launched?

---

## Evidence

- Event ID 1
- Process Name
- Parent Process
- User
- Timestamp
- Command Line


---



## Analysis

Sysmon recorded the execution of PowerShell through Event ID 1.

The parent process was explorer.exe, indicating expected interactive user activity during the lab.

The execution time matched the actions performed during telemetry generation.


---



## Findings

- PowerShell execution identified
- Parent-child relationship verified
- User account identified
- Timestamp validated
- No suspicious behaviour observed

---

## Conclusion

The recorded activity matched expected user behaviour and demonstrated how Sysmon provides valuable endpoint telemetry for investigations.



