## Investigation: Privilege Escalation (Adding Persistence Account to Sudo Group)

- **Affected host:** linux-target (Wazuh agent 001, hostname `ubuntu`)
- **Timestamp (from Wazuh dashboard):** Sep 2, 2026 @ 13:57:45 local time
  (07:57:45 UTC in auth.log — timezone offset of the dashboard vs. UTC log
  confirmed consistent with prior activities)
- **Username:** ninjax (actor performing the escalation, via sudo)
- **Source IP:** 192.168.220.143 (Kali, via SSH session)
- **Relevant log source:** Linux `/var/log/auth.log` (sudo, PAM), collected
  via Wazuh agent logcollector
- **Event description:** A sudo command was executed as root
  (`usermod -aG sudo backdoor1`), adding the previously created `backdoor1`
  account to the `sudo` group. This is bracketed by a PAM session
  open/close pair, matching the pattern seen in activities #1 and #2.
- **Wazuh alert generated?** Yes:
  - **Rule 5402** — "Successful sudo to ROOT executed."
  - **Rule 5501** — "PAM: Login session opened."
  - **Rule 5502** — "PAM: Login session closed."
- **Screenshots:** `screenshots/linux-03-wazuh-alert.png`

### Narrative
- **What happened?** The `ninjax` account (used throughout this attack
  chain) escalated the `backdoor1` persistence account's privileges by
  adding it to the `sudo` group, granting it full administrative rights
  on linux-target.
- **How was it detected?** The Wazuh agent forwards all sudo invocations
  and PAM session events from auth.log to the manager. The default
  ruleset flags any successful sudo-to-root execution (rule 5402),
  regardless of the specific command run, alongside the PAM session
  lifecycle events that bracket it.
- **What evidence proves it happened?** The auth.log line
  `sudo: ninjax: TTY=/dev/pts/0; PWD=/home/ninjax; USER=root;
  COMMAND=/usr/sbin/usermod -aG sudo backdoor1` directly records the
  exact command executed, cross-referenced against the matching Wazuh
  alert (rule 5402) generated at the corresponding timestamp.

### Chain summary (activities #1–#3)
This activity completes a realistic three-stage attack chain fully
captured and detected end-to-end by Wazuh:
1. **Initial access attempt** — SSH brute force against `ninjax`
   (activity #1, rule 5760)
2. **Persistence** — creation of `backdoor1` account
   (activity #2, rules 5901/5902)
3. **Privilege escalation** — `backdoor1` added to sudo group
   (activity #3, rule 5402)

### Notes for follow-up
- Test accounts (`backdoor`, `backdoor1`) and the sudo group membership
  granted here should be removed after evidence capture:
  `sudo gpasswd -d backdoor1 sudo`, then
  `sudo userdel -r backdoor && sudo userdel -r backdoor1`.