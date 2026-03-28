# Practical 5 — Network Defense with Open-Source Tools

**Tools:** Suricata, Elastic SIEM, CrowdSec  
**Intern:** Pretam Saha | **Week:** 2

---

## Task: Configure Suricata to Block Malicious IPs and Map to MITRE ATT&CK

### Suricata Rule to Block Malicious IP

```
drop ip 192.168.1.100 any -> any any (msg:"Block Malicious IP"; sid:1000001; rev:1;)
```

**Save to:** `/etc/suricata/rules/local.rules`

**Test by pinging from another VM:**
```bash
ping 192.168.1.100
# Expected: No response — packets dropped
```

### Additional Suricata Rules

```
# Detect suspicious HTTP traffic (C2 communication)
alert http any any -> any any (msg:"Suspicious HTTP User-Agent"; content:"curl/"; http_user_agent; sid:1000002; rev:1;)

# Detect port scanning
alert tcp any any -> $HOME_NET any (msg:"Possible Port Scan"; flags:S; threshold:type threshold, track by_src, count 20, seconds 10; sid:1000003; rev:1;)

# Detect PowerShell download
alert http any any -> any any (msg:"PowerShell Download Attempt"; content:"powershell"; nocase; http_uri; sid:1000004; rev:1;)
```

---

## MITRE ATT&CK Mapping

| Suricata Alert | Tactic | Technique | ID | Notes |
|---------------|--------|-----------|-----|-------|
| Suspicious HTTP | Command and Control | Application Layer Protocol | T1071 | Outbound traffic to C2 |
| Port Scan | Discovery | Network Service Discovery | T1046 | Attacker mapping network |
| PowerShell Download | Execution | PowerShell | T1059.001 | Payload download |
| Blocked IP | Defense | N/A | N/A | Active blocking via Suricata |

---

## CrowdSec Setup

```bash
# Install CrowdSec
curl -s https://packagecloud.io/install/repositories/crowdsec/crowdsec/script.deb.sh | sudo bash
sudo apt install crowdsec

# Block attacker IP manually
sudo cscli decisions add --ip 192.168.1.100 --duration 24h --reason "Malicious activity"

# Verify block
sudo cscli decisions list

# Test block
ping 192.168.1.100
# Expected: Request timeout
```

---

## Elastic SIEM Integration

**Suricata log ingestion query:**
```
event.module: suricata AND event.category: intrusion_detection
```

**Dashboard view:** Suricata alerts mapped to MITRE ATT&CK techniques in Elastic Security
