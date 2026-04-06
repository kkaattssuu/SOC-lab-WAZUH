# SOC Home Lab — Wazuh + Sysmon + Kali Linux

A hands-on SOC (Security Operations Center) lab built entirely on local virtual machines. The goal was to simulate real attack scenarios, capture telemetry, and practice alert triage the way a SOC analyst would — not just read about it.

---

## Lab Architecture

```
┌─────────────────────┐         ┌──────────────────────┐
│   Kali Linux VM     │ ──────▶ │   Windows 10 VM      │
│  (Attacker)         │         │  (Target + Sysmon)   │
└─────────────────────┘         └──────────┬───────────┘
                                           │ logs
                                           ▼
                                ┌──────────────────────┐
                                │   Wazuh Manager      │
                                │   (SIEM / Detection) │
                                └──────────────────────┘
```

- **Attacker:** Kali Linux — used to launch brute-force and reconnaissance attacks
- **Target:** Windows 10 with Sysmon deployed for deep endpoint telemetry
- **SIEM:** Wazuh Manager collecting and correlating events, generating alerts

---

## Attack Scenarios Simulated

### 1. SSH / RDP Brute Force
Simulated a credential brute-force attack from the Kali machine against the Windows target. Monitored how Wazuh detected repeated failed authentication attempts and triggered high-severity alerts.

**What I looked for:**
- Rule ID 5710 / 5712 (multiple failed logins)
- Source IP flagged across multiple attempts
- Alert escalation threshold behavior

### 2. Reconnaissance / Port Scanning
Ran Nmap scans from Kali to observe how network-level activity surfaces in Wazuh logs.

---

## Detection & Analysis

Wazuh successfully generated alerts for both scenarios. Key observations:

- Sysmon Event ID 3 (Network Connection) captured outbound scan traffic
- Authentication failure logs from Windows Security Event Log (Event ID 4625) fed into Wazuh in real time
- Wazuh's built-in rules fired correctly without custom tuning — showing the value of out-of-the-box SIEM configuration for common attack patterns

Full detailed findings are documented in the lab reports:
- 📄 [`SOC_Lab.pdf`](./SOC_Lab.pdf) — full lab setup and methodology
- 📄 [`soc_bruteforce_lab_report.pdf`](./soc_bruteforce_lab_report.pdf) — brute-force detection report

---

## What I Learned

- How to deploy and configure Wazuh agents on Windows endpoints
- The role of Sysmon in enriching Windows event logs beyond default telemetry
- How SIEM rules correlate individual log events into meaningful alerts
- The analyst workflow: alert → investigate → confirm → document

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Wazuh | SIEM — log collection, correlation, alerting |
| Sysmon | Windows endpoint telemetry |
| Kali Linux | Attack simulation |
| VirtualBox | VM environment |
| Hydra | Brute-force simulation |
| Nmap | Network reconnaissance |

---

## 📌 Topics

`soc` `wazuh` `siem` `blue-team` `incident-response` `sysmon` `home-lab` `threat-detection` `kali-linux`
