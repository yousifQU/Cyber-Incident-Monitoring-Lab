# Web Defacement Incident Response Playbook

Derived from the playbook written for the web-defacement investigation. Each step lists the procedure, then what to do or look for. Lab findings are shown as *worked examples*.

## 1. Determine Investigation Scope
| Step | Procedure | Worked example |
|---|---|---|
| 1a | Identify incident type | Defacement of Apache2 (LINUX-WEB-01) and IIS (WIN-IIS-01) sites |
| 1b | Assess mission and information impact | 2 of 3 sites defaced; possible data exfiltrated from LINUX-WEB-01 |
| 1c | Identify at-risk assets | Web servers, domain controller, wider domain |
| 1d | Identify weaknesses | Unrotated/known passwords, unnecessary `sudo`, effortless RDP as domain admin |
| 1e | Identify threats | Insider or ex-insider, hacktivists, outside collaborator |

## 2. Perform Technical Analysis
| Step | Procedure |
|---|---|
| 2a | Form a hypothesis of adversary goals using findings and CTI |
| 2b | Collect key logs: Suricata alerts/eve logs (flows, large transfers), Wazuh (sudo, user adds, honeypot), Velociraptor (process execution, RDP auth, Sysmon) |
| 2c | Assess affected systems and networks for adversary behavior |
| 2d | Compare against network, user-behavior, file-hash, and log-volume baselines |
| 2e | Determine root cause |
| 2f | Determine attack vector |
| 2g | Identify targeted services (SSH, RDP, Apache2, IIS) |
| 2h | Identify targeted systems and pivot paths |
| 2i | Identify the first IoC |

## 3. Analyze Adversary TTPs
Map observed behavior to ATT&CK tactics. Full table with evidence: [`detection/mitre-attack-mapping.md`](../detection/mitre-attack-mapping.md). Record "no evidence found" for tactics with no artifacts rather than skipping them.

## 4. Containment
1. **Short-term:** limit access to affected hosts, preserve logs, avoid tipping off the actor, confirm remaining services can sustain the mission, invoke the Business Continuity Plan if needed.
2. **Monitor** for attacker reaction: increase logging verbosity on targeted hosts and services (SSH, Apache2, Sysmon); restrict abnormal interactions.
3. **Terminating condition:** no new compromise signs; preserve evidence for law enforcement if applicable; tune detection tools; verify log viability; remediate hosts; document; proceed to eradication.

## 5. Short Summary (CIA Triad)
| Pillar | Question |
|---|---|
| Confidentiality | What systems, accounts, services, and data were accessed? |
| Integrity | What content or configuration was modified? |
| Availability | What service was lost or degraded? |

## 6. Lessons Learned
Hold the post-incident review (3 to 5 days later), then update policies, detections, playbooks, and training. See [`docs/cirp/lessons-learned.md`](../docs/cirp/lessons-learned.md).

## Quick-Reference Checklist
- [ ] Preserve logs and hash evidence before touching hosts
- [ ] Check `/etc/ssh/sshd_config`, `authorized_keys`, and local groups
- [ ] Review new local/domain users and group changes
- [ ] Review web content roots and server restarts (`apache2`, `iisreset`)
- [ ] Look for `sftp-server`/`scp` and large outbound flows
- [ ] Rotate credentials and revoke keys
- [ ] Restore content from known-good backups and verify hashes
