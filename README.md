# powershell-process-detection-wazuh
## Overview

This lab demonstrates how PowerShell execution can be detected using Sysmon Process Creation events and investigated using Wazuh Threat Hunting.

PowerShell is one of the most abused Windows utilities by attackers for execution, persistence, privilege escalation, defense evasion, and lateral movement. Monitoring PowerShell activity is a fundamental skill for SOC analysts.

---

## Objectives

- Verify Sysmon and Wazuh services
- Execute PowerShell commands
- Generate suspicious PowerShell activity
- Detect PowerShell process creation
- Investigate PowerShell events in Wazuh
- Analyze process command-line arguments
- Map activity to MITRE ATT&CK

---

## Lab Environment

| Component | Version |
|-----------|----------|
| Windows 11 | Endpoint |
| Sysmon | Installed |
| Wazuh Agent | Installed |
| Wazuh Manager | 4.x |
| PowerShell | Windows PowerShell |

---

## MITRE ATT&CK

| Technique | Description |
|-----------|-------------|
| T1059.001 | Command and Scripting Interpreter: PowerShell |

---

## Detection Workflow

1. Verify Sysmon service
2. Verify Wazuh Agent
3. Execute PowerShell commands
4. Execute PowerShell with ExecutionPolicy Bypass
5. Review Sysmon Event ID 1
6. Hunt PowerShell activity in Wazuh
7. Analyze command-line arguments
8. Document investigation findings

---

## Commands Used

### Verify Sysmon

```powershell
Get-Service Sysmon64
```

### Verify Wazuh Agent

```powershell
Get-Service WazuhSvc
```

### Generate PowerShell Activity

```powershell
Get-Date

Get-Process

Get-Service

Get-ChildItem C:\Windows
```

### Simulate Suspicious PowerShell Execution

```powershell
powershell.exe -ExecutionPolicy Bypass -Command "Get-Process"
```

### Review Sysmon Process Creation

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -FilterXPath "*[System[(EventID=1)]]" -MaxEvents 50
```

### Filter PowerShell Events

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -FilterXPath "*[System[(EventID=1)]]" -MaxEvents 100 | Where-Object {$_.Message -match "powershell.exe"}
```

---

## Wazuh Investigation

Suggested searches:

```
powershell.exe
```

```
data.win.eventdata.image:*powershell.exe*
```

```
data.win.eventdata.commandLine:*ExecutionPolicy*
```

Review:

- Process Name
- Command Line
- Parent Process
- User
- Timestamp
- Rule Information
- Agent Name

---

## Detection Results

- PowerShell execution detected
- Sysmon generated Event ID 1
- Wazuh collected PowerShell telemetry
- Command-line arguments successfully reviewed
- Investigation completed

---

## Skills Practiced

- PowerShell Analysis
- Process Creation Investigation
- Sysmon Event ID 1
- Wazuh Threat Hunting
- Command-Line Analysis
- IOC Identification
- MITRE ATT&CK Mapping

---

## Outcome

Successfully simulated PowerShell execution, detected the activity using Sysmon Process Creation events, and investigated the event within Wazuh Threat Hunting.
