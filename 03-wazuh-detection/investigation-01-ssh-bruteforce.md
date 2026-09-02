## Investigation: SSH Failed Authentication Attempts (Brute Force)

- **Affected host:** linux-target (Wazuh agent 001, hostname `ubuntu`)
- **Timestamp (from Wazuh):** Sep 2, 2026 @ 11:11:43.973 (dashboard local
  time); source events span 05:26:36–05:26:44 UTC in auth.log
- **Username:** ninjax
- **Source IP:** 192.168.220.143 (Kali attacker VM)
- **Relevant log source:** Linux `/var/log/auth.log` (sshd / sshd-session /
  PAM), collected via Wazuh agent logcollector
- **Event description:** Repeated failed SSH password authentication
  attempts against the `ninjax` account from a single source IP in rapid
  succession, consistent with an automated brute-force tool. SSH's own
  max-authentication-attempts protection triggered and dropped connections
  after 6 failures per session, but the attacking tool immediately opened
  new connections and continued.
- **Wazuh alert generated?** Yes — Rule ID **5760**, level 5,
  description: "sshd: authentication failed."
- **Screenshots:** `screenshots/linux-01-wazuh-alert.png`

### Narrative
- **What happened?** A brute-force SSH login attack was launched from the
  Kali attacker VM (192.168.220.143) against linux-target (192.168.220.145)
  using Hydra with a common-password wordlist, targeting the `ninjax`
  account on port 22.
- **How was it detected?** The Wazuh agent installed on linux-target
  monitors and forwards sshd/PAM authentication log entries to the Wazuh
  manager in near real time. The manager's default ruleset matched the
  repeated failed-login pattern from sshd and triggered rule 5760.
- **What evidence proves it happened?** Two independent sources confirm
  the event: (1) the raw `/var/log/auth.log` entries on the target itself,
  showing dozens of "Failed password for ninjax from 192.168.220.143"
  lines and repeated "Too many authentication failures" disconnects between
  05:26:36–05:26:44 UTC, and (2) the corresponding Wazuh alert (rule 5760,
  level 5) generated from that same log stream, with matching source IP
  and username.

### Notes for follow-up
- A successful login for a similarly-named user was observed later in the
  log shortly after the attack window — worth confirming this was the
  legitimate account owner and not evidence the brute force (or a
  subsequent attempt) succeeded. Document the outcome here once confirmed.