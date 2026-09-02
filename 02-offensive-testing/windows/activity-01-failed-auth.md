## Activity: Failed Authentication Attempts

- **Objective:** Demonstrate repeated failed login attempts against the
  Windows target and confirm they are detected and logged by Wazuh.
- **Attacker host:** Mac host terminal (192.168.220.1) — see methodology
  note below
- **Target host:** win-target / "ninjax" (192.168.220.148/149), local
  console logon
- **Date/Time:** 2026-09-02, ~10:34–10:53 AM local (9 failed attempts,
  Event Viewer) / 14:51–15:10 local per Wazuh dashboard timestamps
- **Activity performed:**
  Attempted to remotely brute-force the Windows target's RDP service from
  the attacker platform using Hydra and, when that tool lacked RDP support
  and ncrack failed with a library error, pivoted to repeated failed
  authentication at the Windows lock screen using intentionally incorrect
  passwords (5-9 attempts). Windows Hello PIN sign-in was first disabled
  in favour of password authentication, since PIN failures do not reliably
  generate standard Security audit events.
- **Methodology note:** Running all four lab VMs (Wazuh, Kali, Windows,
  Linux target) simultaneously exceeded available host resources (8GB
  RAM MacBook). This activity was therefore performed directly from the
  Mac host terminal / Windows console rather than from the Kali VM. This
  is documented transparently here rather than misattributed, and does
  not affect the validity of the detection evidence.
- **Evidence collected:**
  - `screenshots/windows-01-eventviewer.png` — Event Viewer filtered to
    Event ID 4625, showing 9 failed logon events
  - `screenshots/windows-01-wazuh-alert.png` — Wazuh alert list showing
    Rule 60122 "Logon Failure - Unknown user or bad password" repeated
    across multiple timestamps
- **Expected security impact:** Repeated failed authentication attempts,
  whether remote (RDP/network) or local (console), indicate a possible
  brute-force or credential-guessing attempt. Without monitoring and
  account lockout policies, an attacker with time and a large wordlist
  could eventually guess a weak password. Detection and alerting on
  repeated 4625 events allows early response before a compromise occurs.