## Activity: Privilege Escalation (Adding Persistence Account to Sudo Group)

- **Objective:** Simulate an attacker escalating a previously created
  persistence account to full administrative privileges, and confirm Wazuh
  detects the privileged action.
- **Actor host:** Kali Linux (192.168.220.143), SSH'd in as `ninjax`
- **Target host:** linux-target / "ubuntu" (192.168.220.145)
- **Date/Time:** 2026-09-02, 07:57:45 UTC
- **Activity performed:**
  Continuing the attack narrative from activity #2 (creation of the
  `backdoor1` persistence account), the account was escalated to full
  administrative (sudo) privileges:
  `sudo usermod -aG sudo backdoor1`
  followed by verification with `groups backdoor1`. This represents the
  final stage of a realistic post-compromise attack chain: gain access
  (activity #1) → establish persistence (activity #2) → escalate
  privileges (this activity), after which the attacker would have full
  control of the host via the `backdoor1` account independent of the
  originally compromised credentials.
- **Evidence collected:**
  - `screenshots/linux-03-usermod-command.png` — usermod command and
    `groups backdoor1` confirmation
  - `screenshots/linux-03-authlog-evidence.png` — auth.log line:
    `sudo: ninjax: ... COMMAND=/usr/sbin/usermod -aG sudo backdoor1`
  - `screenshots/linux-03-wazuh-alert.png` — Wazuh alert (Rule 5402 +
    PAM session rules 5501/5502)
- **Expected security impact:** Uncontrolled or unmonitored changes to
  privileged group membership (sudo/wheel/admin) is one of the highest-
  impact security events on a Linux host — it converts a standard
  compromised account into full root-equivalent access. Combined with
  activities #1 and #2, this demonstrates a complete, realistic attack
  chain from initial access to full privilege escalation, underscoring
  the need for privileged-group change alerting and least-privilege
  enforcement (see Defensive Recommendations).