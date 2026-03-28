# Theory 2 — Security Frameworks In Depth

**Intern:** Pretam Saha | **Week:** 2 | **Reviewer:** CyArt

---

## 1. NIST Cybersecurity Framework (CSF)

### Five Core Functions

| Function | Purpose | Key Activities |
|----------|---------|----------------|
| **Identify** | Know your assets and risks | Asset inventory, risk assessment, governance |
| **Protect** | Implement safeguards | Access controls, encryption, user training |
| **Detect** | Monitor for threats | SIEM, anomaly detection, log monitoring |
| **Respond** | Act on incidents | Incident response plan, communication |
| **Recover** | Restore operations | Backups, lessons learned, improvements |

### Implementation Tiers

| Tier | Name | Description |
|------|------|-------------|
| 1 | Partial | Ad hoc, reactive, no formal processes |
| 2 | Risk-Informed | Some risk awareness, not org-wide |
| 3 | Repeatable | Formal policies, consistent processes |
| 4 | Adaptive | Continuously improving, proactive |

### Ransomware Scenario Mapped to NIST CSF

**Scenario:** WannaCry-style ransomware hits an organization

| Function | Action Taken |
|----------|-------------|
| **Identify** | Catalog all systems, identify critical data, assess patch status |
| **Protect** | Enforce least privilege, disable SMBv1, patch MS17-010, train users |
| **Detect** | Alert on unusual file encryption activity, monitor SMB traffic |
| **Respond** | Isolate infected systems, notify stakeholders, activate IR plan |
| **Recover** | Restore from clean offline backups, verify integrity, document lessons |

### WannaCry Analysis (CISA Reference)
- **Date:** May 2017
- **Vulnerability exploited:** MS17-010 (EternalBlue — NSA zero-day)
- **Spread:** Self-propagating via SMBv1 protocol
- **Impact:** 200,000+ systems in 150 countries, NHS UK shutdown
- **NIST CSF Gap:** Protect function failed — unpatched systems, no network segmentation

---

## 2. ISO 27001

### Overview
International standard for Information Security Management Systems (ISMS). Contains **114 controls** across **14 domains**.

### Key Controls

| Control ID | Domain | Description | Ransomware Relevance |
|-----------|--------|-------------|----------------------|
| A.8.2.1 | Asset Management | Classify data by sensitivity | Identify critical data to protect |
| A.12.3 | Operations Security | Regular backups | Recover from ransomware |
| A.12.4 | Operations Security | Logging and monitoring | Detect encryption activity |
| A.12.6 | Operations Security | Patch management | Prevent EternalBlue-style attacks |
| A.14.2 | System Development | Secure development practices | Prevent vulnerable software |
| A.16.1 | Incident Management | Incident response process | Structured response to attacks |

### ISO 27001 vs NIST CSF Mapping

| ISO 27001 Control | NIST CSF Function |
|-------------------|-------------------|
| A.8.1 — Asset Inventory | Identify |
| A.9.1 — Access Control | Protect |
| A.12.4 — Logging | Detect |
| A.16.1 — Incident Response | Respond |
| A.12.3 — Backup | Recover |

---

## 3. CIS Controls Cross-Reference

| CIS Control | Description | NIST CSF Mapping |
|-------------|-------------|-----------------|
| CIS Control 1 | Inventory of Hardware Assets | Identify |
| CIS Control 2 | Inventory of Software Assets | Identify |
| CIS Control 3 | Data Protection | Protect |
| CIS Control 6 | Access Control Management | Protect |
| CIS Control 8 | Audit Log Management | Detect |
| CIS Control 17 | Incident Response | Respond |

---

## Key Objectives Achieved

- [x] Studied NIST CSF five functions and implementation tiers
- [x] Applied NIST CSF to WannaCry ransomware scenario
- [x] Explored ISO 27001 controls across key domains
- [x] Cross-referenced CIS Controls with NIST CSF
- [x] Mapped controls to real-world ransomware mitigation
