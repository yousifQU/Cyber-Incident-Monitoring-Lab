# Example Energy IR: Incident Response Plan and Incident Investigations

> Academic capstone-style cybersecurity project.
> A NIST CSF 2.0-aligned **Cyber Incident Response Plan (CIRP)** for a fictional critical-infrastructure energy provider, plus **two hands-on incident investigations** performed in a simulated enterprise network using Wazuh, Suricata, and Velociraptor.

![Type](https://img.shields.io/badge/type-DFIR%20portfolio-blue) ![Framework](https://img.shields.io/badge/framework-NIST%20CSF%202.0-green) ![ATT&CK](https://img.shields.io/badge/MITRE-ATT%26CK-red) ![TLP](https://img.shields.io/badge/TLP-CLEAR-lightgrey)

## Overview

Example Energy Co. is a fictional U.S. energy-sector cybersecurity company protecting nuclear, wind, coal, and natural gas operations (IT and OT). This repository contains:

1. **A complete CIRP** built on NIST CSF 2.0 (Identify, Protect, Govern, Detect, Respond, Recover), with severity scoring, incident taxonomy, data classification, response workflow, evidence handling, recovery, eradication, and lessons learned.
2. **Incident investigations** in a live lab environment: reconnaissance, insider-style credential abuse, SSH/RDP lateral movement, persistence, data exfiltration, and web defacement of both Apache (Linux) and IIS (Windows) servers.
3. **A reusable web-defacement response playbook**, MITRE ATT&CK mappings, an IOC list, and detection recommendations derived from what the investigations found.

## Repository Map

| Path | Contents |
|---|---|
| [`docs/cirp/`](docs/cirp/) | The CIRP, broken into one file per CSF function and supporting tables |
| [`incident-reports/incident-2-web-defacement.md`](incident-reports/incident-2-web-defacement.md) | Full investigation: scope, timeline, root cause, TTPs, CIA impact |
| [`incident-reports/earlier-activity-evidence.md`](incident-reports/earlier-activity-evidence.md) | Evidence log from the earlier activity (Wazuh + Suricata) |
| [`playbooks/web-defacement-playbook.md`](playbooks/web-defacement-playbook.md) | Step-by-step IR playbook (scope, analysis, TTPs, containment, summary) |
| [`detection/mitre-attack-mapping.md`](detection/mitre-attack-mapping.md) | Technique-by-technique mapping with evidence sources |
| [`detection/iocs.md`](detection/iocs.md) | Indicators of compromise and behavioral indicators |
| [`detection/detection-recommendations.md`](detection/detection-recommendations.md) | Detection and hardening improvements |
| [`LAB-ENVIRONMENT.md`](LAB-ENVIRONMENT.md) | Hosts, tools, and data sources used |
| [`assets/screenshots/`](assets/screenshots/) | Place for sanitized evidence screenshots |

## Key Findings (Incident 2: Web Defacement)

- **Attack vector:** Insider threat. Credentials for `user-a` (Ubuntu) and `CORP\Administrator` (domain) were used without guessing or brute forcing.
- **Root cause:** Poor credential hygiene. Passwords known to former or unauthorized insiders were never rotated.
- **Kill chain:** ICMP sweep and Nmap scans, then SSH to LINUX-WEB-01 as `user-a`, then `PermitRootLogin` enabled and an SSH key planted, then SFTP exfiltration, then Apache `index.html` defaced. In parallel, RDP to WIN-DC-01 as domain admin, creation of rogue user `rogue-user`, and a pivot to WIN-IIS-01 where the IIS start page was defaced.
- **Impact:** Confidentiality, integrity, and availability were all affected. Two of three public sites were defaced and data was likely exfiltrated.
- **First IoC:** `user-a` logon on LINUX-WEB-01, seen independently by Suricata, Wazuh, and Velociraptor.

## Skills Demonstrated

Incident response lifecycle (NIST SP 800-61) | NIST CSF 2.0 | MITRE ATT&CK mapping | SIEM analysis (Wazuh) | Network IDS (Suricata) | Endpoint forensics and threat hunting (Velociraptor) | Evidence handling and chain of custody | Playbook authoring | Windows and Linux log analysis | Root cause analysis | Security governance and reporting (NERC CIP awareness)

## Author

[Your Name] | [LinkedIn] | [Email]

## Disclaimer

All systems, accounts, IP addresses, and the company "Example Energy Co." are **fictional or belong to an isolated academic lab**. No real organization or production data is involved. This repository is a portfolio piece, not a template to submit as coursework.

## License

Documentation released under [CC BY 4.0](LICENSE).
