# Home-Lab Setup: Security Monitoring Using ManageEngine Log360

## Objective
To build a home lab for security monitoring using **ManageEngine Log360**, a comprehensive log management and security monitoring tool. This setup includes configuring an Ubuntu server and a Windows machine to send security-related logs to Log360 for centralized analysis and monitoring.

---

## Home-Lab Setup

---

## Requirements
- **Virtualization Platform**:
  - [VirtualBox](https://www.virtualbox.org/)
  - [VMware Workstation Pro](https://www.vmware.com/products/workstation-pro.html)
  - [Proxmox VE](https://www.proxmox.com/en/proxmox-ve)
- **Ubuntu Server 22.04 ISO**: [Download here](https://releases.ubuntu.com/22.04/)
- **Windows ISO**: [Download here](https://www.microsoft.com/en-us/software-download/windows10)
- **ManageEngine Log360**: [Download here](https://www.manageengine.com/log-management/download.html)
- Stable internet connection.

---

## Step-by-Step Guide

### Part 1: Setting Up ManageEngine Log360
**Objective**: Install and configure ManageEngine Log360 as the central security monitoring platform.

#### Step 1: Set Up a Windows VM for Log360
1. Download the Windows ISO and create a new VM using your virtualization platform:
   - Assign 2 CPUs, 4 GB RAM, and 40 GB of disk space.
   - Install Windows and configure the VM with a static IP address.
2. Install necessary Windows updates and ensure network connectivity.

#### Step 2: Install ManageEngine Log360
1. Download the Log360 installer from the [official website](https://www.manageengine.com/log-management/download.html).
2. Install Log360 on the Windows VM:
   - Run the installer and follow the setup wizard.
   - Choose default settings or customize the installation directory if needed.
3. Start the ManageEngine Log360 service:
   - Open **Services** and ensure the **ManageEngine Log360** service is running.

#### Step 3: Configure Log360
1. Access the Log360 web interface:
   - Open a browser and navigate to `http://<log360-server-ip>:8080`.
   - Log in with the default admin credentials and change the password.
2. Configure the security monitoring workspace:
   - Navigate to **Settings > Add Log Sources**.
   - Add sources for Linux (Syslog) and Windows Event Logs.

---

### Part 2: Setting Up Ubuntu Server for Security Monitoring
**Objective**: Configure Ubuntu to send security-related logs to Log360 using the Syslog protocol.

#### Step 1: Set Up the Ubuntu VM
1. Download the Ubuntu Server 22.04 ISO.
2. Create a new VM and allocate:
   - 1 CPU, 2 GB RAM, and 20 GB of disk space.
   - Install Ubuntu Server with a static IP address.
3. Complete the initial setup (user creation, updates, etc.).

#### Step 2: Enable Security Log Forwarding
1. Install and configure `rsyslog` for log forwarding:
   ```bash
   sudo apt update && sudo apt install rsyslog -y
  ``
2. Edit the rsyslog configuration file:
```
sudo nano /etc/rsyslog.conf
```
- Add the following line to forward logs:
```
*.* @<log360-server-ip>:514
```
- Save and close the file.
3. Restart the rsyslog service:
```
sudo systemctl restart rsyslog
```
4. Verify that security logs are forwarded:
- Check the Log360 interface for incoming logs from the Ubuntu server.

---
### Part 3: Setting Up Windows Machine for Security Monitoring
Objective: Configure a Windows client machine to forward event logs to Log360.

#### Step 1: Set Up the Windows VM
1. Download the Windows ISO and create a new VM:
- Assign 2 CPUs, 4 GB RAM, and 40 GB of disk space.
- Install Windows and configure it with a static IP address.
#### Step 2: Enable Windows Security Event Log Forwarding
1. Download and install the Event Log Forwarder tool (available with Log360).
2. Configure the Event Log Forwarder:
- Set the Log360 server's IP address and port (default: 514).
- Specify which event logs to monitor:
  - Security for login attempts and access control.
  - System for kernel-level alerts.
  - Application for application-specific security logs.
3. Start the forwarding service to send event logs to Log360.
#### Step 3: Verify Logs
1. Open the Log360 dashboard and confirm that logs from the Windows machine are being received.
2. Navigate to Security Logs > Windows Event Logs to analyze incoming security events.

---
### Part 4: Analyzing and Monitoring Logs in Log360
Objective: Leverage Log360's tools to monitor security events, generate alerts, and create reports.

#### Step 1: Configure Security Dashboards
1. Navigate to Dashboard > Add New Widget.
2. Add widgets for:
- Real-time log monitoring.
- Event severity analysis (e.g., Critical, Warning, Informational).
- Authentication success/failure trends.
#### Step 2: Set Up Security Alerts
1. Go to Alerts > Create Alert.
2. Define alert triggers:
- Unauthorized login attempts.
- Sudden spike in error logs.
- System-level warnings.
3. Enable email or SMS notifications for critical alerts.
#### Step 3: Generate Compliance and Security Reports
1. Navigate to Reports > Security Reports.
2. Generate reports such as:
- User login activity.
- Privilege escalation attempts.
- Failed authentication attempts.
#### Step 4: Enable Threat Detection
1. Use the Threat Intelligence Module in Log360.
2. Correlate logs to identify potential security threats (e.g., brute force attacks).
3. Customize rules for specific use cases or compliance requirements.

### Conclusion
This home lab demonstrates the setup of ManageEngine Log360 for centralized security monitoring. By integrating Ubuntu and Windows machines, you can monitor security events, generate actionable insights, and respond to potential threats effectively.

With ManageEngine Log360, your home lab becomes a powerful tool for learning security monitoring and improving your cybersecurity skills!

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
