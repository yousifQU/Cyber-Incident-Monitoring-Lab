# Lab Environment

## Network
Simulated enterprise domain `corp.example` on the `10.10.10.0/24` segment.

| Host | Role | Notes |
|---|---|---|
| LINUX-WEB-01 (`LINUX-WEB-01`) | Linux web server (Apache2) | `user-a` account in `sudo` group; targeted for SSH access, persistence, exfiltration, defacement |
| WIN-DC-01 (`WIN-DC-01`, 10.10.10.10) | Windows Server / Domain Controller | `CORP\Administrator` is Domain Admin; used as pivot |
| WIN-IIS-01 (`WIN-IIS-01`, 10.10.10.20) | Windows Server running IIS | Public website defaced |
| LINUX-HONEYPOT (`LINUX-HONEYPOT`) | Linux honeypot (OpenCanary) | Recorded SSH password-guessing attempts |

Attacker source addresses observed in Suricata: `203.0.113.2`, `203.0.113.3`, `203.0.113.5`. The report notes these resemble ranges used for legitimate VPN remote access.

## Defensive Tooling

| Tool | Purpose | What it provided |
|---|---|---|
| **Wazuh** (SIEM) | Log correlation and alerting | sudo usage, user additions, auth success, honeypot attempts |
| **Suricata** (NIDS) | Network detection | `alert.log` and `eve.json`: scans, RDP/SSH probes, large flows suggesting file transfer |
| **Velociraptor** (EDR/DFIR) | Endpoint hunting | Process executions, RDP authentication logs, Sysmon events |
| **OpenCanary** | Deception | Honeypot alerts on LINUX-HONEYPOT |
| **Sysmon** | Windows telemetry | Process creation and command lines |

## Timestamp Caveat
Wazuh, Suricata, and Velociraptor screenshots show timestamps in different time zones or formats. This is itself a documented lesson learned in the CIRP: **normalize all logs to UTC**.
