# Hacking SOP

## Attack Kill Chain SOP

### Recon
Assess the situation from the outside-in, in order to identify both targets and tactics for the attack.

#### Active
See nmap.md in repo.

### Intrusion
Based on what is discovered in the reconnaissance phase, get into the systems. Often leveraging malware or security vulnerabilities.

### Exploitation
Exploit vulnerabilities, and deliver malicious code onto the system, in order to get a better foothold.

### Privilege Escalation
Gain more privileges on the system to get access to more data and permissions: for this, escalate privileges to an Admin if possible.

### Lateral Movement
Move laterally to other systems and accounts in order to gain more leverage: whether that’s higher permissions, more data, or greater access to systems.

### Obfuscation / Anti-forensics
Cover the tracks, possibly lay false trails, compromise data, and clear logs to confuse and/or slow down any forensics team.

### Denial of Service
Disrupt normal access for users and systems, in order to stop the attack from being monitored, tracked, or blocked

### Exfiltration
Get data out of the compromised system.