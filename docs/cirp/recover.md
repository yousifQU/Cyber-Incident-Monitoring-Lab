# CSF: Recover

Recover returns systems to a **stable and trusted** state after containment, not merely "turning things back on." For an energy provider, systems tied to generation and monitoring must not return before they are verified clean.

After rebuild, systems are monitored for **two weeks to three months** depending on severity.

## Recovery Strategy
| Step | Description | Responsible | Validation |
|---|---|---|---|
| Backup Restoration | Restore from last known-good backup (OS images, configs) | Sys Admin, IT Tech | Verify integrity, timestamp relevance |
| System Rebuilding | Rebuild destroyed, attacker-added, or missing systems | Sys Admin, IT Tech | Match network design documentation |
| Integrity Verification | Compare files/databases to baseline hashes; revert deviations | Security Analyst, CIRT | Vulnerability scans, baseline hashes |
| Staged Reintroduction | Gradual deployment of remediated systems | Sys Admin, IT Tech | Reference CoOP and SOPs |
| Internal Telemetry Monitoring | Review system health and overlooked compromises | Security Analyst, SOC Manager | Compare to known-good baselines |
| External Telemetry Monitoring | Analyze client-side impact of public services | Security Analyst, SOC Manager | Compare to known-good baselines |

## Eradication
Eradication removes the root cause entirely. Systems move to recovery only after eradication is **verified**.

| Action | Root cause addressed | Verification |
|---|---|---|
| Malware removal | Phishing/exploit infection | Clean endpoint scans, no IoCs in logs |
| Patch management | Unpatched vulnerabilities | Scans show no critical/high findings |
| Account credential reset | Credential compromise, privilege escalation | No unauthorized auth attempts; MFA enforced |
| Configuration hardening | Misconfiguration | Audits confirm baseline compliance |
| Backdoor removal | Attacker persistence (rogue accounts, tasks) | Audits and log review show no unauthorized paths |
| Network segmentation enforcement | Lack of segmentation | Traffic analysis shows restricted zone access |

> **Applied to Incident 2:** eradication would include removing `rogue-user`, the planted SSH key, and the `PermitRootLogin yes` change, rotating `user-a` and `CORP\Administrator` credentials, and restoring both web content roots from known-good backups.
