# Example Energy Cyber Incident Response Plan (CIRP)

## Purpose
Provide a structured, coordinated approach for identifying, managing, and recovering from cybersecurity incidents affecting Example Energy's IT and operational technology (OT). Because Example Energy supports nuclear, wind, coal, and natural gas operations, the plan prioritizes **operational continuity, public safety, and regulatory compliance**.

## Scope
All systems and data owned or operated by Example Energy: corporate IT, cloud, data centers, and OT supporting generation, monitoring, and distribution across the U.S. Any incident affecting the confidentiality, integrity, or availability (CIA) of these assets is in scope, as are external incidents that threaten operations or regulatory obligations. Applies to employees, contractors, and approved third parties.

## Standards Alignment
| Standard | Role in the CIRP |
|---|---|
| NIST CSF 2.0 | Overall structure (six Functions) |
| NIST SP 800-61 | Incident handling practice |
| NIST SP 800-53 | Preventive security controls |
| NERC CIP | Energy-sector regulatory expectations (including CIP-015) |

## Key Definitions
- **Cybersecurity Event:** a change that directly affects the operations or goals of an organization.
- **Cybersecurity Incident:** an event that puts the CIA of an information system in jeopardy.
- **Significant Cybersecurity Incident:** an event that puts CIA largely in jeopardy.
- **Operational Technology:** hardware and software that directly monitor and control physical devices, processes, and infrastructure.
- **Incident Response Team:** specialized group that detects, contains, mitigates, and recovers from threats or breaches.

## Business Context
Example Energy's mission is to secure operational systems and sensitive data, keep power available to hospitals, data centers, and public utilities, and align with energy-sector regulation (NERC CIP). A disruption can reach beyond the business into public safety and trust.

## IR Team Operating Structure
| Role | Responsibilities |
|---|---|
| CISO | Cybersecurity strategy, SOC oversight, regulatory and legal compliance, authorizes response at incident declaration |
| SOC Director | Liaison to CISO, manages security operations team |
| Threat Intelligence Manager | Gathers and analyzes threat data, turns it into actionable insight, stakeholder meetings |
| CIRT Members | Detection, analysis, and response to cyber threats |

**Charter:** the team may monitor and audit systems, networks, and traffic; investigate and respond to incidents, including containing and isolating threats; and collect, preserve, and analyze digital evidence. All actions must relate directly to incident response and follow organizational policy. Collected data is used solely for authorized purposes.

## How the CSF Functions Fit Together
**Govern, Identify, Protect, and Detect** operate continuously and support **Respond** and **Recover**, which are deployed when an incident occurs. Each Function produces outcomes the others consume.

## Contents
| Function / Topic | File |
|---|---|
| Identify (severity, incident types, classification) | [identify.md](identify.md) |
| Protect | [protect.md](protect.md) |
| Govern (data classification) | [govern.md](govern.md) |
| Detect | [detect.md](detect.md) |
| Respond (workflow, evidence handling) | [respond.md](respond.md) |
| Recover (strategy, eradication) | [recover.md](recover.md) |
| Post-incident lessons learned | [lessons-learned.md](lessons-learned.md) |
| Cross-function examples | [how-the-functions-connect.md](how-the-functions-connect.md) |
