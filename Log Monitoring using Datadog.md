# Home-Lab Setup: Log Monitoring using Datadog

## Objective
To set up a home lab for centralized log monitoring using **Datadog**, configured to collect logs from an **Ubuntu Server** and a **Windows machine**. This setup will allow for real-time log aggregation, analysis, and visualization.

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
- **Datadog Agent**: [Official website](https://www.datadoghq.com/)
- **Datadog Account**: [Sign up here](https://app.datadoghq.com/signup)
- Stable internet connection.

---

## Step-by-Step Guide
---
### Part 1: Setting Up Datadog
**Objective**: Create a Datadog account and configure a Datadog Agent to collect logs.

#### Step 1: Create a Datadog Account
1. Go to the [Datadog website](https://app.datadoghq.com/signup).
2. Sign up with an email address or login using an existing account.
3. Obtain your **API Key** from the Datadog dashboard:
   - Navigate to **Integrations > APIs**.
   - Copy the generated API key for use in agent configuration.

---

### Part 2: Setting Up Ubuntu Server
**Objective**: Install the Datadog Agent on an Ubuntu machine to forward system logs to Datadog.

#### Step 1: Set Up the Ubuntu Virtual Machine
1. Download the Ubuntu Server 22.04 ISO.
2. Create a new virtual machine using your preferred virtualization platform:
   - Assign 2 CPU cores, 4 GB RAM, and 20 GB disk space.
   - Mount the Ubuntu ISO and install Ubuntu.
3. Configure a static IP address for network stability.

#### Step 2: Install the Datadog Agent on Ubuntu
1. Update system packages:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Install the Datadog Agent:
```
DD_API_KEY=<YOUR_API_KEY> bash -c "$(curl -L https://s3.amazonaws.com/dd-agent/scripts/install_script.sh)"
```
Replace <YOUR_API_KEY> with the API key copied earlier.
3. Enable log collection:
- Edit the agent configuration file:
```
sudo nano /etc/datadog-agent/datadog.yaml
```
- Add or update the following lines:
```
logs_enabled: true
```
- Save and close the file.
4. Configure log sources:
- Create a configuration file for log sources:
```
sudo nano /etc/datadog-agent/conf.d/system_logs.d/conf.yaml
```
- Add the following configuration:
```
logs:
  - type: file
    path: /var/log/syslog
    service: ubuntu
    source: syslog
```
- Save and close the file.
5. Restart the Datadog Agent:
bash
Copy code
sudo systemctl restart datadog-agent
---
### Part 3: Setting Up Windows Machine
Objective: Install the Datadog Agent on a Windows machine to forward logs to Datadog.

#### Step 1: Set Up the Windows Virtual Machine
1. Install Windows in a virtual machine using your preferred platform.
2. Configure networking and install updates.
#### Step 2: Install the Datadog Agent on Windows
1. Download the Datadog Agent installer for Windows from the Datadog website.
2. Run the installer and follow the prompts:
- Enter the API Key during installation.
- Enable log collection when prompted.
3. Configure log sources:
- Open the Datadog Agent Manager from the system tray.
- Navigate to the Log Collection tab.
- Add event log sources:
   - Enable logs for `Application`, `Security`, and `System`.
   - Apply and save the configuration.
 
---
### Part 4: Viewing Logs in Datadog
Objective: Verify and visualize logs from Ubuntu and Windows systems in Datadog.

#### Step 1: Access Logs in Datadog
1. Log in to your Datadog dashboard.
2. Navigate to Logs > Live Tail.
3. Filter logs by source or service (e.g., `ubuntu` or `syslog`).
#### Step 2: Create Dashboards
1. Navigate to Dashboards and create a new dashboard.
2. Add widgets for:
- Log streams filtered by source or severity.
- Log analytics showing trends over time.
3. Save the dashboard for continuous monitoring.
---
## Conclusion
This guide demonstrates how to set up a home lab using Datadog for centralized log monitoring. By aggregating logs from both Ubuntu and Windows machines, you gain real-time insights into system performance and security, enabling better decision-making and proactive responses to potential issues.

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
