# **Project: Setting Up Splunk Enterprise for Security Monitoring**

---

## **Objective**
Set up a Splunk Enterprise instance on an Ubuntu server, ingest sample data, and create a basic dashboard for monitoring.

---

## **Steps to Complete the Project**

### **Step 1: Prepare Your Environment**
1. **Update the server packages**:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
### Step 2: Download and Install Splunk
   1. Create a Splunk user (optional but recommended):
   ```
   sudo useradd -m splunk
   sudo passwd splunk
   ```
   sudo usermod -aG sudo splunk
   2. Switch to the Splunk user:
   bash
   Copy code
   su - splunk
   - Download the `Splunk` Enterprise installer:
   bash
   Copy code
   wget -O splunk.tgz "https://download.splunk.com/products/splunk/releases/9.0.3/linux/splunk-9.0.3-linux-64bit.tgz"
   - Extract the installer:
   bash
   Copy code
   tar -xvf splunk.tgz
   Move Splunk to the appropriate directory:
   bash
   Copy code
   sudo mv splunk /opt/
Step 3: Start and Configure Splunk
Navigate to the Splunk directory:
bash
Copy code
cd /opt/splunk/bin
Start Splunk and accept the license:
bash
Copy code
sudo ./splunk start --accept-license
Set an admin username and password: Follow the prompts to set the credentials.
Step 4: Configure Splunk to Start at Boot
Enable Splunk as a service:
bash
Copy code
sudo ./splunk enable boot-start
Step 5: Access Splunk Web Interface
Open a web browser and navigate to:
arduino
Copy code
http://<your-server-ip>:8000
Log in using the admin credentials you set up.
Step 6: Ingest Sample Data
Download a sample dataset:
bash
Copy code
wget -O sample_logs.tar.gz "https://github.com/datasets/logs/raw/main/sample_logs.tar.gz"
Extract the logs:
bash
Copy code
tar -xvf sample_logs.tar.gz
Upload logs to Splunk:
Go to Settings > Add Data > Upload.
Select your log files from the extracted folder.
Follow the prompts to complete the ingestion.
Step 7: Search and Analyze Logs
Search logs in Splunk:
Navigate to Search & Reporting.
Use a basic query:
spl
Copy code
index=_internal | stats count by sourcetype
Explore different queries to analyze your data.
Step 8: Create a Basic Dashboard
Navigate to Dashboard in Splunk.
Click on Create New Dashboard.
Add a panel with a query:
spl
Copy code
index=_internal | timechart count by sourcetype
Save and view your dashboard.
Commands Summary
Server Update:
bash
Copy code
sudo apt update && sudo apt upgrade -y
Download and Extract Splunk:
bash
Copy code
wget -O splunk.tgz "https://download.splunk.com/products/splunk/releases/9.0.3/linux/splunk-9.0.3-linux-64bit.tgz"
tar -xvf splunk.tgz
sudo mv splunk /opt/
Start Splunk:
bash
Copy code
sudo ./splunk start --accept-license
Enable Boot Start:
bash
Copy code
sudo ./splunk enable boot-start
Outcome
By completing this project, you will have a functional Splunk instance capable of ingesting and
