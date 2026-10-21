# Post-Incident Lessons Learned

A formal review occurs **3 to 5 days after** an incident with legal, management, system administrators, and security analysts. Events before, during, and after are reviewed; findings update policies, detection rules, playbooks, and training.

| Category | Observation | Impact | Recommended improvement |
|---|---|---|---|
| Detection | Lateral movement not identified immediately | Attacker reached multiple systems | Implement network segmentation |
| Monitoring | DNS queries not centrally logged | Reduced visibility | Centralize DNS logs |
| Response | Backups reachable from the central network | Risk of backup destruction | Isolate backup systems |
| Governance | No integrity standards | Tampering may go unnoticed | Create baselines for all systems |
| Training | Admins unfamiliar with IR tools | Slower response | Train admins on IR tools |
| Monitoring | Logs not synced to one time zone | Delayed analysis | Normalize all logs to UTC |
