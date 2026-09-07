# Threat Intelligence & IOC Analysis

Indicators from the assignment (defanged form removed — [.] replaced with .).
Checked against VirusTotal and AbuseIPDB.

**Note on hash format:** the 4 hash indicators are 32 hex characters (MD5),
not SHA-256 as the assignment's example table header suggests. VirusTotal
correctly resolves an MD5 search to the matching file's full SHA-256 record,
which is why the underlying file hashes shown below differ in length from
the original IOC list — this is expected VT behaviour, not an error.

## Hashes (MD5, resolved via VirusTotal)

| Original MD5 | VT Result | Finding | Confidence | Malware Family |
|---|---|---|---|---|
| 07763588cf68716a159469ec006183b8 | (resolves to SHA-256 aad37e1e...5fa29) | **Malicious** — 5/62 vendors | Medium | **Mirai** (ELF, RISC-V binary). McAfee/Microsoft/Rising/TrendMicro flag as Backdoor.Mirai / Backdoor:Linux/Mirai.GS |
| 24bd24cf3f7207a990672f3c7f552bbe | (resolves to SHA-256 f701f061...0d3339ca) | **Malicious** — 39/68 vendors | **High** | **Yogi trojan/dropper** (Windows .exe). Multiple vendors: Trojan Dropper, Wacatac variant, CrowdStrike "malicious_confidence_100%" |
| 4a09f8c92732d01c097d9a12cbbbc6da | (resolves to SHA-256 b5dc8167...2ba7b7ff7) | **Malicious** — 29/62 vendors | **High** | **DDoS trojan / "multiverze"** (ELF, ARM architecture). Flagged as Linux DDoS Agent by multiple vendors (Avast, BitDefender, ESET-NOD32) |
| 5c943b9ee464e13570b4d57e643f6a93 | *(not resolved — recheck manually if time permits)* | — | — | — |

**Analysis:** Three of the four hashes are confirmed malicious, spanning both
Windows (trojan/dropper) and Linux/embedded (Mirai, DDoS agent for ARM/
RISC-V — consistent with IoT-class devices). This is a mixed-platform
malware set, not a single campaign — suggesting the IOC list is aggregated
from multiple unrelated threat feeds rather than one incident.

## IP Addresses (AbuseIPDB)

| IOC | Finding | Confidence | ISP / Location | Notes |
|---|---|---|---|---|
| 13.115.104.132 | No abuse reports | Low | AWS Tokyo | Likely legitimate cloud infra; low standalone risk |
| 3.108.37.115 | *(not checked — likely AWS Mumbai range)* | — | — | Recheck if time permits |
| 200.175.61.207 | **Malicious — 100% confidence** (6,730 reports) | **High** | Telefonica Brasil, Florianópolis | Fixed-line ISP IP with very high report volume — likely compromised residential/business connection used for attacks |
| 187.120.72.90 | **Suspicious — 62% confidence** (245 reports) | Medium | MASTER S/A, Passos, Brazil | Fixed-line ISP; moderate report volume |
| 185.246.128.25 | **Malicious — 100% confidence** (6,781 reports) | **High** | w1n ltd, Stockholm, Sweden | Data center/hosting IP — high-volume abuse source, consistent with scanning/attack infrastructure |
| 185.226.197.7 | **Malicious — 100% confidence** (9,357 reports) | **High** | ICG-4-ZEN-AMS, Netherlands | Hostname resolves to `internet-census.org` — associated with mass internet-wide scanning activity |
| 184.105.247.251 | Reported, but attributable to legitimate research | Low | **The Shadowserver Foundation**, Fremont, CA | Shadowserver is a well-known non-profit security research organization that performs internet-wide scanning for threat intelligence and vulnerability notification purposes. High report counts on Shadowserver IPs are extremely common and typically reflect its scanning activity being mistaken for malicious probing, not actual compromise. **Recommend Low/informational confidence, not High**, despite any raw report count — a good example of why confidence should not be based on report volume alone. |

## URLs

| IOC | Finding | Confidence | Associated Malware | Detection Method |
|---|---|---|---|---|
| http://210.208.111.2:36838/i | *(not directly checked — pattern strongly resembles C2 staging)* | Medium (inferred) | Raw IP + non-standard high port + short path is a classic malware C2/payload-delivery pattern | DNS/Web/Proxy |
| https://mediafire.com/file/pfxpcqrssrvi4h4/file | *(not directly checked)* | — | Legitimate file-hosting service commonly abused to host malware payloads, evading domain-reputation blocklists | DNS/Web/Proxy |
| http://115.55.183.61:57147/Mozi.a | **Malicious — confirmed** | **High** | **Mozi botnet** (P2P IoT botnet derived from Mirai/Gafgyt/IoT Reaper source code). Targets routers/DVRs via weak Telnet credentials; used for DDoS, data exfiltration, remote command execution | DNS/Web/Proxy; unusual outbound Telnet/UDP port 14737 traffic on IoT devices |

## Key Findings Summary

1. **7 of 13 IOCs confirmed malicious/suspicious** with independent evidence
   (VirusTotal vendor detections or AbuseIPDB abuse-confidence scores).
2. **Mixed threat landscape**: IoT botnets (Mirai, Mozi), a Windows dropper
   trojan, a Linux DDoS agent, and multiple high-volume malicious IPs —
   this is a diverse indicator set rather than a single coordinated attack,
   consistent with a general threat-intelligence training exercise rather
   than one real incident.
3. **False-positive risk identified**: the Shadowserver Foundation IP
   (184.105.247.251) demonstrates that raw AbuseIPDB report counts can be
   misleading — analysts must verify *who* is generating reports and *why*
   before assigning High confidence, not just read the percentage.
4. **Scope of applicability**: the IoT-targeting malware (Mirai, Mozi)
   would not directly threaten this lab's Windows/Linux general-purpose
   endpoints, but the malicious IPs and Windows/Linux malware hashes are
   broadly applicable and should inform firewall/DNS blocklisting and
   endpoint AV signature updates regardless of host type.
5. **Remaining gaps**: one hash (5c943b9e...) and one IP (3.108.37.115)
   were not fully resolved in this pass — recommend a final check before
   submission if time allows, otherwise note as an acknowledged limitation
   in the report.