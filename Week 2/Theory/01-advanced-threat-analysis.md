# Theory 1 — Advanced Threat Analysis

**Intern:** Pretam Saha | **Week:** 2 | **Reviewer:** CyArt

---

## 1. STRIDE Threat Modeling

STRIDE is a framework to identify security threats by analyzing six categories:

| Letter | Threat | Real Example |
|--------|--------|--------------|
| **S** | Spoofing | Attacker steals session token and logs in as another user |
| **T** | Tampering | SQL injection modifies records in the database |
| **R** | Repudiation | No audit logs — user denies performing an action |
| **I** | Information Disclosure | Passwords sent over HTTP visible in Wireshark |
| **D** | Denial of Service | Flood of requests crashes the web server |
| **E** | Elevation of Privilege | Normal user accesses /admin due to missing auth check |

### How to Apply STRIDE to a Web Application

**Assets:** User credentials, session tokens, database records  
**Entry Points:** Login form, API endpoints, HTTP headers  
**Trust Boundaries:** Internet ↔ Web Server ↔ Database

**STRIDE Applied to Login Page:**

| Component | Threat Type | Attack Scenario | Mitigation |
|-----------|-------------|-----------------|------------|
| Login Page | Spoofing (S) | Fake login page phishes credentials | HTTPS, MFA |
| Web Server | Tampering (T) | HTTP request modified via proxy | Input validation |
| Web Server | Repudiation (R) | No logs of who logged in | Enable audit logging |
| HTTP Traffic | Info Disclosure (I) | Plain text password captured | Enforce TLS/HTTPS |
| Web Server | DoS (D) | 1000 login requests per second | Rate limiting |
| Any User | Privilege Escalation (E) | User accesses /admin endpoint | Role-based access control |

**Tool Used:** OWASP Threat Dragon v2.6.1  
**Screenshot:** `../Screenshots/stride-threat-model.png`

---

## 2. MITRE ATT&CK Framework

### What It Is
A globally accessible knowledge base of adversary tactics and techniques based on real-world cyber attack observations. Used by defenders to understand how attackers operate.

### Structure
```
Tactics (The Goal) → Techniques (The Method) → Sub-techniques (Specific Variation)
```

### Key Tactics (Enterprise Matrix)
| Tactic | Description |
|--------|-------------|
| Initial Access | Getting into the target system |
| Execution | Running malicious code |
| Persistence | Maintaining access after reboot |
| Privilege Escalation | Gaining higher permissions |
| Defense Evasion | Avoiding detection |
| Credential Access | Stealing usernames/passwords |
| Discovery | Learning about the environment |
| Lateral Movement | Moving to other systems |
| Exfiltration | Stealing data out |
| Command and Control | Communicating with malware |

### T1566 — Phishing (Detailed Analysis)

| Field | Details |
|-------|---------|
| Technique ID | T1566 |
| Tactic | Initial Access |
| Platforms | Windows, Linux, macOS, Office Suite, SaaS, Identity Provider |
| Version | 2.7 |
| Created | 02 March 2020 |
| Last Modified | 24 October 2025 |

**Sub-techniques:**
- **T1566.001** — Spearphishing Attachment: Malicious PDF/Word/Excel sent via email
- **T1566.002** — Spearphishing Link: URL pointing to fake/malicious website
- **T1566.003** — Spearphishing via Service: Attack via LinkedIn, WhatsApp, Slack DMs
- **T1566.004** — Spearphishing Voice: Phone call tricks user into action (vishing)

**Real Groups Using T1566:**
- AppleJeus (G1049) — distributes malicious payloads via spearphishing
- Axiom (G0001) — uses spear phishing for initial compromise
- GOLD SOUTHFIELD — malspam campaigns

**Detection Methods:**
- Monitor email gateway logs for suspicious attachments
- Alert on process creation from email clients (Outlook spawning PowerShell)
- Analyze URLs in emails against threat intel feeds

**Mitigations:**
- User awareness training
- Email sandboxing and filtering
- Multi-factor authentication (MFA)
- Anti-phishing browser extensions

**Screenshot:** `../Screenshots/mitre-attack-t1566.png`

---

## 3. Advanced Attack Vectors

### A. Advanced Persistent Threats (APTs)

**Definition:** Sophisticated, long-term targeted attacks, usually nation-state sponsored, designed to steal data or cause disruption while remaining undetected.

**Characteristics:**
- Stay hidden for months or years
- Use multiple attack stages (initial access → lateral movement → exfiltration)
- Highly targeted — specific organizations or individuals
- Use zero-days, custom malware, and living-off-the-land techniques

**Example:** APT29 (Cozy Bear) — Russian SVR-backed group responsible for SolarWinds attack

### B. Supply Chain Attack — SolarWinds 2020

**Source:** CISA Advisory AA20-352A  
**Screenshot:** `../Screenshots/solarwinds-cisa.png`

**What Happened:**
```
Step 1: APT29 compromised SolarWinds' internal build environment (March 2020)
Step 2: Injected SUNBURST malware into Orion software update (DLL backdoor)
Step 3: SolarWinds released the poisoned update as legitimate software
Step 4: 18,000+ organizations installed the trojanized Orion update
Step 5: Attackers remained hidden for ~9 months
Step 6: Discovered by FireEye (Mandiant) in December 2020
```

**Victims:** US Treasury, Pentagon, DHS, Microsoft, Intel, Cisco, and more

**MITRE ATT&CK Mapping:**

| Stage | Tactic | Technique ID | Technique Name |
|-------|--------|--------------|----------------|
| Compromise build server | Initial Access | T1195.002 | Compromise Software Supply Chain |
| Inject malicious DLL | Persistence | T1574.001 | DLL Search Order Hijacking |
| Avoid detection 14 days | Defense Evasion | T1497 | Virtualization/Sandbox Evasion |
| Communicate with C2 | Command & Control | T1071 | Application Layer Protocol |
| Steal data | Exfiltration | T1041 | Exfiltration Over C2 Channel |

**Key Lesson:** Even trusted, digitally signed software updates can be weaponized. Organizations must verify software integrity (hash checking) and monitor ALL outbound traffic, even from trusted applications.

### C. Zero-Day Exploits

**Definition:** A vulnerability that is unknown to the vendor/public. No patch exists, making it extremely dangerous.

**Zero-Day Lifecycle:**
```
Discovery → Weaponization → Attack → Disclosure → Patch Released → Patch Deployed
```

**From Exploit-DB (March 2026):**

| Date | Exploit | Type | Platform | Severity |
|------|---------|------|----------|----------|
| 2026-03-03 | WordPress Backup Migration 1.3.7 — RCE | WebApps | Multiple | Critical |
| 2026-03-03 | WeGIA 3.5.0 — SQL Injection | WebApps | PHP | High |
| 2026-02-11 | Windows 10.0.17763.7009 — Spoofing | Remote | Windows | High |
| 2026-02-11 | glibc 2.38 — Buffer Overflow | Local | Linux | High |
| 2026-02-04 | Redis 8.0.2 — RCE | Remote | Linux | Critical |

**Screenshot:** `../Screenshots/exploit-db.png`

**Detection Challenges:**
- No signatures exist for zero-days
- Behavior-based detection is the only option
- Require threat hunting and anomaly detection

---

## Key Objectives Achieved

- [x] Created STRIDE threat model for web application using OWASP Threat Dragon
- [x] Mapped phishing attack to MITRE ATT&CK T1566
- [x] Analyzed SolarWinds 2020 supply chain attack
- [x] Studied zero-day exploits via Exploit-DB
- [x] Explored MITRE ATT&CK Navigator at attack.mitre.org
