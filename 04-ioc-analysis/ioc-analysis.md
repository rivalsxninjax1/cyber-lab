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
| 07763588cf68716a159469ec006183b8 | Malicious (5/62 vendors) | Malicious | Medium | Mirai (ELF/RISC-V backdoor) |
| 24bd24cf3f7207a990672f3c7f552bbe | Malicious (39/68 vendors) | Malicious | High | Yogi trojan/dropper (Windows) |
| 4a09f8c92732d01c097d9a12cbbbc6da | Malicious (29/62 vendors) | Malicious | High | DDoS trojan/multiverze (ELF/ARM) |
| 5c943b9ee464e13570b4d57e643f6a93 | Malicious (24/60 vendors) | Malicious | High | Zbot trojan/dropper (VBS) |


**Analysis:** Three of the four hashes are confirmed malicious, spanning both
Windows (trojan/dropper) and Linux/embedded (Mirai, DDoS agent for ARM/
RISC-V — consistent with IoT-class devices). This is a mixed-platform
malware set, not a single campaign — suggesting the IOC list is aggregated
from multiple unrelated threat feeds rather than one incident.

## IP Addresses (AbuseIPDB)

| IOC | Finding | Confidence | ISP / Location | Notes |
|---|---|---|---|---|
| 13.115.104.132 | No abuse reports | Low | AWS Tokyo | AWS Tokyo likely legitimate cloud infra |
| 3.108.37.115 | No abuse reports | Low | Data Center/Web Hosting/Transit | — |
| 200.175.61.207 | Malicious, 100% (6,730 reports) | High | Telefonica Brazil | Fixed-line ISP |
| 187.120.72.90 | Suspicious, 62% (245 reports) | Medium | Brazil fixed-line ISP | — |
| 185.246.128.25 | Malicious, 100% (6,781 reports) | High | Data center/hosting, Sweden | — |
| 185.226.197.7 | Malicious, 100% (9,357 reports) | High | Netherlands | Hostname tied to internet-census.org |
| 184.105.247.251 | Reported, but legitimate research | Low | The Shadowserver Foundation | False-positive risk |

## URLs

| IOC | Finding | Confidence | Associated Malware | Detection Method |
|---|---|---|---|---|
| http://210.208.111.2:36838/i | Activity related to MIRAI, MOZI | Medium | Malicious (alphaMountain.ai) | — |
| https://mediafire.com/file/pfxpcqrssrvi4h4/file | File Sharing/Storage, Media Sharing (alphaMountain.ai) | Low | Legitimate host commonly abused for payload delivery | — |
| http://115.55.183.61:57147/Mozi.a | Malicious confirmed | High | Mozi (P2P IoT botnet, Mirai/Gafgyt/IoT Reaper lineage) | — |

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