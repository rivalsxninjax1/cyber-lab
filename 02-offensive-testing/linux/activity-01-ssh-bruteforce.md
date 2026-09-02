## Activity: SSH Failed Authentication Attempts (Brute Force)

- **Objective:** Demonstrate a brute-force SSH login attempt against the
  Linux target and confirm it is detected and logged by Wazuh.
- **Attacker host:** Kali Linux (192.168.220.143)
- **Target host:** linux-target / "ubuntu" (192.168.220.145), port 22
- **Date/Time:** 2026-09-02, 05:26:36–05:26:44 UTC (from auth.log
  timestamps; Wazuh alert recorded at 11:11:43.973 local time — timezone
  offset accounted for)
- **Activity performed:**
  Ran a Hydra SSH brute-force attack from Kali against linux-target:
  `hydra -l ninjax -P /usr/share/wordlists/rockyou.txt.gz -t 4 ssh://192.168.220.145`
  The attack generated repeated failed password attempts against the
  `ninjax` account. SSH's built-in max-authentication-attempts protection
  triggered multiple times, dropping the connection after 6 failed attempts
  each round ("Too many authentication failures", "PAM service(sshd)
  ignoring max retries; 6 > 3"), before the attack was stopped manually.
- **Evidence collected:**
  - `screenshots/linux-01-hydra-attack.png` — Hydra running in Kali
  - `screenshots/linux-01-authlog-evidence.png` — `/var/log/auth.log`
    showing repeated "Failed password for ninjax from 192.168.220.143"
    entries and connection drops
  - `screenshots/linux-01-wazuh-alert.png` — Wazuh alert detail (Rule 5760)
  - `linux-01-authlog-raw.txt` (this folder) — full raw log excerpt
- **Expected security impact:** Weak or default SSH credentials combined
  with unrestricted SSH exposure allow attackers to brute-force valid
  accounts. Successful compromise would grant a foothold on the host,
  potentially escalating to full system access. Real-world impact is
  reduced by strong password policy, key-based auth, rate limiting/
  fail2ban, and restricting SSH source IPs (see Defensive Recommendations).