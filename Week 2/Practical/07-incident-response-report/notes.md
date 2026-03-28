# Practical 7 — Incident Response Report

**Template:** SANS Incident Response  
**Intern:** Pretam Saha | **Week:** 2

---

# INCIDENT RESPONSE REPORT

**Incident ID:** IR-2025-001  
**Date:** August 18, 2025  
**Severity:** High (P2)  
**Status:** Closed  
**Analyst:** Pretam Saha  

---

## Executive Summary

On August 18, 2025, a phishing attack was detected targeting an employee via a spoofed corporate email. The attacker delivered a malicious PowerShell payload via an email attachment. Upon execution, a reverse shell was established allowing remote access to the victim machine. The incident was detected within 30 minutes by the SIEM, contained within 2 hours, and fully remediated by end of day. No confirmed data exfiltration occurred. Immediate actions included system isolation, malware removal, credential reset, and blocking the attacker's infrastructure.

---

## Timeline

| Time | Event |
|------|-------|
| 09:45 AM | Phishing email delivered to employee inbox |
| 10:00 AM | Employee opened malicious attachment |
| 10:01 AM | PowerShell reverse shell executed (T1059.001) |
| 10:02 AM | Attacker established C2 connection on port 4444 |
| 10:30 AM | SIEM alert triggered — unusual PowerShell process from Outlook |
| 10:35 AM | SOC analyst begins investigation |
| 10:45 AM | Victim system isolated from network |
| 11:00 AM | Malware artifacts collected via Velociraptor |
| 11:30 AM | Malware removed, system cleaned |
| 12:00 PM | System restored from clean backup |
| 12:30 PM | Credentials reset for affected user |
| 01:00 PM | Attacker IP blocked via CrowdSec |
| 02:00 PM | Incident closed, report filed |

---

## Findings

| Finding | Details |
|---------|---------|
| Attack Type | Spearphishing with malicious attachment |
| MITRE Technique | T1566.001 — Spearphishing Attachment |
| Entry Point | Email attachment (fake invoice PDF) |
| Payload | PowerShell reverse shell |
| C2 Server | 192.168.1.100:4444 |
| Persistence | Registry Run Key created |
| Data Exfiltration | None confirmed |

---

## Mitigation Steps

1. Block attacker domain at email gateway
2. Enable MFA for all user accounts
3. Deploy email sandboxing solution
4. Restrict PowerShell execution via AppLocker
5. Conduct company-wide phishing awareness training
6. Update EDR signatures with new IOCs
7. Review and update email security policies

---

## Incident Response Flowchart

```
Detection (SIEM Alert)
        ↓
   Triage & Analysis
        ↓
   Containment (Isolate System)
        ↓
   Eradication (Remove Malware)
        ↓
   Recovery (Restore from Backup)
        ↓
   Lessons Learned (Update Playbook)
```

---

## IOCs (Indicators of Compromise)

| IOC Type | Value | Action |
|----------|-------|--------|
| IP Address | 192.168.1.100 | Blocked via CrowdSec |
| File Hash | d41d8cd98f00b204e9800998ecf8427e | Quarantined |
| Registry Key | HKCU\...\Run\UpdateService | Deleted |
| Domain | evil-attacker.com | Blocked at DNS level |
