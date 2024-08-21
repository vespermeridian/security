# Nmap Playbook

## Flags
| mode | syntax | Notes |
| ---- | ------ | ----- |
| Ping Scan | -sn | Light Recon. Tells Nmap not to run a port scan after host discovery. |
| TCP Syn Scan | -sS | Half open, or 'stealth' scan. Faster. |
| TCP Connect Scan | -sT | Resp. of RST = closed. No response = filtered or firewall. Syn/Ack = open. |
| UDP Scan | -sU | Less reliable because stateless. Port closed if ICMP response is received. |
| Version Scan | -sV | Probe open ports to determine service/version info. |
| OS Version Scan | -sS -O --osscan-guess | Stealth scan w/ OS version detection. (Requires root) |
| Aggressive Scan | -A | Enables OS detection (-O), version scanning (-sV), script scanning (-sC) and traceroute (–traceroute). |
| Null Scan | -sN | The TCP packets sent don’t have any of the flags set. The target should respond back with an RST if the port is closed. |
| Fin Scan | -sF | Like Null Scan except it sends a completely empty TCP packet, it sends a packet with the FIN flag set which is used to gracefully close a connection. The target must respond back with an RST for closed. |
| Xmas Scan | -sX | Sends TCP packets with the PSH, URG and FIN flags set. Like the last two scan types, this too expects RST packets for closed ports. |
| ARP scan w/o port scan | -PR -sn | Sends ARP query w/o port scan. ARP reply = open. |
| Use Scripts | --script=default | Uses default scripts during scan. |

## Nmap Scripting Engine
| Script Category| Description |
| -------------- | ------------------- |
| auth | Authentication related scripts |
| broadcast | Discover hosts by sending broadcast messages |
| brute | Performs brute-force password auditing against logins |
| default | Default scripts, same as -sC |
| discovery | Retrieve accessible information, such as database tables and DNS names |
| dos | Detects servers vulnerable to Denial of Service (DoS) |
| exploit | Attempts to exploit various vulnerable services |
| external | Checks using a third-party service, such as Geoplugin and Virustotal |
| fuzzer | Launch fuzzing attacks |
| intrusive | Intrusive scripts such as brute-force attacks and exploitation |
| malware | Scans for backdoors |
| safe | Safe scripts that won’t crash the target |
| version | Retrieve service versions |
| vuln | Checks for vulnerabilities or exploit vulnerable services |

## Live Host Discovery

## Basic Port Scans

## Advanced Port Scans

## Post Port Scans

## Service Detection
