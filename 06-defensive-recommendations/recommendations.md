# Defensive Recommendations

Map each recommendation back to a specific finding from the risk assessment
where possible.

## Authentication and MFA
- Enforce strong password policy on both targets (addresses Finding #1)[cite: 1].
- Move to SSH key-based authentication where feasible (addresses Finding #1)[cite: 1].
- Enable MFA on the Wazuh dashboard and other admin interfaces (addresses Finding #5)[cite: 1].
- Supplement Windows Hello PIN sign-in with additional auditing (addresses Finding #5)[cite: 1].

## Least Privilege
- Restrict sudo/Administrators membership to accounts that require it (addresses Finding #2)[cite: 1].
- Require documented approval before privileged group changes (addresses Finding #2)[cite: 1].
- Regularly audit local accounts and group memberships on both hosts (addresses Finding #2)[cite: 1].

## Windows Hardening
- Disable unused services and remote access protocols[cite: 1].
- Apply CIS benchmark hardening and leverage Wazuh's SCA module for continuous compliance checking[cite: 1].
- Enable account lockout thresholds[cite: 1].

## Linux Hardening
- Disable unused services and remote access protocols[cite: 1].
- Apply CIS benchmark hardening and leverage Wazuh's SCA module for continuous compliance checking[cite: 1].
- Enable fail2ban (addresses Finding #1)[cite: 1].

## Firewall and Network Restrictions
- Restrict SSH to trusted source IP ranges (addresses Finding #1)[cite: 1].
- Deploy egress filtering against confirmed malicious IPs identified in IOC analysis (addresses Finding #4)[cite: 1].
- Segment the network so targets cannot reach the SIEM management interface unnecessarily[cite: 1].

## Patch Management
- Establish a regular OS and software patching cadence[cite: 1].
- Use Wazuh's vulnerability detection module for continuous package auditing[cite: 1].

## Endpoint Protection
- Deploy AV/EDR capable of detecting the malware families confirmed in IOC analysis (Mirai, Windows dropper, Linux DDoS agent)[cite: 1].
- Configure real-time (whodata) FIM rather than relying solely on scheduled scans[cite: 1].

## Security Logging and Monitoring
- Configure active-response real-time alerting for high-severity rules (addresses Finding #3)[cite: 1].
- Enable PowerShell Script Block Logging across Windows hosts[cite: 1].
- Activate Wazuh's threat-intelligence/VirusTotal integration for automatic IOC correlation[cite: 1].

## Account and Privilege Monitoring
- Alert immediately on new-user-creation and privileged-group-change rules demonstrated in this project (5902/60109, 5402/60132) (addresses Findings #2 & #3)[cite: 1].
- Periodically reconcile active accounts against an approved baseline[cite: 1].
- Rotate and monitor the Wazuh platform's own administrative credentials (addresses Finding #5)[cite: 1].