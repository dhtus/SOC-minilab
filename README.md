🛡️ Home SOC Lab – Wazuh + OpenVAS

A practical home SOC lab built with Wazuh, OpenVAS, and a local network to simulate real-world security monitoring and vulnerability management.

This project is designed for:

SOC analyst practice

cybersecurity learning

interview demonstration

hands-on detection engineering

📐 Architecture Overview
                    ┌──────────────┐
                    │   Kali Linux  │
                    │ Attack / Scan │
                    └──────┬───────┘
                           │
                           ▼
┌────────────────────────────────────────────┐
│                Ubuntu SOC Server           │
│                                            │
│  ┌─────────────┐    ┌──────────────────┐   │
│  │  OpenVAS    │───▶│ Vulnerability DB │   │
│  └─────────────┘    └──────────────────┘   │
│                                            │
│  ┌─────────────┐                           │
│  │  Wazuh      │◀─────────────── Logs ─────┤
│  │  Manager    │                           │
│  └─────────────┘                           │
│         │                                  │
│         ▼                                  │
│  ┌─────────────┐                           │
│  │ Wazuh Index │                           │
│  └─────────────┘                           │
│         │                                  │
│         ▼                                  │
│  ┌─────────────┐                           │
│  │ Dashboard   │                           │
│  └─────────────┘                           │
└────────────────────────────────────────────┘
            ▲                     ▲
            │                     │
            │                     │
   ┌────────┴────────┐   ┌────────┴────────┐
   │ Windows Agent   │   │ macOS / Linux   │
   │ Event + Sysmon  │   │ Audit logs      │
   └─────────────────┘   └─────────────────┘

⚙️ Environment
Component	Spec
Host	Windows 11 Pro
CPU	Intel Core Ultra
RAM	32GB
Hypervisor	VMware Workstation
SOC Server	Ubuntu 22.04
SOC RAM	16GB
SOC CPU	4 cores
🚀 Features

Vulnerability scanning with OpenVAS

Endpoint detection with Wazuh

Centralized log analysis

MITRE ATT&CK mapping

Local network monitoring

Real attack simulation

🧱 Components
🟢 Ubuntu SOC Server

Runs:

Wazuh Manager

Wazuh Indexer

Wazuh Dashboard

OpenVAS

Nginx reverse proxy

🟦 Windows Lab

Wazuh agent

Sysmon

event log monitoring

brute-force simulation target

🔴 Kali Linux

attack simulation

recon

exploit testing

traffic generation

📦 Installation – Wazuh (All-in-One)
1. Update system
sudo apt update
sudo apt full-upgrade -y

2. Install Wazuh
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
chmod +x wazuh-install.sh
sudo ./wazuh-install.sh -a


Installation time: 10–25 minutes.

3. Retrieve dashboard credentials
sudo tar -xvf wazuh-install-files.tar
cat wazuh-install-files/wazuh-passwords.txt

4. Access dashboard
https://<ubuntu-ip>


Login:

admin / password from file

🧩 Agent Setup
Windows

Download:

https://packages.wazuh.com

Install agent → set manager IP → start service.

Kali Linux
sudo apt install wazuh-agent -y


Edit:

/var/ossec/etc/ossec.conf


Set manager IP.

Start:

sudo systemctl start wazuh-agent

🔍 OpenVAS Integration

OpenVAS will be used for:

vulnerability discovery

network scanning

asset exposure mapping

Workflow:

OpenVAS scan
↓
Vulnerability identified
↓
Wazuh correlation
↓
SOC alert

🧠 SOC Detection Flow
Kali attack
↓
Windows event generated
↓
Wazuh ingest logs
↓
Detection rule triggered
↓
Dashboard alert
↓
OpenVAS confirms vulnerability

📊 MITRE ATT&CK Mapping

This lab enables practice for:

Initial access

Privilege escalation

Persistence

Lateral movement

Exfiltration

🛠️ Troubleshooting
Services
systemctl status wazuh-manager
systemctl status wazuh-dashboard
systemctl status wazuh-indexer

Logs
/var/ossec/logs/
/var/log/wazuh-indexer/
/var/log/wazuh-dashboard/

📈 Next Phases

Windows attack simulation

OpenVAS automation

threat intelligence feeds

Sigma rules

SOC playbooks

🎯 Goal

Build a realistic SOC home lab capable of:

detection engineering practice

security monitoring

vulnerability management

interview-ready demonstrations

👤 Author

Cybersecurity Home Lab Project
Wazuh + OpenVAS + VMware + Local Network
