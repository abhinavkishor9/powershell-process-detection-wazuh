# Troubleshooting Notes

## Issue 1

### Problem

Sysmon service not running.

### Resolution

Verify the service:

```powershell
Get-Service Sysmon64
```

Start the service if required:

```powershell
Start-Service Sysmon64
```

---

## Issue 2

### Problem

Wazuh Agent not connected.

### Resolution

Verify:

```powershell
Get-Service WazuhSvc
```

Restart if necessary:

```powershell
Restart-Service WazuhSvc
```

---

## Issue 3

### Problem

No Sysmon Event ID 1 generated.

### Resolution

Verify Sysmon configuration includes **Process Creation** monitoring.

Run:

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 20
```

---

## Issue 4

### Problem

PowerShell events not visible in Wazuh.

### Resolution

- Refresh Threat Hunting.
- Increase the time range.
- Confirm the endpoint is connected.
- Wait 30–60 seconds for event ingestion.

---

## Issue 5

### Problem

Search query returned no results.

### Resolution

Try alternative searches:

```
powershell.exe
```

```
ExecutionPolicy
```

```
data.win.eventdata.image:*powershell.exe*
```

```
event.code:1
```

---

## Issue 6

### Problem

Incorrect timestamps between Sysmon and Wazuh.

### Resolution

Compare timestamps while accounting for timezone differences and allow a small delay for event ingestion.

---

## Issue 7

### Problem

Unable to identify the executed command.

### Resolution

Review the **CommandLine** field within the Sysmon Event ID 1 details and compare it with the Wazuh event.

---

## Lessons Learned

- PowerShell is a frequently abused Windows utility.
- Sysmon Event ID 1 provides detailed process creation telemetry.
- Command-line arguments are essential for distinguishing normal from suspicious PowerShell activity.
- Wazuh enables centralized investigation and correlation of PowerShell events.
- Mapping PowerShell activity to MITRE ATT&CK improves investigation context and reporting.
