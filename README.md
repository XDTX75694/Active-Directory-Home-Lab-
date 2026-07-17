# Active Directory Home Lab
# Objective

This project simulates a small corporate network to practice real-world system administration, Active Directory management, and security monitoring. The lab consists of a Windows Server 2022 domain controller, a Windows 10 client joined to the domain, an Ubuntu server, and Splunk. Kali Linux attacker machine used to generate test telemetry.

# Skills Learned
 ► Using and configuring virtual machines using VMware software
 
 ► Deployed a Windows Server 2022 domain controller (AD DS, DNS, Group Policy)

 ► Configured and deployed Ubuntu server for the use of Splunk
 
 ► Managed users, groups, and OUs via GUI and PowerShell
 
 ► Configured static IPs and verified connectivity across VMs
 
 ► Deployed Sysmon and Splunk Forwarders for log collection
 
 ► Built Splunk dashboards to detect suspicious activity
 
 ► Simulated brute-force attack using Kali Linux
 
 ► Hardened environment with Firewall rules and Group Policy
 ##
 
## Tools Used
`VMware`
`Windows Server 2022`
`Windows 10` 
`Ubuntu Linux`
`Kali Linux`
`Active Directory` 
`Splunk`
`Sysmon` 
`PowerShell`
`Scripting`

---
# Network Architecture

 
<img width="600" height="775" alt="image" src="https://github.com/user-attachments/assets/7e3b95cb-77a6-4dbb-a500-a4842c790499" />



*Router/switch layout connecting the Splunk server, Active Directory server, client machine, and attacker machine on the 192.168.10.0/24 network. The dashed line represents the Splunk Universal Forwarder log path from the client to the Splunk server.*
 
---
## Table of Contents
- [Windows Server 2022 Setup](#windows-server-2022-setup)
- [Windows 10 Client Setup & Domain Join](#windows-10-client--domain-join)
- [Splunk & Sysmon Deployment](#splunk-siem--sysmon-deployment)
- [Active Directory Deployment](#active-directory-structure)
- [Group Policies Applied](#group-policies-applied)
- [Attack Simulation with Kali Linux](#attack-simulation-with-kali-linux)
- [Blocking Social Media Using Group Policy](#blocking-social-media-using-group-policy)
---
## Windows Server 2022 Setup

<img width="1919" height="1024" alt="server 22 ISO" src="https://github.com/user-attachments/assets/1ccf8153-2217-48d9-9566-33ff4d38dfbb" />
Using the setup wizard to install Windows Server 2022 using an ISO file. 

---


<img width="1919" height="1000" alt="Server specs " src="https://github.com/user-attachments/assets/c760c1d4-5432-4874-8f97-dd21be9e7006" />
Specs for the Windows Server include 50GB of hard drive space, 4096 MB or 4 GB of memory, and 4 CPU cores.  

---


<img width="908" height="668" alt="Server version " src="https://github.com/user-attachments/assets/35aae4af-955d-4c94-abf5-d9868f55534f" />

Selecting the version to use with Windows Server 2022. The highlighted version was chosen because it provides a graphical user interface (GUI).   

---
<img width="1203" height="820" alt="Admin" src="https://github.com/user-attachments/assets/11847fde-1e74-40ef-a852-20ee8e3bd9f1" />

Administrator username and password are being created. 

---
<img width="1099" height="819" alt="Complete install" src="https://github.com/user-attachments/assets/4dd9642a-ab1d-4b32-964a-f990f7fdfbbb" />

Installation of the server completed.

---
<img width="1014" height="982" alt="Changing IP" src="https://github.com/user-attachments/assets/66555a7e-d571-4066-9e0e-7bec9cd31338" />
To change the adapter settings and make a static IP, you go to the network icon, go to the taking you to the network status page, and then click Change adapter options under the Advanced network settings. Then right-click Ethernet0 and click on Properties.  

---
<img width="500" height="500" alt="IP Version 4 " src="https://github.com/user-attachments/assets/0c63eccf-a362-4b00-8ffc-873609cde5a0" />

After clicking Properties and double-clicking on Internet Protocol Version 4 (IPv4) to configure a static IP for the server. (The static IP configuration is shown below.)

---

<img width="600" height="550" alt="Static IP" src="https://github.com/user-attachments/assets/dd092605-1868-475f-8529-b0067d2ff99a" />







