# Active Directory Home Lab
# Objective

This project simulates a small corporate network to gain hands-on experience with real-world System Administration, Active Directory management, Group Policy, and Security Monitoring. The lab environment consists of a `Windows Server 2022` domain controller, a `Windows 10 client` joined to the domain, and an `Ubuntu Server` running `Splunk` as a SIEM, with `Kali Linux` acting as the attacker machine to generate realistic security telemetry. This repository documents the full build, from installing and configuring each system to deploying Sysmon for endpoint logging, forwarding events to Splunk, and analyzing the results.

# Skills Learned
 ► Using and configuring virtual machines using VMware software
 
 ► Deployed a Windows Server 2022 domain controller (AD DS, DNS, Group Policy)

 ► Configured and deployed Ubuntu server for use with Splunk
 
 ► Managed users, groups, and OUs via GUI and PowerShell
 
 ► Configured static IPs and verified connectivity across VMs
 
 ► Deployed Sysmon and Splunk Forwarders for log collection
 
 ► Used Splunk dashboards to detect suspicious activity
 
 ► Simulated brute-force attack using Kali Linux
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
- [Windows Server 2022 Setup & AD Deployment](01-Window-Server-Setup.md)
- [Windows 10 Client Setup & Domain Join](02-Windows-10-Client-Setup-&-Domain-Join.md)
- [Blocking Social Media Using Group Policy](03-Blocking-Social-Media-Using-Group-Policy.md)
- [Splunk Server & Sysmon Deployment](04-Splunk-Server-&-Sysmon-Deployment.md)
- [Attack Simulation with Kali Linux](05-Attack-Simulation-With-Kali-Linux.md)

---
 [Back to main page](https://github.com/XDTX75694)
