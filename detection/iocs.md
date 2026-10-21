# Indicators of Compromise

> All values come from an isolated academic lab and are not real-world threat intelligence.

## Network
| Indicator | Context |
|---|---|
| `203.0.113.2` | ICMP sweep and Nmap activity (Day 8) |
| `203.0.113.3` | Scanning, RDP/SSH probing, SSH overflow-filler alerts, `user-a` login (Day 1) |
| `203.0.113.5` | RDP requests to `10.10.10.10:3389` |
| Suricata SIDs/rules | `ET SCAN RDP Connection Attempt from Nmap`, `ET SCAN Nmap Scripting Engine User-Agent Detected`, `ET SCAN Potential SSH Scan`, `INDICATOR-SHELLCODE ssh CRC32 overflow filler`, `GPL ICMP PING *NIX` |

## Host Artifacts
| Artifact | Host | Meaning |
|---|---|---|
| `PermitRootLogin yes` in `/etc/ssh/sshd_config` | LINUX-WEB-01 | Persistence/privilege change |
| `/home/user-a/.ssh/authorized_keys` (new) | LINUX-WEB-01 | Attacker public key |
| `sftp-server` spawned via `bash -c` by `user-a` | LINUX-WEB-01 | Probable exfiltration |
| Modified `/var/www/html/index.html`, `apache2` restart | LINUX-WEB-01 | Defacement |
| Deleted `/var/run/apache2/*ssl_scache*` | LINUX-WEB-01 | File removal |
| User `rogue-user` and membership in Print Operators | WIN-DC-01 | Rogue account |
| `mstsc.exe` run by `CORP\Administrator` | WIN-DC-01 | RDP pivot |
| Modified `C:\inetpub\wwwroot\iisstart.htm`, `iisreset.exe` | WIN-IIS-01 | IIS defacement |
| `C:\Windows\Temp\*.ps1` by SYSTEM (Day 1) | WIN-DC-01 | Script staging |

## Behavioral
- Successful login on the **first attempt** for an account that never previously logged in from that source
- Admin commands (`sed` on `sshd_config`, `systemctl restart sshd`) at unusual hours
- Account and group changes on a domain controller outside change windows
