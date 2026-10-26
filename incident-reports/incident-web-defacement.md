# Incident: Web Defacement via Insider-Style Credential Abuse

**Context:** Academic lab exercise | **TLP:** CLEAR
**Activity date:** Day 8 of the exercise (Velociraptor timestamps in UTC)
**Systems:** LINUX-WEB-01 (Apache2), WIN-DC-01 (domain controller), WIN-IIS-01 (IIS)

## Executive Summary
Two of the organization's three public websites, the Apache2 server on LINUX-WEB-01 and the IIS server on WIN-IIS-01, were defaced with content mocking the organization over a sports result. The attacker logged into LINUX-WEB-01 as `user-a` and into WIN-DC-01 and WIN-IIS-01 as `CORP\Administrator` without any guessing or brute forcing, which points to prior knowledge of the credentials. On LINUX-WEB-01 the attacker enabled root SSH login, planted an SSH key for persistence, launched an SFTP server (likely exfiltration), removed files, and rewrote `index.html`. On Windows the attacker created a rogue account (`rogue-user`), added it to Print Operators, pivoted from WIN-DC-01 to WIN-IIS-01 over RDP, edited the IIS start page, and reset IIS. The root cause was poor password control: credentials known to ex-employees or unauthorized insiders were never changed.

## Scope and Impact
| Question | Finding |
|---|---|
| Incident type | Web defacement of two public sites (Apache2 on LINUX-WEB-01, IIS on WIN-IIS-01) |
| Mission impact | Only one of three sites remained intentionally operational; severe if these were public-facing services. Data was apparently exfiltrated from LINUX-WEB-01 (contents unknown). |
| At-risk assets | LINUX-WEB-01 Apache2, WIN-DC-01 and the wider domain, WIN-IIS-01 IIS |
| Weaknesses | `user-a` password accepted without guessing; domain admin RDP to WIN-DC-01 and WIN-IIS-01 was effortless; `user-a` held unnecessary `sudo` rights |
| Threats considered | Current or former insider; hacktivists; possible outside collaborator who knew the organization's affiliations (the defacement content referenced them) |

## Timeline (Day 8, UTC)
| Time | Event | Source |
|---|---|---|
| 07:45:27 | ICMP ping sweep from `203.0.113.2` across `10.10.10.10` to `.80` (Nmap host discovery) | Suricata |
| 07:45 to 07:49 | Repeated `CORP\Administrator` logons on WIN-IIS-01 from `10.10.10.10` (WIN-DC-01) | Velociraptor |
| 07:48:57 | `user-a` runs `uname -o/-r/-m` for system recon; `rm` of temp files in `/var/lib/update-notifier/` | Velociraptor |
| 07:49:18 | `user-a` sets `PermitRootLogin yes` in `/etc/ssh/sshd_config` and restarts `sshd` | Velociraptor |
| 07:49:24 | `user-a` creates `~/.ssh/authorized_keys` (modes 700/600) to plant a public key | Velociraptor |
| 07:50:03 | `root` removes `/var/run/apache2/*ssl_scache*`; `index.html` rewritten via a series of shell commands; `systemctl restart apache2` | Velociraptor |
| 07:50:08 | `user-a` runs `nmap -sV 10.10.10.20` (service versions on WIN-IIS-01) | Wazuh, Velociraptor |
| 07:50:42 | `user-a` launches `sftp-server` (probable exfiltration; `scp` also possible) | Velociraptor |
| 07:54:45 | On WIN-DC-01, `net user rogue-user ... /add` | Velociraptor |
| 07:55:14 | `net localgroup "Print Operators" rogue-user /add` | Velociraptor |
| 07:55:27 | `mstsc.exe` opened on WIN-DC-01 (RDP client) | Velociraptor |
| 07:56:11 | PowerShell and `iisreset.exe` activity on WIN-IIS-01 | Velociraptor |
| 07:58:33 | `notepad.exe` edits `C:\inetpub\wwwroot\iisstart.htm` on WIN-IIS-01 | Velociraptor |
| 07:59:12 | `iisreset.exe` restarts IIS | Velociraptor |

## Technical Analysis
- **Reconnaissance:** one internal scan of all hosts; WIN-IIS-01 specifically fingerprinted by LINUX-WEB-01 using `nmap -sV`.
- **Initial access:** SSH to LINUX-WEB-01 as `user-a`; RDP to WIN-DC-01 as `CORP\Administrator`; pivot to WIN-IIS-01 using the same credentials. `CORP\Administrator` is a Domain Admin and also in the Ubuntu `sudo` group via domain integration (`administrator@corp.example`).
- **Persistence and privilege:** `PermitRootLogin yes`, planted `authorized_keys`, rogue AD/local user `rogue-user`.
- **Exfiltration:** `sftp-server` spawned under `user-a`.
- **Impact:** file deletion (`rm`), defacement of `index.html` (LINUX-WEB-01) and `iisstart.htm` (WIN-IIS-01), service restarts.
- **First IoC:** `user-a` logon on LINUX-WEB-01 (Suricata, Wazuh, Velociraptor).
- **Not observed:** defense evasion, credential access, collection, and command-and-control artifacts were not identified.

### Baseline Deviations
| Baseline | Deviation |
|---|---|
| Network | Internal network scan and host-level scanning |
| User behavior | `user-a` ran unexpected commands at an unusual time |
| File hash | Critical service configuration (`sshd_config`) changed |
| Log volume | Wazuh showed visibly more logs during the attack |

## Root Cause and Vector
- **Root cause:** weak password controls; credentials known to former or unauthorized insiders were not rotated.
- **Attack vector:** insider threat.
- **Services targeted:** SSH and Apache2 (LINUX-WEB-01), RDP (WIN-DC-01, WIN-IIS-01), IIS (WIN-IIS-01).

## CIA Impact
| Pillar | Impact |
|---|---|
| Confidentiality | Unauthorized access to at least three systems, local and domain accounts, services, and the data they serve |
| Integrity | Content of both public websites modified |
| Availability | Loss of useful service from the LINUX-WEB-01 and WIN-IIS-01 web servers |

## Containment Plan
1. Restrict access to LINUX-WEB-01, WIN-DC-01, and WIN-IIS-01 to limit log contamination and avoid tipping off any active actor; preserve logs from all relevant sources.
2. Confirm the remaining web server can sustain the mission and decide whether to invoke the Business Continuity Plan.
3. Raise logging verbosity on affected hosts and services (SSH, Apache2, Sysmon) and limit abnormal interaction.
4. **Exit condition:** no new signs of compromise, then preserve evidence (for law enforcement if applicable), tune detections, verify log viability, remediate hosts, document everything, and move to eradication.

## Recommendations
See [`detection/detection-recommendations.md`](../detection/detection-recommendations.md). Highest priority: rotate all shared and legacy credentials, remove unnecessary `sudo` from `user-a`, enforce MFA on SSH and RDP, remove `rogue-user` and the planted key, and alert on `sshd_config` changes.
