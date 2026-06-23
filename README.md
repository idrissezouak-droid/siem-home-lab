# siem-home-lab
ELK Stack SIEM home lab with custom detection rules - Elastic 8.x on Kali Linux

# 🛡️ SIEM Home Lab — Elastic Stack on Kali Linux

![Elastic](https://img.shields.io/badge/Elastic-8.x-005571?style=flat&logo=elastic)
![Kali Linux](https://img.shields.io/badge/Kali-Linux-557C94?style=flat&logo=kali-linux)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE-ATT%26CK-red?style=flat)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat)

## 📌 Objective

Built a fully functional SIEM home lab from scratch using the Elastic Stack (Elasticsearch + Kibana + Filebeat + Auditbeat) on Kali Linux. The goal was to simulate real-world SOC analyst work: ingesting logs, creating custom detection rules, triggering alerts, and building security dashboards.

---

## 🏗️ Architecture

Kali Linux (VirtualBox)

├── Elasticsearch 8.x    → Log storage & indexing

├── Kibana 8.x           → SIEM interface & dashboards

├── Filebeat             → Collects /var/log/ (auth, syslog)

└── Auditbeat            → Monitors processes, files & syscalls
---

## 🔍 Detection Rules Created

| Rule Name | Query | MITRE Technique | Severity |
|---|---|---|---|
| SSH Brute Force Detection | `event.dataset: "system.auth" AND event.outcome: "failure"` | T1110.001 | High |
| Privilege Escalation Attempt | `event.dataset: "system.auth" AND (message: *sudo* OR message: *su* OR message: *root*) AND event.outcome: "failure"` | T1548.003 | High |
| Suspicious User Added to Privileged Group | `event.dataset: "system.auth" AND (message: *useradd* OR message: *usermod* OR message: *wheel*)` | T1136.001 / T1098 | Critical |
| Nmap Port Scan Detected | `event.module: "auditd" AND event.action: "executed" AND process.name: "nmap"` | T1046 | Medium |

---

## ⚔️ Attack Simulation

Simulated a **SSH Brute Force attack** against localhost to trigger detection rules:

```bash
# Started SSH service
sudo systemctl start ssh

# Simulated 8 failed login attempts
ssh wronguser@localhost  # repeated with wrong passwords
```

**Result:** 8 alerts triggered — Severity: High — Risk Score: 73

---

## 📊 Dashboard

Built a custom Kibana security dashboard with 3 visualizations:
- **Failed logins over time** — Bar chart showing authentication failures timeline
- **Top targeted users** — Pie chart showing most attacked usernames
- **Events by category** — Pie chart showing distribution of event types

---

## 🖼️ Screenshots

### Detection Rules
![Detection Rules](VirtualBox_kali%20linux_23_06_2026_01_06_55.png)

### Alerts Triggered
![Alerts](VirtualBox_kali%20linux_23_06_2026_01_13_37.png)

### Security Dashboard
![Dashboard](VirtualBox_kali%20linux_23_06_2026_01_24_03.png)

---

## 🛠️ Tools & Technologies

- **Elasticsearch 8.x** — Log indexing and storage
- **Kibana 8.x** — SIEM interface, detection rules, dashboards
- **Filebeat** — Log shipper for system auth logs
- **Auditbeat** — Linux audit framework monitoring
- **Kali Linux** — Attack simulation platform
- **VirtualBox** — Virtualization

---

## 📚 Skills Demonstrated

- ELK Stack deployment and configuration from scratch
- Custom SIEM detection rule creation (KQL)
- MITRE ATT&CK framework mapping
- Attack simulation and alert validation
- Security dashboard design
- Linux log analysis (auth.log, syslog, auditd)

---

## 👤 Author

**Idriss Ezouak**  
Bac+2 Cybersecurity — IGA Morocco  
TryHackMe SOC Level 1  
