# Risk Assessment — Top 5 Findings

| # | Finding | Asset | Likelihood | Impact | Risk | Recommendation |
|---|---|---|---|---|---|---|
| 1 | SSH brute-force succeeded against weak credentials, no rate-limiting | Linux target | High | High | Critical | Enforce strong password policy; move to SSH key-based authentication; enable fail2ban and restrict source IPs. |
| 2 | Uncontrolled privilege escalation via sudo/Administrators group | Both targets | Medium | Critical | Critical | Restrict sudo/Administrators membership; require documented approval before privileged group changes; audit local accounts/groups regularly. |
| 3 | Unauthorized account creation undetected until manual review | Both targets | Medium | High | High | Configure active-response real-time alerting for high-severity rules (e.g., rules 5902/60109). |
| 4 | No egress filtering against confirmed malicious IPs | Network-wide | Medium | High | High | Deploy firewall egress filtering against confirmed malicious IPs identified in IOC analysis. |
| 5 | Default Wazuh credentials; PIN sign-in visibility gap | Wazuh server, Windows | Low | Medium | Medium | Enable MFA on Wazuh dashboard, rotate default administrative credentials, and supplement Windows Hello PIN sign-in with additional auditing. |

**Example (from assignment brief):**
Finding: Weak SSH auth → Asset: Linux server → Likelihood: High → Impact: High
→ Risk: Critical → Recommendation: Restrict SSH and strengthen authentication

## Likelihood / Impact scale used
- Low / Medium / High for both axes
- Risk = combination of Likelihood x Impact (Low, Medium, High, Critical)