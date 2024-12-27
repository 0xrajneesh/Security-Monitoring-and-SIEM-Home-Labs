# Home-Lab Setup: Grafana and Prometheus for Security Monitoring

## Objective
To create a home lab using Grafana and Prometheus for monitoring the security of an Ubuntu Server 22.04 and a Windows machine. Additionally, configure both systems to send logs for centralized monitoring.

---

## Home-Lab Setup
This lab is designed can be designed on top of Vmware workstation, Virtualbox or Proxmox.
![Blue and Gold Elegant Minimalist Jewelry Promotions Business Floor Decal (150 x 120 mm) (2)](https://github.com/user-attachments/assets/f93c0cda-6551-44ce-b474-03bb955e5855)

---

## Requirements
- **Virtualization Platform**:
  - [VirtualBox](https://www.virtualbox.org/)
  - [VMware Workstation Pro](https://www.vmware.com/products/workstation-pro.html)
  - [Proxmox VE](https://www.proxmox.com/en/proxmox-ve)
- **Ubuntu Server 22.04 ISO**: [Download here](https://releases.ubuntu.com/22.04/)
- **Windows ISO**: [Download here](https://www.microsoft.com/en-us/software-download/windows10)
- **Grafana**: [Official website](https://grafana.com/)
- **Prometheus**: [Official website](https://prometheus.io/)
- **Promtail (for syslog forwarding)**: [Loki Documentation](https://grafana.com/docs/loki/latest/clients/promtail/)
- **Loki**: [Official website](https://grafana.com/oss/loki/)
- **Winlogbeat**: [Official website](https://www.elastic.co/beats/winlogbeat)
- Stable internet connection.

---

## Step-by-Step Guide

### Part 1: Grafana and Prometheus Setup
**Objective**: Set up Grafana and Prometheus on an Ubuntu server to monitor system metrics and provide a centralized visualization platform.

#### Step 1: Set Up the Virtual Machine
1. Download the Ubuntu Server 22.04 ISO.
2. Choose a virtualization platform (VirtualBox, VMware, or Proxmox).
3. Create a new virtual machine:
   - Assign at least 2 CPU cores and 4 GB RAM.
   - Allocate 20 GB disk space.
   - Mount the Ubuntu ISO and install Ubuntu Server 22.04.
4. During the installation, set up a hostname, user account, and static IP.

#### Step 2: Install Prometheus
1. Update the system packages:
   ```bash
   sudo apt update && sudo apt upgrade -y
  ``
2. Download and install Prometheus:
```
wget https://github.com/prometheus/prometheus/releases/download/v2.44.0/prometheus-2.44.0.linux-amd64.tar.gz
tar -xvf prometheus-2.44.0.linux-amd64.tar.gz
sudo mv prometheus-2.44.0.linux-amd64 /usr/local/prometheus
```
3. Configure Prometheus:
```
sudo nano /usr/local/prometheus/prometheus.yml
```
- Add the following:
```
global:
  scrape_interval: 15s
scrape_configs:
  - job_name: 'ubuntu'
    static_configs:
      - targets: ['localhost:9090']
```
4. Start Prometheus:
```
cd /usr/local/prometheus
./prometheus --config.file=prometheus.yml &
```
#### Step 3: Install Grafana
1. Download and install Grafana:
```
wget https://dl.grafana.com/oss/release/grafana-9.6.4_amd64.deb
sudo dpkg -i grafana-9.6.4_amd64.deb
```
2. Start Grafana:
```
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
```
3. Access Grafana:
- Go to http://<server-ip>:3000.
- Login with default credentials (admin/admin) and set a new password.

### Part 2: Setting Up Ubuntu Server
Objective: Configure Ubuntu Server to forward system logs to Grafana using Loki and Promtail.

#### Step 1: Install Loki and Promtail
1. Install Loki:
```
wget https://github.com/grafana/loki/releases/download/v2.10.0/loki-linux-amd64.zip
unzip loki-linux-amd64.zip
sudo mv loki-linux-amd64 /usr/local/bin/loki
```
2. Configure Loki:
```
sudo nano /etc/loki-config.yml
```
- Add the following:
```
auth_enabled: false
server:
  http_listen_port: 3100
ingester:
  lifecycler:
    ring:
      kvstore:
        store: inmemory
schema_config:
  configs:
    - from: 2023-01-01
      store: boltdb-shipper
      object_store: filesystem
      schema: v11
      index:
        prefix: index_
        period: 24h
storage_config:
  boltdb_shipper:
    active_index_directory: /tmp/loki/boltdb-shipper-active
    shared_store: filesystem
    cache_location: /tmp/loki/boltdb-shipper-cache
  filesystem:
    directory: /tmp/loki/chunks
```
3. Start Loki:
```
loki -config.file=/etc/loki-config.yml &
```
4. Install Promtail:
```
wget https://github.com/grafana/loki/releases/download/v2.10.0/promtail-linux-amd64.zip
unzip promtail-linux-amd64.zip
sudo mv promtail-linux-amd64 /usr/local/bin/promtail
```
5. Configure Promtail:
```
sudo nano /etc/promtail-config.yml
```
- Add the following:
```
server:
  http_listen_port: 9080
clients:
  - url: http://localhost:3100/loki/api/v1/push
positions:
  filename: /tmp/positions.yaml
scrape_configs:
  - job_name: 'syslog'
    static_configs:
      - targets:
          - localhost
        labels:
          job: 'syslog'
          __path__: /var/log/syslog
```
6. Start Promtail:
```
promtail -config.file=/etc/promtail-config.yml &
```
### Part 3: Setting Up Windows Machine
Objective: Configure a Windows machine to send event logs to Grafana using Loki and Winlogbeat.

#### Step 1: Install Winlogbeat
1. Download Winlogbeat.
2. Extract the files and navigate to the directory.
#### Step 2: Configure Winlogbeat
1. Open winlogbeat.yml in a text editor.
2. Configure the output to Loki:
```
output:
  loki:
    hosts: ["http://<server-ip>:3100"]
```
3. Add log paths for monitoring:
```
winlogbeat.event_logs:
  - name: Application
  - name: Security
  - name: System
```
#### Step 3: Run Winlogbeat
1. Install Winlogbeat as a service:
```
.\install-service-winlogbeat.ps1
```
2. Start the service:
```
Start-Service winlogbeat
```
#### Step 4: Verify Logs in Grafana
1. Add Loki as a data source in Grafana.
2. Create dashboards to visualize logs from Ubuntu and Windows systems.

## Conclusion
This guide demonstrates setting up a home lab with Grafana, Prometheus, Loki, Promtail, and Winlogbeat to monitor Ubuntu and Windows machines. By centralizing logs and metrics, you can effectively analyze and enhance your system's security.

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
