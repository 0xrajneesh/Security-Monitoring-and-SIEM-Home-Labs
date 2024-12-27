# Home-Lab Setup: Security Monitoring using SecurityOnion
## Objective
To create a home lab using Security Onion for monitoring security events from an Ubuntu server and a Windows machine. This step-by-step guide covers installing Security Onion, setting up syslog collection on Ubuntu, and configuring event collection on Windows.

---

## Requirements
- **Virtualization Platform**:
  - [VirtualBox](https://www.virtualbox.org/)
  - [VMware Workstation Pro](https://www.vmware.com/products/workstation-pro.html)
  - [Proxmox VE](https://www.proxmox.com/en/proxmox-ve)
- **Security Onion ISO**: [Download here](https://securityonion.net/)
- **Ubuntu Server 22.04 ISO**: [Download here](https://releases.ubuntu.com/22.04/)
- **Windows ISO**: [Download here](https://www.microsoft.com/en-us/software-download/windows10)
- Stable internet connection.
- At least 16 GB RAM and 100 GB disk space for the Security Onion VM.

---

## Step-by-Step Guide

---
### Part 1: Setting Up Security Onion
**Objective**: Install and configure Security Onion to act as the central log and event monitoring platform.

#### Step 1: Install Security Onion
1. Download the Security Onion ISO.
2. Create a new virtual machine in your virtualization platform:
   - Assign at least 8 CPU cores, 16 GB RAM, and 100 GB disk space.
   - Mount the Security Onion ISO and boot from it.
3. Follow the installation steps:
   - Choose **Evaluation Mode** for simplicity.
   - Configure the hostname and assign a static IP.
   - Complete the installation and restart the VM.
4. After reboot, the system will display a URL for the Security Onion web interface.

#### Step 2: Access the Security Onion Web Interface
1. Open a browser and navigate to the URL provided during the installation.
2. Log in using the admin credentials created during setup.

---

### Part 2: Setting Up Ubuntu Server to Collect Syslog
**Objective**: Configure the Ubuntu server to forward syslog to Security Onion.

#### Step 1: Install and Configure Rsyslog
1. Log in to the Ubuntu server.
2. Update the system:
   ```bash
   sudo apt update && sudo apt upgrade -y
  ``
3. Install rsyslog:
```
sudo apt install rsyslog -y
```
4. Configure rsyslog to forward logs to Security Onion:
```
sudo nano /etc/rsyslog.d/50-securityonion.conf
```
- Add the following:
```
*.* @<Security-Onion-IP>:514
```
5. Restart the rsyslog service:
```
sudo systemctl restart rsyslog
```
#### Step 2: Verify Logs in Security Onion
1. In the Security Onion web interface, navigate to Kibana.
2. Search for logs from the Ubuntu server using its hostname or IP address.
---
### Part 3: Setting Up Windows to Collect Events
Objective: Configure a Windows machine to forward event logs to Security Onion.

#### Step 1: Download and Install Winlogbeat
1. Download Winlogbeat on the Windows machine.
2. Extract the Winlogbeat package and navigate to the extracted folder.
#### Step 2: Configure Winlogbeat
1. Open winlogbeat.yml in a text editor.
2. Configure the output to forward logs to Security Onion:
```
output:
  logstash:
    hosts: ["<Security-Onion-IP>:5044"]
```
3. Define the event logs to collect:
```
winlogbeat.event_logs:
  - name: Application
  - name: Security
  - name: System
```
#### Step 3: Install and Start Winlogbeat
1. Install Winlogbeat as a service:
```
.\install-service-winlogbeat.ps1
```
2. Start the service:
```
Start-Service winlogbeat
```
#### Step 4: Verify Logs in Security Onion
1. In the Security Onion web interface, navigate to Kibana.
2. Search for logs from the Windows machine using its hostname or IP address.

---

## Conclusion
This guide demonstrates how to set up Security Onion as a centralized platform for monitoring security events from an Ubuntu server and a Windows machine. With this home lab, you can explore real-world log analysis, event correlation, and threat detection to enhance your cybersecurity skills.

# 🌟 Ultimate Security Analyst Course🌟

Get unstuck and complete all the tasks with detailed step-by-step videos plus

- **Video Tutorials**: 145+ Videos with step-by-step guide.
- **13 Hands-on course**(beginner to Medium Level): Including courses on Cybersecurity 101, IT networking, Server and Cloud, Splunk for Beginners, Endpoint Investigation, Network Investigation, Security Compliance, Offensive security
- **90 Days Challenge**(Medium to Advanced Level): You are expected to finish 9 Hands-on Projects in 90 Days covering tools such as Splunk, ELK, Wireshark, Velociraptor, Osquery, AWS etc
- **LifeTime Access**: Once you finish the 90-Days Challenge, you still be having access to all the modules for lifetime
- **Join the Community**: Access our exclusive community platform to share insights, seek advice, and learn from fellow challengers.
- **Earn Recognition**: Complete the challenge within 90 days to earn a shoutout during our Hall of Fame celebration on LinkedIn and YouTube! 🏆📣

Want to get started?

<a href="https://learn.haxsecurity.com/services/securitychallenge">
  <img src="https://img.shields.io/badge/-Enroll%20Now-008CBA?&style=for-the-badge&logo=Book&logoColor=white" alt="Enroll Now"/>
</a>
