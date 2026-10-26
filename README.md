# Example Energy IR: Incident Response Plan and Incident Investigations

> Academic cybersecurity project, completed as group work.
> A NIST CSF 2.0-aligned **Cyber Incident Response Plan (CIRP)** for a fictional critical-infrastructure energy provider, plus **hands-on incident investigations** performed in a simulated enterprise network using Wazuh, Suricata, and Velociraptor.

---

## Executive Summary

**In short:** an attacker got into a fictional energy company's network using real employee and administrator passwords, opened hidden ways back in, probably copied data out, and replaced the homepage on two of the company's three websites. Our team traced every step of the attack minute by minute, found the root cause (passwords that were never changed), and produced a response plan, a reusable playbook, and 12 concrete fixes.

### The scenario
"Example Energy Co." is a fictional U.S. energy company that runs power plants (nuclear, wind, coal, and natural gas). Because outages at an energy company can affect hospitals and the public, a cyberattack here is treated as high-stakes. Our team acted as the company's security team and did two things:

1. **Wrote the rulebook:** a full Cyber Incident Response Plan explaining how the company should prepare for, detect, handle, and recover from attacks.
2. **Investigated a real (simulated) attack:** inside an isolated lab network built to look like a company network, using professional security monitoring tools.

### What happened
| When | What the attacker did (plain language) |
|---|---|
| **Day 1** | Scanned the network from outside to see which computers and services were available. Tried guessing passwords on a decoy server (a "honeypot" set up to catch intruders) and failed. Then logged into a real web server **on the first try** using an employee's actual password, and changed a setting so the all-powerful "root" account could log in remotely. |
| **Day 8** | Came back. On the **Linux web server**: installed a personal access key (a backdoor that works even if passwords change), likely copied files out, deleted files, and replaced the website's homepage. On the **Windows side**: logged in with the company's top administrator account, created a fake user account and gave it extra permissions, jumped to a second web server, and replaced that website's homepage too. The whole Day 8 attack took about 15 minutes. |

**End result:** two of the company's three public websites displayed the attacker's message instead of the real content, and company data was likely taken.

### How we found out
We used three monitoring tools that work like a building's security systems:

| Tool | Everyday comparison | What it showed us |
|---|---|---|
| **Suricata** | Security cameras on the hallways (the network) | Scanning, login attempts, large file transfers |
| **Wazuh** | Central alarm panel | Logins, permission use, new accounts, honeypot alerts |
| **Velociraptor** | Forensic investigator inside each computer | Exactly which commands were run, and when |

By combining all three, we rebuilt a minute-by-minute timeline of the attack.

### Why it happened
**No hacking skill was needed. The attacker simply knew the passwords.** Passwords that former or unauthorized insiders knew were never changed. Two other weaknesses made it worse: an ordinary user account had administrator-level power it didn't need, and remote logins to servers required only a password (no second verification step).

### Impact
| Area | What was affected |
|---|---|
| **Confidentiality** (keeping data private) | At least three systems and their accounts were accessed; data was likely copied out |
| **Integrity** (keeping data accurate) | Content of two public websites was replaced |
| **Availability** (keeping services running) | Two of three websites stopped serving their real content |

### Results and deliverables
- **A complete Incident Response Plan** built on NIST CSF 2.0, the U.S. government's standard cybersecurity framework.
- **A full investigation report** with timeline, root cause, and impact.
- **A reusable step-by-step playbook** for handling website defacement attacks.
- **17 attacker techniques identified** and mapped to MITRE ATT&CK, the industry-standard catalog of attacker behavior.
- **A list of warning signs** (indicators of compromise) that defenders can watch for.
- **12 prioritized recommendations** to prevent this from happening again.

### Top recommendations
1. **Change all shared and old passwords**, and disable accounts of people who have left.
2. **Require two-step verification (MFA)** for all remote logins to servers.
3. **Give users only the access they need**, and remove admin rights from accounts that don't need them.
4. **Alert automatically** when website files, login settings, or user accounts are changed.
5. **Separate the web servers from the core network** so an attacker can't jump between them easily.

### Key lesson learned
Our three tools recorded times in different time zones, which made rebuilding the timeline harder than it needed to be. Every log should use one standard time (UTC). We also learned that a decoy server catching failed password guesses is useful, but those alerts must be connected to later *successful* logins from the same source. Here, the noisy failures on the decoy hid a quiet success on a real server.

### Want more detail?
| If you want to... | Read |
|---|---|
| See the full attack timeline and analysis | [`incident-reports/incident-2-web-defacement.md`](incident-reports/incident-2-web-defacement.md) |
| See the raw evidence from Day 1 | [`incident-reports/earlier-activity-evidence.md`](incident-reports/earlier-activity-evidence.md) |
| Read the company's Incident Response Plan | [`docs/cirp/`](docs/cirp/) |
| Follow the step-by-step response playbook | [`playbooks/web-defacement-playbook.md`](playbooks/web-defacement-playbook.md) |
| See the attacker techniques mapped to MITRE ATT&CK | [`detection/mitre-attack-mapping.md`](detection/mitre-attack-mapping.md) |
| See the warning signs (IOCs) | [`detection/iocs.md`](detection/iocs.md) |
| See all 12 recommendations | [`detection/detection-recommendations.md`](detection/detection-recommendations.md) |
| See the lab setup and tools | [`LAB-ENVIRONMENT.md`](LAB-ENVIRONMENT.md) |

---

## Technical Overview

Example Energy Co. is a fictional U.S. energy-sector company protecting nuclear, wind, coal, and natural gas operations (IT and OT). This repository contains:

1. **A complete CIRP** built on NIST CSF 2.0 (Govern, Identify, Protect, Detect, Respond, Recover), with severity scoring, incident taxonomy, data classification, response workflow, evidence handling, recovery, eradication, and lessons learned.
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

## Key Findings (Incident 2: Web Defacement)

- **Attack vector:** Insider threat. Credentials for `user-a` (Ubuntu) and `CORP\Administrator` (domain) were used without guessing or brute forcing.
- **Root cause:** Poor credential hygiene. Passwords known to former or unauthorized insiders were never rotated.
- **Kill chain:** ICMP sweep and Nmap scans, then SSH to LINUX-WEB-01 as `user-a`, then `PermitRootLogin` enabled and an SSH key planted, then SFTP exfiltration, then Apache `index.html` defaced. In parallel, RDP to WIN-DC-01 as domain admin, creation of rogue user `rogue-user`, and a pivot to WIN-IIS-01 where the IIS start page was defaced.
- **Impact:** Confidentiality, integrity, and availability were all affected. Two of three public sites were defaced and data was likely exfiltrated.
- **First IoC:** `user-a` logon on LINUX-WEB-01, seen independently by Suricata, Wazuh, and Velociraptor.

## Skills Demonstrated

Incident response lifecycle | NIST CSF 2.0 | MITRE ATT&CK mapping | SIEM analysis (Wazuh) | Network IDS (Suricata) | Endpoint forensics and threat hunting (Velociraptor) | Evidence handling and chain of custody | Playbook authoring | Windows and Linux log analysis | Root cause analysis | Security governance and reporting (NERC CIP awareness)

## Author

Yousif Alqunfuzi | [LinkedIn](https://www.linkedin.com/in/yousif-alqunfuzi) | yousifalqunfuzi@gmail.com

## Disclaimer

All systems, accounts, IP addresses, and the company "Example Energy Co." are **fictional or belong to an isolated academic lab**. No real organization or production data is involved. This repository is a portfolio piece, not a template to submit as coursework.

This work was completed as a **group project**. I am not the sole owner of the work.
