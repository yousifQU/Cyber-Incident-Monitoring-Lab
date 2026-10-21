# Earlier Activity: Day 1 Evidence Log

Evidence compiled from **Wazuh** and **Suricata** for activity attributed to `203.0.113.3` (and a related `203.0.113.5`). This is the raw-evidence companion to the analysis in [Incident 2](incident-2-web-defacement.md).

> **Timestamp caution:** Suricata shows roughly 04:57 to 05:13 local console times while Wazuh shows 05:10 to 05:13 for related events, so the tools' clocks or zones are not aligned. Times below are as displayed by each tool. This mismatch directly motivated the "normalize logs to UTC" lesson learned.

## Suricata (Network)
| Time | Observation |
|---|---|
| 04:57:12 to 04:57:15 | ICMP ping sweep from `203.0.113.3` against `10.10.10.10` through `.80` (ET/PROTOCOL-ICMP PING) |
| 04:57:18 | `ET SCAN Potential SSH Scan` against `10.10.10.10:22` |
| 04:57:22 | `ET SCAN Behavioral Unusually fast Terminal Server Traffic` toward `10.10.10.50:3389` |
| 04:57:39 to 04:57:44 | `ET SCAN RDP Connection Attempt from Nmap` and Microsoft Terminal Server request alerts across `.10`, `.20`, `.40`; MS Terminal Server traffic on non-standard port 443 |
| 04:57:52 | `ET SCAN Nmap Scripting Engine User-Agent Detected` against HTTP (port 80) on multiple hosts |
| 04:59:43 to 04:59:56 | Same NSE detections against `10.10.10.20:443` |
| 05:10:18 to 05:13:43 | `INDICATOR-SHELLCODE ssh CRC32 overflow filler` against SSH (22) on `10.10.10.40` and `.60` |
| 06:15:49 to 06:15:56 | RDP requests to `10.10.10.10:3389` from `203.0.113.5` |
| 18:38 to 18:49 | Later ICMP pings (`GPL ICMP PING *NIX`) at `10.10.10.40` and further SSH overflow attempts against `.40`, `.50`, `.60` |

## Wazuh (Host)
| Time | Host | Event |
|---|---|---|
| 05:10:19 | WIN-IIS-01 | Special privileges assigned to new logon (event 4672) for `CORP\Administrator`: privilege escalation indicators (`SeDebugPrivilege`, `SeBackupPrivilege`, and others) |
| 05:10:26 / 05:10:30 | LINUX-HONEYPOT (honeypot) | Repeated SSH password guessing as `root` from `203.0.113.3`; attempted passwords captured in plaintext by the honeypot |
| 05:11:07 | LINUX-WEB-01 | `sshd` authentication success for user `user-a` from `203.0.113.3` (MITRE: Valid Accounts, Remote Services) |
| 05:11:07 | LINUX-WEB-01 | Command line launched on the host |
| 05:11:23 | LINUX-WEB-01 | First-time `sudo` use: `sed` modifies `PermitRootLogin` in `/etc/ssh/sshd_config` (MITRE T1548.003) |
| 05:11:23 | LINUX-WEB-01 | PAM login session closed (logoff) |
| 05:11:32 | WIN-DC-01 | Kerberos authentication attempt |
| 05:12:53 | WIN-DC-01 | PowerShell launched; temporary `.ps1` file created in `C:\Windows\Temp` by `NT AUTHORITY\SYSTEM` |
| 05:13:16 | WIN-IIS-01 | Schannel event 36874: TLS 1.2 connection rejected, no cipher suite in common |

## Observations
- The honeypot caught the *noisy* guessing against LINUX-HONEYPOT while the real `user-a` login on LINUX-WEB-01 succeeded on the first try. That contrast supports the insider-knowledge hypothesis.
- The `PermitRootLogin` change appears in both the Day 1 Wazuh data and the Day 8 Velociraptor data, suggesting repeat or persistent access.
- The source address resembles ranges used for legitimate VPN access, per the report; attribution beyond that was not established.
