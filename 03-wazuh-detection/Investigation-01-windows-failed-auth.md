## Investigation: Failed Authentication Attempts (Windows)

- **Affected host:** win-target (Wazuh agent, hostname "Windows" /
  "ninjax")
- **Timestamp (from Wazuh dashboard):** Sep 2, 2026, multiple events
  14:51:34–15:10:10 local time
- **Username:** ninjax (account targeted at the lock screen)
- **Source IP:** N/A / local console logon (not a network-sourced logon
  event — see activity log for methodology note)
- **Relevant log source:** Windows Security Event Log, Event ID 4625
  ("An account failed to log on"), collected via Wazuh agent
- **Event description:** A cluster of repeated failed logon attempts
  against the local Windows account, occurring in rapid succession
  (multiple events within seconds of each other across two separate
  bursts), consistent with a deliberate series of incorrect password
  attempts.
- **Wazuh alert generated?** Yes — Rule ID **60122**, level 5,
  description: "Logon Failure - Unknown user or bad password." Triggered
  repeatedly (at least 8 occurrences observed in the dashboard).
- **Screenshots:** `screenshots/windows-01-wazuh-alert.png`

### Narrative
- **What happened?** Multiple failed login attempts were made against the
  Windows target's local account, using intentionally incorrect
  passwords, to simulate a credential-guessing/brute-force scenario.
- **How was it detected?** The Wazuh agent installed on win-target
  forwards Windows Security Event Log entries to the manager. The default
  ruleset matches Event ID 4625 (failed logon) and generates rule 60122
  for each occurrence, allowing the manager to surface the pattern of
  repeated failures.
- **What evidence proves it happened?** Windows Event Viewer independently
  recorded 9 Event ID 4625 entries between 10:34 AM and 10:53 AM, and the
  Wazuh dashboard shows a matching cluster of rule 60122 alerts — two
  independent sources confirming the same event sequence.

### Notes for follow-up
- Initial attempts to trigger this activity via Windows Hello PIN entry
  did not generate 4625 events; switching to password-based sign-in was
  required. Worth noting in the report as a small but genuine finding:
  PIN-based sign-in failures are not captured by standard Security
  auditing in the same way password failures are, which is itself a
  minor visibility gap worth flagging under Defensive Recommendations.