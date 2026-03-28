# Practical 4 — Incident Response Simulation

**Tools:** MITRE Caldera, Velociraptor  
**Intern:** Pretam Saha | **Week:** 2

---

## Task: Simulate Phishing Attack and Collect Artifacts

### Phishing Attack Path (100-word summary)

A mock phishing payload was deployed via MITRE Caldera on a Windows VM. The attacker sent a simulated spearphishing email with a malicious attachment (T1566.001). Upon execution, a PowerShell reverse shell was established (T1059.001), providing remote access. The attacker performed local reconnaissance using built-in Windows commands (T1082 — System Information Discovery). Caldera's agent beaconed back to the C2 server over HTTP on port 8888. Velociraptor collected forensic artifacts including running processes, active network connections, and recent file modifications. Key IOCs identified: PowerShell spawned from Outlook, outbound connection to 192.168.1.100:4444, and suspicious registry persistence key created.

---

### Velociraptor Artifact Queries

```sql
-- Collect all running processes
SELECT * FROM processes WHERE name = 'powershell.exe';

-- Collect network connections
SELECT * FROM netstat WHERE remote_port = 4444;

-- Collect recent file modifications
SELECT * FROM file WHERE modified > now() - 3600;
```

**Save results to CSV:**
```bash
velociraptor artifacts collect Generic.System.Pstree --output /tmp/artifacts.csv
```

---

### IOC Analysis Table

| IOC Type | Value | Significance |
|----------|-------|--------------|
| Process | powershell.exe spawned by outlook.exe | Phishing attachment executed |
| Network | Outbound to 192.168.1.100:4444 | C2 communication |
| Registry | HKCU\Software\Microsoft\Windows\CurrentVersion\Run | Persistence established |
| File | %TEMP%\payload.exe | Dropped malware |

---

### MITRE ATT&CK Mapping

| Stage | Technique | ID |
|-------|-----------|-----|
| Initial Access | Spearphishing Attachment | T1566.001 |
| Execution | PowerShell | T1059.001 |
| Discovery | System Info Discovery | T1082 |
| C2 | Web Protocols | T1071.001 |
| Persistence | Registry Run Keys | T1547.001 |
