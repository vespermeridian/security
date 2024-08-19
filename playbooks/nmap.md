# Nmap Playbook

| mode | syntax | Notes |
| ---- | ------ | ----- |
| TCP Syn Scan | -sS | Half open, or 'stealth' scan. Faster. |
| TCP Connect Scan | -sT | Resp. of RST = closed. No response = filtered or firewall. Syn/Ack = open. |
| UDP Scan | -sU | Less reliable because stateless. Port closed if ICMP response is received. |
| Version Scan | -sV | Probe open ports to determine service/version info. |
| Aggressive Scan | -A | Enables OS detection (-O), version scanning (-sV), script scanning (-sC) and traceroute (–traceroute). |
| Null Scan | -sN | The TCP packets sent don’t have any of the flags set. The target should respond back with an RST if the port is closed. |
| Fin Scan | -sF | Like Null Scan except it sends a completely empty TCP packet, it sends a packet with the FIN flag set which is used to gracefully close a connection. The target must respond back with an RST for closed. |
| Xmas Scan | -sX | Sends TCP packets with the PSH, URG and FIN flags set. Like the last two scan types, this too expects RST packets for closed ports. |

## Live Host Discovery

## Basic Port Scans

## Advanced Port Scans

## Post Port Scans

## Service Detection
