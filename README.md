# Cyber Attack, Detection and Security Assessment — Final Assignment

Security monitoring lab demonstrating both offensive and defensive skills:
Wazuh-based detection, controlled security testing, threat intel / IOC analysis,
risk assessment, and defensive recommendations.

## Lab Environment

| Role            | OS                  | Purpose                                 |
|-----------------|---------------------|------------------------------------------|
| Attacker        | Kali Linux          | Performs controlled security activities |
| Wazuh Server    | Ubuntu Server 22.04 | SIEM — collects & analyzes logs         |
| Windows Target  | Windows 10/11       | Monitored host, Wazuh agent installed   |
| Linux Target    | Ubuntu Desktop/Server | Monitored host, Wazuh agent installed |

All VMs run on an isolated **host-only** VMware network. No activity is
performed against systems outside this lab.

## Repo Structure

```
01-lab-architecture/        Architecture diagram + setup notes
02-offensive-testing/
  windows/                  Activity logs (objective, target, time, evidence, impact)
  linux/                    Activity logs (objective, target, time, evidence, impact)
03-wazuh-detection/         Investigation writeups per activity
04-ioc-analysis/            IOC analysis table + notes
05-risk-assessment/         Top 5 findings risk table
06-defensive-recommendations/  Recommended controls
07-report/                  Final compiled report (docx/pdf)
screenshots/                Supporting evidence images
```


