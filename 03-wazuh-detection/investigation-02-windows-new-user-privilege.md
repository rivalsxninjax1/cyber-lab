## Investigation: New User Creation (Windows)

- **Affected host:** win-target (Wazuh agent, hostname "Windows")
- **Timestamp (from Wazuh dashboard):** Sep 7, 2026 @ 07:44:05.974–.987
- **Username:** backdoor (account created)
- **Source IP:** N/A / local console execution on win-target
- **Relevant log source:** Windows Security Event Log, Event ID 4720
  ("A user account was created"), collected via Wazuh agent
- **Event description:** A new local user account named "backdoor" was
  created via the `net user` command, immediately followed by related
  "User account changed" (60110-range) events as the account was
  configured.
- **Wazuh alert generated?** Yes — Rule ID **60109**, description:
  "User account enabled or created."
- **Screenshots:** `screenshots/windows-02-wazuh-alert.png`

### Narrative
- **What happened?** A new local account, "backdoor," was created on
  win-target using the built-in `net user` command-line tool.
- **How was it detected?** The Wazuh agent forwards Windows Security
  Event Log entries to the manager. Event ID 4720 is matched by the
  default Windows ruleset and mapped to rule 60109.
- **What evidence proves it happened?** Windows Event Viewer independently
  logged Event ID 4720 at the time of creation, and the Wazuh dashboard
  shows the corresponding rule 60109 alert at a matching timestamp
  (07:44:05).

---

## Investigation: Privilege-Related Activity — Admin Group Addition (Windows)

- **Affected host:** win-target (Wazuh agent, hostname "Windows")
- **Timestamp (from Wazuh dashboard):** Sep 7, 2026 @ 07:46:15.653
- **Username:** backdoor (account added to Administrators group)
- **Source IP:** N/A / local console execution on win-target
- **Relevant log source:** Windows Security Event Log, Event ID 4732
  ("A member was added to a security-enabled local group"), collected
  via Wazuh agent
- **Event description:** The "backdoor" account (created ~2 minutes
  earlier) was added to the local Administrators group via the
  `net localgroup` command, granting it full administrative rights.
- **Wazuh alert generated?** Yes — Rule ID **60132**, description:
  "Administrators Group Changed."
- **Screenshots:** `screenshots/windows-02-wazuh-alert-admingroup.png`

### Narrative
- **What happened?** The newly created "backdoor" account was escalated
  to full administrative privileges by adding it to the local
  Administrators group.
- **How was it detected?** The Wazuh agent forwards Event ID 4732 from
  the Windows Security log, matched to rule 60132 by the default
  ruleset — the Windows equivalent of the sudo-group-change detection
  used on the Linux target.
- **What evidence proves it happened?** Windows Event Viewer logged Event
  ID 4732 at 07:46:15, and the Wazuh dashboard shows the matching rule
  60132 alert at the same timestamp.

### Chain summary (Windows activities #1–#3)
Combined with the failed-authentication activity, this gives a complete
cross-platform attack chain mirroring the Linux side:
1. **Credential-guessing attempt** — repeated failed logons
   (activity #1, rule 60122)
2. **Persistence** — creation of "backdoor" account
   (activity #2, rule 60109)
3. **Privilege escalation** — "backdoor" added to Administrators
   (activity #3, rule 60132)

### Notes for follow-up
- Remove the test account after evidence capture:
  `net localgroup administrators backdoor /delete` then
  `net user backdoor /delete`