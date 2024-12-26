# **Creating a Home Lab Using Splunk on Ubuntu Server 22.04**

---

## **Objective**  
Set up a Splunk home lab on Ubuntu Server 22.04 to ingest and analyze syslogs from another Ubuntu server using the Splunk Universal Forwarder (UF).

---

## **Requirements**  
1. **Virtualization Platform:**  
   - [VirtualBox](https://www.virtualbox.org/) (Free)  
   - [VMware Workstation Pro](https://www.vmware.com/products/workstation-pro.html) (Paid)  
   - [Proxmox VE](https://www.proxmox.com/en/) (Free and Open Source)  

2. **Resources:**  
   - **Ubuntu Server ISO**: [Ubuntu 22.04](https://ubuntu.com/download/server)  
   - **Splunk Software**:  
     - [Splunk Enterprise](https://www.splunk.com/en_us/download/splunk-enterprise.html)  
     - [Splunk Universal Forwarder](https://www.splunk.com/en_us/download/universal-forwarder.html)  
   - **Minimum Hardware Requirements for VMs**:  
     - Splunk Server:  
       - 2 CPU cores  
       - 4 GB RAM (8 GB recommended)  
       - 40 GB disk space  
     - Syslog Server:  
       - 1 CPU core  
       - 2 GB RAM  
       - 20 GB disk space  

---

## **Step-by-Step Guide**

### **Part 1: Install Splunk on Ubuntu Server (Main Splunk Server)**

#### **Step 1: Install Virtualization Platform**
1. Download and install your preferred virtualization platform:
   - [VirtualBox Installation Guide](https://www.virtualbox.org/manual/ch02.html)  
   - [VMware Workstation Pro Guide](https://www.vmware.com/products/workstation-pro/resources.html)  
   - [Proxmox VE Installation Guide](https://pve.proxmox.com/wiki/Install_Proxmox_VE)  

#### **Step 2: Create and Configure Virtual Machines**
1. Create a virtual machine for the Splunk server:
   - OS: Ubuntu 22.04 (64-bit)  
   - Memory: Minimum 4 GB  
   - Hard Disk: Minimum 40 GB  

2. Create a second VM for the syslog server:
   - OS: Ubuntu 22.04 (64-bit)  
   - Memory: Minimum 2 GB  
   - Hard Disk: Minimum 20 GB  

#### **Step 3: Install Ubuntu Server on Both VMs**
1. Install Ubuntu Server on both virtual machines:
   - Follow the standard Ubuntu Server installation steps.  
   - Install OpenSSH server for remote access.  

#### **Step 4: Update Ubuntu on Both VMs**
1. After installation, log in to both servers and update the packages:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
#### **Step 5: Install Splunk on Main Server**
1.  Download and install Splunk Enterprise:
  ```
  wget -O splunk.deb https://download.splunk.com/products/splunk/releases/latest/linux/splunk-latest-linux-2.6-amd64.deb
  sudo dpkg -i splunk.deb
  sudo /opt/splunk/bin/splunk start --accept-license
  sudo /opt/splunk/bin/splunk enable boot-start
  ```
2. Access Splunk via the web interface:
  ```
  http://<Splunk_Server_IP>:8000
```
  ### Part 2: Configure Syslog Collection from the Second Ubuntu Server
 #### Step 6: Install Splunk Universal Forwarder on Syslog Server
  1. Download Splunk Universal Forwarder:
  ```
  wget -O splunkforwarder.deb https://download.splunk.com/products/universalforwarder/releases/latest/linux/splunkforwarder-latest-linux-2.6-amd64.deb
  ```
  2. Install the Universal Forwarder:
  ```
  sudo dpkg -i splunkforwarder.deb
  ```
  3. Start the Splunk Universal Forwarder and accept the license:
  ```
  sudo /opt/splunkforwarder/bin/splunk start --accept-license
  sudo /opt/splunkforwarder/bin/splunk enable boot-start
  ```
  4. Configure Splunk Forwarder to send logs to the Splunk server:
  ```
  sudo /opt/splunkforwarder/bin/splunk add forward-server <Splunk_Server_IP>:9997 -auth admin:<your_password>
  ```
  #### Step 7: Configure Syslog Monitoring on Syslog Server
  1. Create a directory to store syslog files:
  ```
  sudo mkdir -p /var/log/syslog
  ```
  2. Configure rsyslog to output logs to /var/log/syslog. Edit the rsyslog configuration file:
  ```
  sudo nano /etc/rsyslog.conf
  ```
  - Uncomment or add:
  ```
  *.* /var/log/syslog
  ```
  - Restart rsyslog:
  ```
  sudo systemctl restart rsyslog
  ```
  #### Step 8: Add Syslog Directory to Splunk Universal Forwarder Inputs
  1. Add the syslog directory as a monitored input:
  ```
  sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/syslog
  ```
  #### Step 9: Configure Splunk Server to Receive Logs
  1. On the Splunk server, enable receiving:
  ```
  sudo /opt/splunk/bin/splunk enable listen 9997
  ```
  2. Verify the forwarder connection:
  ```
  sudo /opt/splunk/bin/splunk list forward-server
  ```
  #### Step 10: Search for Syslogs in Splunk
  1. Access Splunk via the web interface:
  ```
  http://<Splunk_Server_IP>:8000
  ```
  2. Navigate to Search & Reporting and search for incoming logs using:
  ```
  index=_internal OR index=main
  ```
  ## Conclusion
  This setup allows you to analyze syslogs from a second Ubuntu server using Splunk's powerful interface. By following this guide, you now have a functional home lab for practicing security investigations, monitoring, and analysis. Experiment with different log sources and enrich your skills further!