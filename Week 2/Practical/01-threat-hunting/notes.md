# Practical 1 — Threat Hunting with Open-Source Tools

**Tools:** Elastic Security, Security Onion, Sigma Rules  
**Intern:** Pretam Saha | **Week:** 2

---

## Task: Write a Sigma Rule to Detect Suspicious PowerShell Activity

### Sigma Rule

```yaml
title: Suspicious PowerShell Activity
id: a1b2c3d4-e5f6-7890-abcd-ef1234567890
status: experimental
description: Detects PowerShell execution with -Command flag which may indicate malicious activity
author: Pretam Saha
date: 2025/08/18
logsource:
  category: process_creation
  product: windows
detection:
  selection:
    Image|endswith: '\powershell.exe'
    CommandLine|contains: '-Command'
  condition: selection
falsepositives:
  - Legitimate administrator scripts
  - Automated deployment tools
level: medium
tags:
  - attack.execution
  - attack.t1059.001
```

### How to Test This Rule

**Step 1:** Open a Windows VM  
**Step 2:** Open PowerShell and run:
```powershell
powershell -Command "Write-Host Test"
```
**Step 3:** Check Windows Event Log for Event ID 4688 (process creation)  
**Step 4:** The Sigma rule should trigger on this event

---

## Elastic Security Query

**Query to find PowerShell events (Event ID 4688):**
```
event.code: "4688" AND process.name: "powershell.exe"
```

**Extended query with command line:**
```
event.code: "4688" AND process.name: "powershell.exe" AND process.command_line: *-Command*
```

---

## Threat Hunting Log Table

| Timestamp | Process | Command Line | Event ID | Notes |
|-----------|---------|--------------|----------|-------|
| 2025-08-18 10:00:00 | powershell.exe | -Command Write-Host | 4688 | Suspicious execution |
| 2025-08-18 10:02:00 | powershell.exe | -Command Get-Process | 4688 | Possible recon |
| 2025-08-18 10:05:00 | powershell.exe | -Command Invoke-WebRequest | 4688 | HIGH — download attempt |

---

## MITRE ATT&CK Mapping

| Detection | Tactic | Technique | Sub-technique |
|-----------|--------|-----------|---------------|
| PowerShell -Command flag | Execution | T1059 | T1059.001 — PowerShell |

---

## Tools Reference

| Tool | Purpose | Link |
|------|---------|------|
| Elastic Security | SIEM + log analysis | https://www.elastic.co/security |
| Security Onion | Network security monitoring | https://securityonionsolutions.com |
| Sigma | Detection rule format | https://github.com/SigmaHQ/sigma |
