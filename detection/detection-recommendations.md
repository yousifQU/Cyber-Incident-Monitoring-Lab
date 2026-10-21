# Detection and Hardening Recommendations

Derived from findings and lessons learned. These are recommendations, not deployed rules.

| # | Gap observed | Recommendation | Maps to |
|---|---|---|---|
| 1 | Stale and shared credentials enabled both intrusions | Rotate all admin and service passwords; disable ex-employee accounts; periodic access reviews | Protect, Eradication |
| 2 | No MFA on SSH or RDP | Enforce MFA, restrict RDP/SSH to a jump host or VPN with IP allow-listing | Protect |
| 3 | `user-a` had unnecessary `sudo` | Apply least privilege; remove from `sudo` group | Protect |
| 4 | `sshd_config` change went unnoticed | File Integrity Monitoring on `/etc/ssh/sshd_config` and `authorized_keys`; alert on `PermitRootLogin` changes | Detect (T1098.004) |
| 5 | Rogue account created on a DC | Alert on new user and group-membership events (4720, 4732) outside change windows | Detect (T1098.007) |
| 6 | Defacement not detected as it happened | FIM on web roots (`/var/www/html`, `C:\inetpub\wwwroot`); alert on web-server restarts | Detect, Recover |
| 7 | SFTP exfiltration | Alert on `sftp-server` spawned from interactive shells; monitor large outbound flows in Suricata eve logs | Detect (T1048.002) |
| 8 | Lateral movement via RDP | Alert on RDP from servers (not admin workstations) and on `mstsc.exe` launched on servers | Detect (T1021.001) |
| 9 | Honeypot saw noisy guessing but real login succeeded silently | Correlate honeypot hits with later successful logins from the same source | Detect |
| 10 | Logs in mixed time zones | Normalize to UTC across Wazuh, Suricata, Velociraptor | Lessons learned |
| 11 | Flat network allowed pivots | Segment web servers from the domain controller; restrict admin protocols | Eradication |
| 12 | Backups reachable from main network | Isolate backup systems | Recover |
