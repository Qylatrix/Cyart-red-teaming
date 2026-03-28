# Theory 3 — Incident Response Fundamentals

**Intern:** Pretam Saha | **Week:** 2 | **Reviewer:** CyArt

---

## Incident Lifecycle

```
Preparation → Detection → Containment → Eradication → Recovery → Lessons Learned
```

### Each Phase Explained

| Phase | What Happens | Key Tools/Actions |
|-------|-------------|-------------------|
| **Preparation** | Build IR team, create playbooks, set up monitoring tools | SIEM, EDR, playbooks, contact lists |
| **Detection** | Identify that an incident has occurred via alerts or reports | SIEM alerts, IDS/IPS, user reports |
| **Containment** | Stop the spread — isolate systems, block IPs | Firewall rules, network isolation, CrowdSec |
| **Eradication** | Remove the root cause — delete malware, patch vulnerability | AV scans, patch management |
| **Recovery** | Restore systems to normal operation | Backups, system restore, integrity checks |
| **Lessons Learned** | Document what happened and improve defenses | Post-incident report, updated playbooks |

---

## SOC Workflows

**Incident Priority Levels:**

| Priority | Level | Response Time | Example |
|----------|-------|---------------|---------|
| P1 | Critical | Immediate (< 15 min) | Active ransomware encryption |
| P2 | High | < 1 hour | Confirmed data breach |
| P3 | Medium | < 4 hours | Suspicious login attempts |
| P4 | Low | < 24 hours | Outdated software alert |

---

## Playbooks

A playbook is a documented step-by-step procedure for responding to a specific type of incident.

**Example — Phishing Playbook:**
1. Alert received from email gateway
2. Analyst reviews email headers and attachments
3. If malicious → quarantine email, notify user
4. Check if user clicked link or opened attachment
5. If clicked → isolate workstation, collect artifacts
6. Eradicate malware, reset credentials
7. Document and close ticket

---

## Key Resources
- SANS Incident Handler's Handbook: https://www.sans.org/white-papers/incident-handlers-handbook/
- Let's Defend Platform: https://letsdefend.io/
