# MITRE ATT&CK Mapping: Incident 2

| Tactic | Technique | ID | Evidence |
|---|---|---|---|
| Reconnaissance | Active Scanning: Scanning IP Blocks | T1595.001 | Suricata ICMP sweep of `10.10.10.10` to `.80` |
| Reconnaissance | Gather Victim Host Information | T1592 | `user-a` ran `uname -o/-r/-m` (Velociraptor) |
| Reconnaissance | Active Scanning: Vulnerability Scanning | T1595.002 | `nmap -sV 10.10.10.20` (Wazuh, Velociraptor) |
| Initial Access | External Remote Services | T1133 | Attacker IP resembles VPN ranges (Suricata) |
| Initial Access | Valid Accounts: Local Accounts | T1078.003 | `user-a` on LINUX-WEB-01 (member of `sudo`) |
| Initial Access | Valid Accounts: Domain Accounts | T1078.002 | `CORP\Administrator`, Domain Admin |
| Execution | Unix Shell | T1059.004 | Interactive bash for `user-a` and `root` (`/etc/passwd`) |
| Execution | PowerShell | T1059.001 | PowerShell on WIN-IIS-01 (Velociraptor) |
| Execution | Windows Command Shell | T1059.003 | `cmd.exe` on WIN-DC-01 (Velociraptor) |
| Persistence | Account Manipulation: SSH Authorized Keys | T1098.004 | `authorized_keys` created; `sshd_config` edited |
| Privilege Escalation | Account Manipulation: Additional Local or Domain Groups | T1098.007 | `rogue-user` added to Print Operators |
| Defense Evasion | None identified | | |
| Credential Access | None identified | | |
| Discovery | Software Discovery | T1518 | `nmap -sV` against WIN-IIS-01 |
| Lateral Movement | Remote Services: RDP | T1021.001 | `mstsc.exe` on WIN-DC-01; repeated WIN-IIS-01 logons from WIN-DC-01's IP |
| Collection | None identified | | |
| Command and Control | None identified | | |
| Exfiltration | Exfiltration Over Asymmetric Encrypted Non-C2 Protocol | T1048.002 | `sftp-server` launched by `user-a` |
| Impact | Data Destruction | T1485 | `rm -f` of files on LINUX-WEB-01 |
| Impact | Defacement: External Defacement | T1491.002 | `index.html` (Apache) and `iisstart.htm` (IIS) modified |

Additional technique seen in Wazuh on Day 1: **Sudo and Sudo Caching (T1548.003)** when `user-a` used `sudo` to edit `sshd_config`.
