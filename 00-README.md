# Active Directory Home Lab
# Objective

This project simulates a small corporate network to practice real-world system administration, Active Directory management, and security monitoring. The lab consists of a Windows Server 2022 domain controller, a Windows 10 client joined to the domain, an Ubuntu server, and Splunk. Kali Linux attacker machine is used to generate test telemetry. This lab displays the installation and configuration of Windows Server, Windows 10, Ubuntu Server, Splunk, Sysmon, and Kali Linux. 

# Skills Learned
 ► Using and configuring virtual machines using VMware software
 
 ► Deployed a Windows Server 2022 domain controller (AD DS, DNS, Group Policy)

 ► Configured and deployed Ubuntu server for use with Splunk
 
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
## Network Diagram

 
<img width="600" height="775" alt="image" src="https://github.com/user-attachments/assets/7e3b95cb-77a6-4dbb-a500-a4842c790499" />



*Router/switch layout connecting the Splunk server, Active Directory server, client machine, and attacker machine on the 192.168.10.0/24 network. The dashed line represents the Splunk Universal Forwarder log path from the client to the Splunk server.*
 
---
## Table of Contents
- [Windows Server 2022 Setup & AD Deplyment](01WindowServerSetup.md)
- [Windows 10 Client Setup & Domain Join](02-Windows-10-Client-Setup-&-Domain-Join.md)
- [Splunk & Sysmon Deployment](#splunk-siem--sysmon-deployment)
- [Blocking Social Media Using Group Policy](#blocking-social-media-using-group-policy)
- [Attack Simulation with Kali Linux](#attack-simulation-with-kali-linux)

---
