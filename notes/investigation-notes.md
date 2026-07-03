# Investigation Notes

## Scenario

PowerShell execution was observed on a Windows endpoint. Since PowerShell is commonly used by attackers, the activity required investigation to determine whether it was legitimate administrative activity or potentially suspicious.

---

## Initial Verification

Verified:

- Sysmon service running
- Wazuh Agent connected
- Endpoint successfully reporting to Wazuh

---

## Activity Performed

Executed standard PowerShell commands:

- Get-Date
- Get-Process
- Get-Service
- Get-ChildItem

Executed a PowerShell process using:

```
powershell.exe -ExecutionPolicy Bypass -Command "Get-Process"
```

This command is safe but resembles attacker behavior because it uses the `ExecutionPolicy Bypass` parameter.

---

## Detection

Sysmon generated:

Event ID 1

Process Creation

Observed process:

```
powershell.exe
```

---

## Wazuh Investigation

Threat Hunting successfully detected PowerShell execution.

Observed event information included:

- Process Image
- Command Line
- Parent Process
- User Account
- Timestamp
- Agent Name
- Rule Information

---

## Evidence Collected

- PowerShell process creation
- Sysmon Event ID 1
- Command-line arguments
- Wazuh Threat Hunting event
- Process metadata
- User context

---

## Indicators Observed

| Indicator | Value |
|-----------|-------|
| Process | powershell.exe |
| Event ID | 1 |
| Execution Policy | Bypass |
| Activity | Process Creation |

---

## MITRE ATT&CK

Technique

```
T1059.001
```

Technique Name

```
Command and Scripting Interpreter: PowerShell
```

Tactic

```
Execution
```

---

## Severity Assessment

Medium

PowerShell execution is common for administrators but becomes suspicious when combined with parameters such as `ExecutionPolicy Bypass`, encoded commands, or unusual parent processes.

---

## Analyst Conclusion

The observed PowerShell activity was generated intentionally for lab validation. Sysmon successfully recorded the process creation event, and Wazuh provided sufficient telemetry for investigation and correlation.
