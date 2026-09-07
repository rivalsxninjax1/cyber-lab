# Lab Architecture

## Network Diagram

```
[Kali Linux - Attacker]
        |
        |  (attacks over host-only network)
        v
[Windows Target] <----logs----> [Wazuh Server] <----logs---- [Linux Target]
                                       |
                                (Wazuh Dashboard - analyst view)
```

## VM Inventory

| VM             | OS               | IP (host-only) | RAM  | Role                     |
|-----------------|------------------|-----------------|------|---------------------------|
| kali-attacker   | Kali Linux       | 192.168.X.X     | -    | Offensive testing         |
| wazuh-server    | Ubuntu Server    | 192.168.X.X     | -    | SIEM manager + dashboard  |
| win-target      | Windows 10/11    | 192.168.X.X     | -    | Monitored host, agent installed |
| linux-target    | Ubuntu           | 192.168.X.X     | -    | Monitored host, agent installed |

## Log Sources Configured

- Windows: Security Event Log (logon events, process creation, PowerShell
  script block logging), Sysmon (optional, recommended)
- Linux: `/var/log/auth.log` (SSH), `/var/log/syslog`, `/var/log/audit/audit.log`
  (auditd, optional)

## Agent Enrollment Confirmation

- [ ] Windows agent shows "Active" in Wazuh dashboard
- [ ] Linux agent shows "Active" in Wazuh dashboard
- Screenshot: `../screenshots/agents-active.png`
