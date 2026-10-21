# CSF: Respond

Once an alert escalates to **Level 3 (Incident Declaration)**, the **CISO authorizes** the CIRT to begin coordinated response. Data sources: Wazuh (SIEM), Velociraptor (EDR), Suricata (IDS), OpenCanary (deception).

Key activities: investigation (scope, root cause, affected systems), containment (isolate endpoints, block malicious IPs at the firewall, disable affected accounts), stakeholder notification, and thorough documentation in the incident management system.

## Response Workflow
| Stage | Description | Responsible | Objective |
|---|---|---|---|
| Incident Confirmation | Validate alerts in the SIEM against multiple sources and baselines | SOC Analyst | Confirm incident, activate response |
| Initial Containment | Quarantine or isolate compromised systems; segmentation lets teams triage in parallel | IRT, SOC Analyst | Stop spread |
| Investigation and Analysis | Determine what was accessed, how, and by whom | IRT, SOC Analyst | Define scope |
| Eradication | Remove malicious artifacts (malware, accounts, software), harden systems | IRT, Sys Admin | Remove threat |
| Recovery Coordination | Restore downed services to full capacity | IT Techs, Sys Admins | Restore operations |
| Documentation | Record discoveries, remediation, recovery; build playbooks | IRT, SOC Analyst, SOC Coordinator | Streamline future response |

## Evidence Handling and Documentation
- Preserve data **immediately** after confirmation; never alter originals; analyze forensic copies in a secure environment.
- **Hash** collected files, keep originals read-only, and maintain a **chain of custody** (who collected, accessed, analyzed).
- Keep a detailed timeline: timestamps, escalation actions, recovery milestones.

| Evidence | Source | Handling |
|---|---|---|
| SIEM logs | Wazuh | Preserve originals, export copies |
| Network traffic | Suricata | Capture PCAP when possible |
| Endpoint artifacts | Velociraptor | Acquire forensic images, avoid changing system state |
| Firewall logs | Perimeter firewalls | Retain inbound/outbound traffic |
| Authentication logs | Active Directory | Preserve logins and privilege escalation events |
| Deception alerts | OpenCanary | Correlate with other logs |
| System event logs | Wazuh | Export immediately to avoid overwrite |
| Malware samples | Infected systems | Isolate in a sandbox |
