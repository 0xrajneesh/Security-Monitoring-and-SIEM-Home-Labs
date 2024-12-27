# Setting Up a Home-Lab Using Graylog

## Objective  
To create a home-lab using Graylog on Ubuntu Server 22.04 for security monitoring. This lab will enable you to monitor logs and security events from another Ubuntu Server, simulating a real-world Security Information and Event Management (SIEM) setup.

---

## Requirements  
- **Virtualization Tool**: Choose one of the following:  
  - [VirtualBox](https://www.virtualbox.org/) (Free and open-source)  
  - [VMware Workstation Pro](https://www.vmware.com/products/workstation-pro.html) (Paid, with a free trial available)  
  - [Proxmox VE](https://www.proxmox.com/en/proxmox-ve) (Open-source virtualization platform)  

- **Ubuntu Server 22.04 ISO**: [Download here](https://ubuntu.com/download/server)  
- Minimum hardware requirements for each VM:
  - 4 GB RAM
  - 20 GB disk space
  - 2 vCPUs

- **Graylog**: Open-source SIEM tool. [Download here](https://www.graylog.org/products/open-source)  

---

## Step-by-Step Guide  

### **Part 1. Set Up the Virtual Environment**  
1. Install your chosen virtualization tool.  
2. Create two VMs using the Ubuntu Server 22.04 ISO:  
   - **Graylog Server**: This will host Graylog, MongoDB, and Elasticsearch.  
   - **Client Server**: This will send logs to Graylog for monitoring.  

---

### **Part 2. Configure the Graylog Server**  
#### **Step 1: Update the System**  
Run the following commands to ensure the system is updated:  
```bash
sudo apt update && sudo apt upgrade -y
```
#### Step 2: Install Java
Java is required for Elasticsearch:

```
sudo apt install openjdk-11-jre -y
```
#### Step 3: Install MongoDB
Install MongoDB as the database for Graylog:

```
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
sudo apt update
sudo apt install -y mongodb-org
sudo systemctl start mongod
sudo systemctl enable mongod
```
#### Step 4: Install Elasticsearch
Install Elasticsearch to index and search logs:

```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update
sudo apt install elasticsearch -y
sudo systemctl start elasticsearch
sudo systemctl enable elasticsearch
```
#### Step 5: Install Graylog
Install Graylog as the log management platform:

```
wget https://packages.graylog2.org/repo/packages/graylog-4.3-repository_latest.deb
sudo dpkg -i graylog-4.3-repository_latest.deb
sudo apt update
sudo apt install graylog-server -y
```
#### Step 6: Configure Graylog
- Edit the Graylog configuration file:

```
sudo nano /etc/graylog/server/server.conf
```
- Set `password_secret` and `root_password_sha2`:
Generate the values using:
```
pwgen -N 1 -s 96
echo -n yourpassword | sha256sum
```
- Add the values to the configuration file.
Restart Graylog:

```
sudo systemctl restart graylog-server
```
### **Part 3. Configure the Client Server**
####Step 1: Update the System
Run the following commands:

```
sudo apt update && sudo apt upgrade -y
```
#### Step 2: Install Rsyslog
Install Rsyslog for log forwarding:

```
sudo apt install rsyslog -y
```
#### Step 3: Configure Log Forwarding to Graylog
- Edit the Rsyslog configuration file:

```
sudo nano /etc/rsyslog.conf
```
- Add the following line to forward logs to the Graylog server:
```
*.* @<GRAYLOG_SERVER_IP>:514
```
- Restart Rsyslog:

```
sudo systemctl restart rsyslog
```
### **4. Access the Graylog Web Interface**
1. Open a browser and navigate to http://<GRAYLOG_SERVER_IP>:9000.
2. Log in with the credentials configured earlier.
3. Add an input source (e.g., Syslog UDP) in the Graylog interface to start receiving logs.

## Conclusion
This Graylog-based home-lab provides a practical environment to monitor and analyze security logs from an Ubuntu client server. By following this guide, you can enhance your skills in security monitoring and log management, laying a foundation for advanced SIEM and SOC tasks.

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
