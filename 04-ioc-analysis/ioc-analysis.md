# Threat Intelligence & IOC Analysis

Indicators from the assignment (defanged form removed — [.] replaced with .).

## Hashes (SHA-256, likely truncated/partial in assignment — verify format)

| IOC | Type | Finding | Confidence | Associated Malware | Detection Method |
|---|---|---|---|---|---|
| 07763588cf68716a159469ec006183b8 | Hash | TBD | TBD | TBD | Endpoint (EDR/AV/Wazuh FIM+YARA) |
| 24bd24cf3f7207a990672f3c7f552bbe | Hash | TBD | TBD | TBD | Endpoint |
| 4a09f8c92732d01c097d9a12cbbbc6da | Hash | TBD | TBD | TBD | Endpoint |
| 5c943b9ee464e13570b4d57e643f6a93 | Hash | TBD | TBD | TBD | Endpoint |

## IP Addresses

| IOC | Type | Finding | Confidence | Detection Method |
|---|---|---|---|---|
| 13.115.104.132 | IP | TBD | TBD | Firewall/Network |
| 3.108.37.115 | IP | TBD | TBD | Firewall/Network |
| 200.175.61.207 | IP | TBD | TBD | Firewall/Network |
| 187.120.72.90 | IP | TBD | TBD | Firewall/Network |
| 185.246.128.25 | IP | TBD | TBD | Firewall/Network |
| 185.226.197.7 | IP | TBD | TBD | Firewall/Network |
| 184.105.247.251 | IP | TBD | TBD | Firewall/Network |

## URLs

| IOC | Type | Finding | Confidence | Detection Method |
|---|---|---|---|---|
| http://210.208.111.2:36838/i | URL | TBD | TBD | DNS/Web/Proxy |
| https://mediafire.com/file/pfxpcqrssrvi4h4/file | URL | TBD | TBD | DNS/Web/Proxy |
| http://115.55.183.61:57147/Mozi.a | URL | TBD | TBD | DNS/Web/Proxy (note: "Mozi.a" filename is a strong hint — Mozi is a known IoT botnet family) |

## Method Notes

For each IOC, check reputable threat-intel sources such as VirusTotal,
AbuseIPDB, and AlienVault OTX. Record:
- IOC type (hash / IP / domain / URL)
- Whether it's flagged malicious, and by how many vendors/engines
- Associated malware family or campaign, if known
- What systems in *this lab* could plausibly be affected by this IOC type
- How you'd detect it operationally (Wazuh FIM/YARA for hashes, firewall/
  Suricata for IPs, DNS/proxy logs for domains & URLs)
