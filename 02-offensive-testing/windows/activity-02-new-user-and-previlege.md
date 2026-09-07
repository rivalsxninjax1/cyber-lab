## Activity: New User Creation (Windows)

- **Objective:** Simulate an attacker creating a new local account on the
  Windows target for persistence, and confirm Wazuh detects it.
- **Actor host:** Windows target itself (local Administrator Command
  Prompt) — see methodology note in activity-01 regarding Mac/local
  execution due to hardware constraints
- **Target host:** win-target / "Windows" (192.168.220.149)
- **Date/Time:** 2026-09-07, 07:44:05 (account creation)
- **Activity performed:**
  From an elevated Command Prompt, ran:
  `net user backdoor Passw0rd123! /add`
  creating a new local account named "backdoor" — mirroring the same
  persistence technique demonstrated on the Linux target, for a
  consistent attack narrative across both hosts.
- **Evidence collected:**
  - `screenshots/windows-02-netuser-command.png` — net user command run
  - `screenshots/windows-02-eventviewer.png` — Event Viewer showing
    Event ID 4720 ("A user account was created")
  - `screenshots/windows-02-wazuh-alert.png` — Wazuh alert, Rule 60109
    "User account enabled or created"
- **Expected security impact:** Unauthorized local account creation is a
  standard persistence technique, allowing continued access independent
  of any originally compromised credentials. Left undetected, it
  represents a durable foothold for an attacker on the host.

---

## Activity: Privilege-Related Activity — Admin Group Addition (Windows)

- **Objective:** Simulate escalating the newly created account to full
  administrative privileges, continuing the same attack chain used on
  the Linux target.
- **Actor host:** Windows target itself (local Administrator Command
  Prompt)
- **Target host:** win-target / "Windows" (192.168.220.149)
- **Date/Time:** 2026-09-07, 07:46:15
- **Activity performed:**
  `net localgroup administrators backdoor /add`
  adding the "backdoor" account to the local Administrators group,
  granting it full administrative control of the host.
- **Evidence collected:**
  - `screenshots/windows-02-localgroup-command.png` — command run
  - `screenshots/windows-02-eventviewer-4732.png` — Event Viewer showing
    Event ID 4732 ("A member was added to a security-enabled local group")
  - `screenshots/windows-02-wazuh-alert-admingroup.png` — Wazuh alert,
    Rule 60132 "Administrators Group Changed"
- **Expected security impact:** Adding an account to the local
  Administrators group is one of the highest-impact privilege changes
  possible on a Windows host — it grants full control, including the
  ability to disable security tooling, access all data, and create
  further persistence mechanisms. This mirrors the exact attack pattern
  demonstrated on the Linux target (backdoor1 added to sudo), giving a
  consistent, cross-platform attack chain for the report.