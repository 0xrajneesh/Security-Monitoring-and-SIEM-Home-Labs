# Home-Lab Setup: Grafana and Prometheus for Security Monitoring of Ubuntu Server 22.04 (Including Syslog Integration)

## Objective
To create a home lab using Grafana and Prometheus for monitoring the security of an Ubuntu Server 22.04. Additionally, configure the Ubuntu Server to send system logs (syslog) for centralized monitoring.

---

## Requirements
- **Virtualization Platform**:
  - [VirtualBox](https://www.virtualbox.org/)
  - [VMware Workstation Pro](https://www.vmware.com/products/workstation-pro.html)
  - [Proxmox VE](https://www.proxmox.com/en/proxmox-ve)
- **Ubuntu Server 22.04 ISO**: [Download here](https://releases.ubuntu.com/22.04/)
- **Grafana**: [Official website](https://grafana.com/)
- **Prometheus**: [Official website](https://prometheus.io/)
- **Promtail (for syslog forwarding)**: [Loki Documentation](https://grafana.com/docs/loki/latest/clients/promtail/)
- **Loki**: [Official website](https://grafana.com/oss/loki/)
- Stable internet connection.

---

## Step-by-Step Guide

### Step 1: Set Up the Virtual Machine
1. Download the Ubuntu Server 22.04 ISO.
2. Choose a virtualization platform (VirtualBox, VMware, or Proxmox).
3. Create a new virtual machine:
   - Assign at least 2 CPU cores and 4 GB RAM.
   - Allocate 20 GB disk space.
   - Mount the Ubuntu ISO and install Ubuntu Server 22.04.
4. During the installation, set up a hostname, user account, and static IP.

---

### Step 2: Install Prometheus
1. Follow the steps outlined in the earlier section to install Prometheus on the server.
2. Configure Prometheus as described, ensuring the configuration supports adding new targets.

---

### Step 3: Install Grafana
1. Follow the steps outlined in the earlier section to install and set up Grafana.
2. Ensure Grafana is accessible at `http://<server-ip>:3000`.

---

### Step 4: Install Loki and Promtail for Syslog Collection
#### Install Loki
1. Download and install Loki:
   ```bash
   wget https://github.com/grafana/loki/releases/download/v2.10.0/loki-linux-amd64.zip
   unzip loki-linux-amd64.zip
   sudo mv loki-linux-amd64 /usr/local/bin/loki
  ```
2. Create a Loki configuration file:
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
limits_config:
  enforce_metric_name: false
chunk_store_config:
  max_look_back_period: 0s
table_manager:
  retention_deletes_enabled: false
  retention_period: 0s
  ```
3. Start Loki:
```
loki -config.file=/etc/loki-config.yml &
```
- Install Promtail
Download and install Promtail:
```
wget https://github.com/grafana/loki/releases/download/v2.10.0/promtail-linux-amd64.zip
unzip promtail-linux-amd64.zip
sudo mv promtail-linux-amd64 /usr/local/bin/promtail
```
- Create a Promtail configuration file:
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
- Start Promtail:
```
promtail -config.file=/etc/promtail-config.yml &
```
### Step 5: Integrate Syslog Data with Grafana
1. In Grafana, go to Configuration > Data Sources.
2. Add Loki as a data source:
- URL: http://<server-ip>:3100
- Click Save & Test.
3. Create a new dashboard in Grafana:
- Add panels with Loki queries to visualize logs from /var/log/syslog.

## Conclusion
You have now configured a home lab with Grafana, Prometheus, Loki, and Promtail to monitor both metrics and system logs of your Ubuntu Server 22.04. This comprehensive setup enables you to gain hands-on experience with real-time security monitoring and log analysis. Experiment with dashboards and queries to enhance your skills further!





