Wazuh Installation Guide (Home SOC Lab)

This document provides step-by-step instructions to install Wazuh Server on Ubuntu and integrate it into a local SOC lab environment.

Environment target:

Ubuntu VM (16GB RAM / 4 cores)

VMware Workstation

Local network lab

Integration with OpenVAS

Windows + Kali agents

1. System Requirements

Recommended:

Ubuntu 22.04 LTS

16GB RAM

4 CPU cores

80GB+ disk free

Docker installed

Check system:

free -h
nproc
df -h

2. Update System
sudo apt update
sudo apt full-upgrade -y
sudo apt autoremove -y

3. Install Dependencies
sudo apt install curl apt-transport-https unzip wget lsb-release gnupg -y

4. Install Wazuh (All-in-One)

Download official installer:

curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
chmod +x wazuh-install.sh


Run installation:

sudo ./wazuh-install.sh -a


This installs:

Wazuh Manager

Wazuh Indexer

Wazuh Dashboard

Installation time: 10–25 minutes.

5. Get Dashboard Credentials

After installation:

sudo tar -xvf wazuh-install-files.tar


Open file:

cat wazuh-install-files/wazuh-passwords.txt


Find:

admin password

6. Access Wazuh Dashboard

Open browser:

https://<ubuntu-ip>


Example:

https://192.168.1.50


Login:

Username: admin

Password: from wazuh-passwords.txt

7. Install Windows Agent

Download agent:

https://packages.wazuh.com/4.x/windows/wazuh-agent-4.x.x.msi

Install with:

Manager IP: <ubuntu-ip>


Start service:

net start wazuh

8. Install Linux Agent (Kali)
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo apt-key add -
echo "deb https://packages.wazuh.com/4.x/apt stable main" | sudo tee /etc/apt/sources.list.d/wazuh.list

sudo apt update
sudo apt install wazuh-agent -y


Edit config:

sudo nano /var/ossec/etc/ossec.conf


Set manager IP.

Start agent:

sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent

9. Verify Agents

On Wazuh server:

sudo /var/ossec/bin/agent_control -l


Agents should appear as:

Active

10. OpenVAS Integration (Next Phase)

Planned integration:

OpenVAS scan → vulnerability detected
↓
Wazuh ingest logs
↓
Dashboard alert correlation

11. Recommended Architecture
Ubuntu SOC Server
│
├── Wazuh Manager
├── Wazuh Indexer
├── Wazuh Dashboard
├── OpenVAS
└── Nginx reverse proxy

Agents:
├── Windows 10
├── Kali Linux
└── macOS

12. Troubleshooting

Check services:

systemctl status wazuh-manager
systemctl status wazuh-indexer
systemctl status wazuh-dashboard


Logs:

/var/ossec/logs/
/var/log/wazuh-indexer/
/var/log/wazuh-dashboard/

13. Next Steps

Add Windows + Kali agents

Generate attack logs

Configure detection rules

Integrate OpenVAS results

Map MITRE ATT&CK

Author

Home SOC Lab Project
OpenVAS + Wazuh + Local Network Monitoring
