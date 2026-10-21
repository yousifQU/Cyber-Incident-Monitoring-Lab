# CSF: Identify

Identify gives responders the understanding of assets, dependencies, and risk needed to classify incidents correctly. Misclassification or delayed escalation can cause extended regional outages, unsafe operating conditions, and loss of public trust.

## Severity Factors
| Factor | Low | Moderate | High |
|---|---|---|---|
| Operational impact | No noticeable impact | Partial disruption to non-essential services or short-term failure | Critical services disrupted, outages, financial loss |
| Duration | Resolved in hours | One or more business days, coordinated response | Multiple days or weeks, major restoration |
| Regulatory/compliance | None violated | Internal compliance broken | Regulatory/legal violations, potential fines |
| Threat actor capability | Automated, limited knowledge | Targeted, known tools | Advanced, deliberate, adapts to defenses |
| Affected assets | Single machine | Multiple systems in a segment | Widespread, may include critical systems |
| Recovery difficulty | Easily restored | Coordinated effort and reconfiguration | Complete rebuilds with forensic validation |
| Data integrity impact | No data altered | Limited changes to non-critical systems | Unauthorized alteration of critical systems |
| Client disclosure | Internal only | Limited customer/partner notice | Public disclosure, media involvement |

## Incident Types
| Type | Description |
|---|---|
| Spear Phishing | Targeted social engineering of privileged users to obtain credentials or induce actions |
| Data Exfiltration | Malware or unauthorized access used to steal secrets/IP for leaking or extortion |
| Insider Threat | Current or past employee accesses unauthorized resources, intentionally or not (e.g., unrevoked credentials) |
| IoT Device Compromise | Compromised devices used for botnets, monitoring, or lateral movement |
| Malware Infection | Programs that disrupt, destroy, or gain unauthorized access |
| Credential Theft | Unauthorized acquisition via breaches, brute force, social engineering, weak policy |
| Privilege Escalation | Low-privilege access elevated via exploits or stolen privileged credentials |
| Lateral Movement | Attacker moves between systems using exploits or stolen credentials |
| Denial of Service | Making servers, networks, or sites unavailable, typically via excess traffic |
| Web Application Attack | Exploiting web server/app vulnerabilities, commonly tied to defacement |
| Man in the Middle | Attacker intercepts a conversation between two systems |
| Ransomware | Malware encrypting data and extorting for the key |

## Incident Classification
| Level | Name | Impact | Example |
|---|---|---|---|
| 0 | Event Triage | Minor performance issues | Broken internal hyperlink |
| 1 | Minor | Operational inconvenience, possible non-essential disruption | Non-critical workstation down temporarily |
| 2 | Medium | Noticeable disruption, potential internal data exposure | Phished employee account |
| 3 | Major | Reduced critical functions, possible exfiltration | Breach of sensitive customer data |
| 4 | Critical | Immediate attention, severe operational and financial impact | Critical outage affecting all client-facing interfaces |

> **Applied example:** the web-defacement investigation (Incident 2) combined insider-threat credential abuse, lateral movement, exfiltration, and integrity loss across multiple servers, which aligns with Level 3 (Major) characteristics under this scheme.
