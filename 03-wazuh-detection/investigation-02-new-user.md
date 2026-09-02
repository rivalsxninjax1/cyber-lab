## Investigation: New User Creation (Post-Compromise Persistence)

- **Affected host:** linux-target (Wazuh agent 001, hostname `ubuntu`)
- **Timestamp (from Wazuh dashboard):** Sep 2, 2026 @ 12:20:48–12:22:00
  local time (corresponds to the 06:20–06:22 UTC window in auth.log for the
  earlier practice run; see note below on which timestamp maps to which
  account)
- **Username:** ninjax (actor performing the action, via sudo), new account
  created: backdoor1
- **Source IP:** 192.168.220.143 (Kali, via SSH session as ninjax)
- **Relevant log source:** Linux `/var/log/auth.log` (useradd, sudo, PAM),
  collected via Wazuh agent logcollector
- **Event description:** A sequence of related events fired around account
  creation: a sudo command executed as root, a new group added, a new user
  added, a PAM login session opened/closed, and (in one sequence) a
  password change — consistent with the full lifecycle of creating and
  configuring a new local account.
- **Wazuh alert generated?** Yes — multiple correlated alerts:
  - **Rule 5902** — "New user added to the system."
  - **Rule 5901** — "New group added to the system."
  - **Rule 5402** — "Successful sudo to ROOT executed."
  - **Rule 5501/5502** — "PAM: Login session opened / closed."
  - **Rule 5555** — "PAM: User changed password."
- **Screenshots:** `screenshots/linux-02-wazuh-alert.png`

### Narrative
- **What happened?** After authenticating as `ninjax` over SSH, a new local
  user account (`backdoor1`) was created via `useradd` and configured with
  a password via `passwd`, run under `sudo`. This simulates an attacker
  with valid access establishing a persistent foothold account.
- **How was it detected?** The Wazuh agent forwards `useradd`, `sudo`, and
  PAM log entries from auth.log to the manager. The default ruleset
  recognises the "new user:" log line from useradd (rule 5902), the
  associated group creation (rule 5901), the sudo escalation to root
  (rule 5402), and the PAM session lifecycle around it — giving a full,
  correlated picture of the action from multiple independent log lines
  rather than a single alert.
- **What evidence proves it happened?** The auth.log entry
  `useradd[3112]: new user: name=backdoor1, UID=1002, GID=1002,
  home=/home/backdoor1, shell=/bin/bash, from=/dev/pts/1` directly confirms
  the account creation, cross-referenced with the corresponding Wazuh
  alerts (rules 5902, 5901, 5402) generated from the same log stream.

### Notes for follow-up
- Two accounts were created during testing ("backdoor" and "backdoor1");
  this investigation focuses on "backdoor1" per the confirmed useradd log
  line and timestamp. Delete/disable any leftover test accounts
  (`sudo userdel -r backdoor`, `sudo userdel -r backdoor1`) once evidence
  is captured, to avoid leaving unnecessary accounts on the lab host.