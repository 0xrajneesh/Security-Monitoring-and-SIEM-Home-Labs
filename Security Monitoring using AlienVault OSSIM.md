# Home-Lab Setup: Security Monitoring with AlienVault OSSIM

## Objective
To create a home lab using **AlienVault OSSIM** for monitoring security events from an **Ubuntu Server** and a **Windows machine**. The lab will simulate real-world security monitoring scenarios by collecting and analyzing logs and events from the connected systems.



## Home-Lab Setup


## Requirements
- **Virtualization Platform**:
  - [VirtualBox](https://www.virtualbox.org/)
  - [VMware Workstation Pro](https://www.vmware.com/products/workstation-pro.html)
  - [Proxmox VE](https://www.proxmox.com/en/proxmox-ve)
- **AlienVault OSSIM ISO**: [Download here](https://cybersecurity.att.com/products/ossim/download)
- **Ubuntu Server 22.04 ISO**: [Download here](https://releases.ubuntu.com/22.04/)
- **Windows ISO**: [Download here](https://www.microsoft.com/en-us/software-download/windows10)
- **NXLog**: [Official website](https://nxlog.co/products/nxlog-community-edition)
- **Stable Internet Connection**



## Step-by-Step Guide

### Part 1: Setting Up AlienVault OSSIM
**Objective**: Install and configure AlienVault OSSIM as the central monitoring tool in the lab.

#### Step 1: Set Up the Virtual Machine
1. Download the AlienVault OSSIM ISO.
2. Choose a virtualization platform (VirtualBox, VMware, or Proxmox).
3. Create a new virtual machine:
   - Assign at least **4 CPU cores** and **8 GB RAM**.
   - Allocate **100 GB disk space**.
   - Mount the AlienVault ISO and boot the VM.
4. Follow the on-screen installation steps:
   - Configure a hostname and network settings.
   - Set up a root password.
5. After installation, access the OSSIM web interface:
   - Open a browser and navigate to `https://<server-ip>/ossim`.

#### Step 2: Post-Installation Configuration
1. Log in with the default admin credentials and change the password.
2. Update AlienVault OSSIM to the latest version:
   ```bash
   apt update && apt upgrade -y
   ```
3. Configure basic settings:
- Network interfaces.
- Timezone and NTP settings.

---
### Part 2: Setting Up Ubuntu Server
Objective: Configure an Ubuntu Server to send security logs to AlienVault OSSIM.

#### Step 1: Install the Ubuntu Server
1. Download the Ubuntu Server 22.04 ISO.
2. Create a new virtual machine:
- Assign 2 CPU cores, 4 GB RAM, and 20 GB disk space.
- Mount the Ubuntu ISO and install the OS.
3. Set a static IP address for the server.
#### Step 2: Install and Configure Syslog for OSSIM
1. Install the required packages:
```
sudo apt update && sudo apt install rsyslog -y
```
2. Configure rsyslog to forward logs to OSSIM:
- Edit the rsyslog configuration file:
```
sudo nano /etc/rsyslog.conf
```
- Add the following line at the end of the file:
```
*.* @<ossim-ip>:514
```
- Restart the rsyslog service:
```
sudo systemctl restart rsyslog
```
---
### Part 3: Setting Up Windows Machine
Objective: Configure a Windows machine to forward event logs to AlienVault OSSIM.

#### Step 1: Install the Windows Machine
1. Create a new virtual machine using the Windows ISO.
2. Assign 2 CPU cores, 4 GB RAM, and 40 GB disk space.
3. Install Windows and configure a static IP.
#### Step 2: Install NXLog for Log Forwarding
1. Download the NXLog Community Edition.
2. Install NXLog on the Windows machine:
- Follow the installation wizard steps.
3. Configure NXLog:
- Open the configuration file (C:\Program Files (x86)\nxlog\conf\nxlog.conf).
- Modify the file to include:
```
<Extension _syslog>
    Module      xm_syslog
</Extension>

<Input in>
    Module      im_msvistalog
</Input>

<Output out>
    Module      om_udp
    Host        <ossim-ip>
    Port        514
    Exec        to_syslog_snare();
</Output>

<Route 1>
    Path        in => out
</Route>
```
- Save the file and restart the NXLog service.

---
### Part 4: Verify Event Collection in AlienVault OSSIM
Objective: Ensure logs and events from Ubuntu and Windows are being collected by OSSIM.

#### Step 1: Check Log Sources in OSSIM
1. Log in to the OSSIM web interface.
2. Navigate to Environment > Assets.
3. Add the Ubuntu and Windows machines as assets by their IPs.

#### Step 2: Analyze Logs
Go to Analysis > Security Events.
Confirm that logs from both machines are appearing in the dashboard.

## Conclusion
This home lab demonstrates how to set up AlienVault OSSIM for centralized security monitoring of an Ubuntu server and a Windows machine. With AlienVault OSSIM, you can monitor logs, analyze security events, and gain valuable insights to improve your system’s security posture.
