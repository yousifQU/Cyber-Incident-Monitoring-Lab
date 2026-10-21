# How the CSF Functions Connect

The CIRP is designed so each Function feeds the others.

1. **Severity drives escalation.** Investigation identifies incident type and affected assets, which sets classification. *Example:* an NTLM relay attack against a Domain Controller escalates as High severity.
2. **Technical analysis drives containment and recovery.** *Example:* if logs show LDAP abuse, containment can include blocking the attacker IP and disabling vulnerable ADCS web enrollment.
3. **Data classification drives communication.** General system logs can stay internal; exposed AD credentials raise the classification and restrict disclosure.
4. **Asset criticality drives prioritization.** The Identify function defines crown-jewel systems. The same attack on a Domain Controller gets faster escalation and deeper evidence collection than on an ordinary workstation.
