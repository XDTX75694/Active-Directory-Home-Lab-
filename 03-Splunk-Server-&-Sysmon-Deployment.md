# Splunk Server & Sysmon Deployment
---
## **<ins>Section Objective:</ins>**

This section covers deploying Splunk as the lab's SIEM (Security Information and Event Management) system on an Ubuntu server, and installing Sysmon on the Active Directory server and Windows 10 client. Splunk collects and centralizes logs from across the network, while Sysmon provides detailed endpoint telemetry, such as process creation and network connections, which is forwarded to Splunk. Together, these tools make it possible to search, analyze, and detect suspicious activity. 

---
<img width="550" height="500" alt="Making sever more beefy" src="https://github.com/user-attachments/assets/544d667e-edfa-4f43-837d-2c2378544085" />

The Splunk Server is a bit beefier than the rest of the VMs because it will be ingesting data and running searches. The specs of the server are 95GB of storage, 8 GB of RAM, and 4 CPU cores.  

---

<img width="1283" height="410" alt="Account setup" src="https://github.com/user-attachments/assets/3f335b50-cbf3-4003-9150-eb4ea1cbb868" />

The profile configuration screen looks different than Windows, as some Linux distributions and versions are command-line interface only.

---
<img width="600" height="250" alt="Installing most UTD version " src="https://github.com/user-attachments/assets/96210262-1ff2-4e2b-b5b9-cee8da24c77c" />

For the server to update to the current version and be up to date, the command that is used is `sudo apt-get update && sudo apt-get upgrade -y` (Explain what the command does)

---
<img width="887" height="619" alt="Sever is updated " src="https://github.com/user-attachments/assets/42e51c78-9d1e-4e17-81cb-5406f3e2a05e" />

After entering that command, it will give you this confirmation page, and the server will restart all services. It is always a good practice to restart the server for changes to apply  

---
<img width="657" height="170" alt="installing network with static ip" src="https://github.com/user-attachments/assets/25c5a192-ac8a-4e93-8588-75d3e83ecd30" />

Static network config command  

---

<img width="863" height="671" alt="Setting up static ip" src="https://github.com/user-attachments/assets/2a7f2878-1291-4ac3-a210-6d521a056e8a" />

Configuring static IP using YAML. 

---

<img width="956" height="661" alt="Settings applyed" src="https://github.com/user-attachments/assets/bbffb83e-d041-4ef1-b970-e67a2453b932" />

Showing config was applied

---

<img width="830" height="263" alt="IP has changed" src="https://github.com/user-attachments/assets/81e5e781-ba42-4baf-a0f3-ae7ad06f180f" />

Showing IP has changed to a static IP. 



















