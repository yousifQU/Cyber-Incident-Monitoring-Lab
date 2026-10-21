# CSF: Govern

Govern provides oversight, accountability, and decision-making: who may decide during an incident, when to escalate to executives, legal, or regulators (notably **NERC CIP**), and how oversight continues through the incident lifecycle.

## Data Classification
| Level | Description | Examples | Protections |
|---|---|---|---|
| Public | Approved for public release | IIS and Apache public websites, non-sensitive data | None required |
| Private | Internal use; minor impact if released | Internal procedures, employee login page | Standard authentication, network access controls |
| Restricted | Could affect security and operations if released | Network configurations, vulnerability reports | Role-based access, network segmentation |
| Restricted-Specific | Highest sensitivity; strict approval only (IR team, CISO, legal) | Admin credentials, firewall rules, logs, private certificates and keys | Encryption, MFA, continuous monitoring |

Classification drives communication: if only general logs are involved, notification can stay internal; if credentials (e.g., Active Directory) are exposed, the classification rises and limits what can be shared.
