# CSF: Detect

Detect turns baseline protections into real-time awareness using vendor-neutral capabilities:

| Capability | Lab tool |
|---|---|
| SIEM log correlation | Wazuh |
| Network IDS/IPS | Suricata |
| Endpoint telemetry and hunting | Velociraptor (with Sysmon) |
| Authentication monitoring | Windows/Linux auth logs |
| Deception | OpenCanary honeypot (LINUX-HONEYPOT) |

**Targets:** unauthorized access attempts, abnormal behavior, malware, unusual network traffic.

**Goal:** shorten the gap between compromise and response by detecting early, validating quickly, and routing alerts with enough context to the right responders. Consistent classification and escalation ties each alert to business impact. Weak or delayed detection lets attackers move laterally and escalate privileges.

**Baselines used in the investigations:** network behavior, user behavior (commands and timing), file hashes of critical configs, and log volume. See the [Incident 2 report](../../incident-reports/incident-2-web-defacement.md).
