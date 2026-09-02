## Activity: New User Creation (Post-Compromise Persistence)

- **Objective:** Simulate an attacker creating a new local account after
  obtaining valid SSH access, to demonstrate persistence-technique detection.
- **Attacker/actor host:** Kali Linux (192.168.220.143), SSH'd in as `ninjax`
- **Target host:** linux-target / "ubuntu" (192.168.220.145)
- **Date/Time:** 2026-09-02, 06:35:48 UTC (`useradd[3112]`)
- **Activity performed:**
  From an SSH session on linux-target (logged in as `ninjax`, simulating
  a compromised account), ran:
  `sudo useradd -m -s /bin/bash backdoor1`
  followed by `sudo passwd backdoor1` to set a password, creating a new
  local user account. This mirrors a common post-compromise persistence
  technique — an attacker who gains valid access creates their own account
  to retain access even if the original compromised credentials are
  rotated.
  (Note: an earlier practice run created a similarly-named account
  "backdoor" at 06:22:12 UTC — this write-up documents the "backdoor1"
  creation as the recorded activity for evidence purposes.)
- **Evidence collected:**
  - `screenshots/linux-02-useradd-command.png` — command run in SSH session
  - `screenshots/linux-02-authlog-evidence.png` — auth.log `useradd[3112]:
    new user: name=backdoor1, UID=1002, GID=1002, home=/home/backdoor1,
    shell=/bin/bash` entry
  - `screenshots/linux-02-wazuh-alert.png` — Wazuh alert (Rule 5902 and
    related PAM/sudo session rules)
- **Expected security impact:** Unauthorized creation of local accounts is
  a classic persistence and privilege-escalation technique. If undetected,
  it lets an attacker regain access even after the initially compromised
  credentials are reset, and — if the new account is added to an admin
  group — grants full system control. This makes account-creation
  monitoring and alerting a critical detective control (see Defensive
  Recommendations: Account and Privilege Monitoring).