# **Security Monitoring using Splunk**

---

## **Objective**  

Set up a Splunk Home Lab on Ubuntu Server to centralize log collection and analysis. Configure the Splunk Universal Forwarder on an Ubuntu machine to forward syslogs and application logs. Enable Windows Event Log monitoring on a Windows machine and forward logs to the Splunk server for comprehensive monitoring.
  

---

## **Home-Lab Set up**  
The home-lab set up is designed with VMware workstation.
![Blue and Gold Elegant Minimalist Jewelry Promotions Business Floor Decal (150 x 120 mm)](https://github.com/user-attachments/assets/b1cc0dbb-be1f-46d2-a2c7-c4d83d5aa9c9)


---


## **Requirements**  
1. **Virtualization Platform:**  
   - [VirtualBox](https://www.virtualbox.org/) (Free)  
   - [VMware Workstation Pro](https://www.vmware.com/products/workstation-pro.html) (Free)  
   - [Proxmox VE](https://www.proxmox.com/en/) (Free and Open Source)  

2. **Resources:**  
    - **Ubuntu Server ISO**: [Ubuntu 22.04](https://ubuntu.com/download/server)  
   - **Windows ISO**: [Windows Server Evaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server)  
   - **Splunk Software**:  
     - [Splunk Enterprise](https://www.splunk.com/en_us/download/splunk-enterprise.html)  
     - [Splunk Universal Forwarder](https://www.splunk.com/en_us/download/universal-forwarder.html)  
   - **Minimum Hardware Requirements for VMs**:  
     - Splunk Server:  
       - 2 CPU cores  
       - 4 GB RAM (8 GB recommended)  
       - 40 GB disk space  
     - Linux Server:  
       - 1 CPU core  
       - 2 GB RAM  
       - 20 GB disk space  
     - Windows Machine:  
       - 2 CPU cores  
       - 4 GB RAM  
       - 40 GB disk space  

---

## **Step-by-Step Guide**

### **Part 1: Install Splunk on Ubuntu Server (Main Splunk Server)**

#### **Step 1: Install Virtualization Platform**
1. Download and install your preferred virtualization platform:
   - [VirtualBox Installation Guide](https://www.virtualbox.org/manual/ch02.html)  
   - [VMware Workstation Pro Guide](https://www.vmware.com/products/workstation-pro/resources.html)  
   - [Proxmox VE Installation Guide](https://pve.proxmox.com/wiki/Install_Proxmox_VE)  


#### **Step 3: Update Ubuntu on Both VMs**
1. After installation, log in to both servers and update the packages:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
#### **Step 4: Install Splunk on Main Server**
1.  Download and install Splunk Enterprise:
  ```
  wget -O splunk-9.4.0-6b4ebe426ca6-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/9.4.0/linux/splunk-9.4.0-6b4ebe426ca6-linux-amd64.deb"
  sudo dpkg -i splunk-9.4.0-6b4ebe426ca6-linux-amd64.deb
  cd /opt/splunk/bin
  sudo ./splunk start
  ```
2. Make splunk accesible via internet (optional)
```
  nano /opt/splunk/etc/splunk-launch.conf
  SPLUNK_BINDIP=0.0.0.0 //add this
```
3. Add port 8000 in firewall
   ```
   ufw allow 8000/tcp
   ```
4. Access Splunk via the web interface:
  ```
  http://<Splunk_Server_IP>:8000
```
  ### Part 2: Configure Syslog Collection from the Second Ubuntu Server
 #### Step 6: Install Splunk Universal Forwarder on Syslog Server
  1. Download Splunk Universal Forwarder:
  ```
  wget -O splunkforwarder-9.4.0-6b4ebe426ca6-linux-amd64.deb "https://download.splunk.com/products/universalforwarder/releases/9.4.0/linux/splunkforwarder-9.4.0-6b4ebe426ca6-linux-amd64.deb"
  ```
  2. Install the Universal Forwarder:
  ```
  sudo dpkg -i splunkforwarder-9.4.0-6b4ebe426ca6-linux-amd64.deb
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

   ### Part 3: Install Splunk Universal Forwarder on a Windows Machine
   #### Step 1: Download and Install Splunk Universal Forwarder
   Download the Splunk Universal Forwarder for Windows:
   Splunk UF for Windows
   Run the installer and follow the setup wizard:
   Accept the license agreement.
   Choose the installation directory.
   Provide the Splunk server IP and receiving port (e.g., 9997).
   #### Step 2: Add Monitored Inputs on Windows
   Launch the Splunk Universal Forwarder Configuration utility.
   Add directories or files to monitor (e.g., Event Logs, system logs).
   #### Step 3: Verify Forwarding to Splunk
   - On the Splunk server, verify that the Windows machine is forwarding logs:
 ```
   sudo /opt/splunk/bin/splunk list forward-server
  ```
   - Search for Windows logs in Splunk's Search & Reporting app:
   ```
   index=windows_logs
   ```

  ## Conclusion
  This setup allows you to analyze syslogs from a second Ubuntu server using Splunk's powerful interface. By following this guide, you now have a functional home lab for practicing security investigations, monitoring, and analysis. Experiment with different log sources and enrich your skills further!

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
