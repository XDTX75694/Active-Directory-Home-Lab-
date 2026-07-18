# Active Directory Home Lab
# Objective

This project simulates a small corporate network to practice real-world system administration, Active Directory management, and security monitoring. The lab consists of a Windows Server 2022 domain controller, a Windows 10 client joined to the domain, an Ubuntu server, and Splunk. Kali Linux attacker machine used to generate test telemetry. This lab displays the installation and configuration of Windows Server, Windows 10, Ubuntu Server, Splunk, Sysmon, and Kali Linux. 

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

Selecting the version to use with Windows Server 2022. The highlighted version was chosen because it provides a graphical user interface (GUI) instead of using a command-line interface.   

---
<img width="1203" height="820" alt="Admin" src="https://github.com/user-attachments/assets/11847fde-1e74-40ef-a852-20ee8e3bd9f1" />

Administrator username and password are being created. 

---
<img width="1099" height="819" alt="Complete install" src="https://github.com/user-attachments/assets/4dd9642a-ab1d-4b32-964a-f990f7fdfbbb" />

Installation of the Windows Server is complete.

---
<img width="1014" height="982" alt="Changing IP" src="https://github.com/user-attachments/assets/66555a7e-d571-4066-9e0e-7bec9cd31338" />
To change the adapter settings and make a static IP, you go to the network icon, go to the taking you to the network status page, and then click Change adapter options under the Advanced network settings. Then right-click Ethernet0 and click on Properties.  

---
<img width="500" height="500" alt="IP Version 4 " src="https://github.com/user-attachments/assets/0c63eccf-a362-4b00-8ffc-873609cde5a0" />

After clicking Properties and double-clicking on Internet Protocol Version 4 (IPv4) to configure a static IP for the server. A static IP was created to identify the workstation more easily. (The static IP configuration is shown below)

---

<img width="600" height="550" alt="Static IP" src="https://github.com/user-attachments/assets/dd092605-1868-475f-8529-b0067d2ff99a" />

[Reference the network diagram above](#network-diagram).

---

<img width="970" height="323" alt="Checking IP change" src="https://github.com/user-attachments/assets/5480ebb1-9ea3-4dc9-b1ea-2918d0ae0c9f" />

After setting up the static IP address, we need to verify the change by using Command Prompt, which is a command-line interface. To verify the new IP, I used the ipconfig command to show the current IP, and the change was successful.

---
<img width="923" height="654" alt="Changing NAT network to have slash 24" src="https://github.com/user-attachments/assets/6aa917f2-007a-4ee7-aee4-025c5170da17" />
<img width="937" height="788" alt="Win server on network" src="https://github.com/user-attachments/assets/b72b68bd-8b38-4809-ad45-a3f4ac65db50" />


For all the virtual machines to be reachable on the same network, I created a new Network Address Translation (NAT) for all the operating systems to be able to communicate with each other, as seen in the first screenshot.

The second screenshot just shows the Virtual Machine being connected to the new NAT network. 

---

<img width="1001" height="887" alt="Name change" src="https://github.com/user-attachments/assets/cb923e51-5834-4b36-8fdd-0e35d28e0371" />

To make it easier to identify this server on the network, I changed the computer's name to ACDC01. 

---

<img width="1014" height="760" alt="Adding users and roles " src="https://github.com/user-attachments/assets/411ab064-0703-42b7-8c9a-bc7ac74622ef" />

To use Active Directory, it needs to be installed by clicking on Manage on the top right and then clicking Add Roles and Features bring you into the wizard as shown in the screenshots below. 

---

<img width="997" height="706" alt="The settings " src="https://github.com/user-attachments/assets/7c4a6334-6a34-4678-9f8b-c45788580cce" />

---

<img width="688" height="345" alt="AD Domin services " src="https://github.com/user-attachments/assets/15c20627-d16d-4f4c-9ee2-446cc3c5e06e" />

---
<img width="944" height="633" alt="Installing features " src="https://github.com/user-attachments/assets/b81461e7-6469-4470-8658-a73e8834b18b" />

---
<img width="453" height="261" alt="Promoting server to a domain controler" src="https://github.com/user-attachments/assets/aac7c92f-86d2-4338-acfb-9c6376c2bb88" />

After the install is complete for Active Directory, the server needs to be promoted to a Domain Controller so it turns into the core authority of the network.  

---
<img width="772" height="568" alt="Deployment config" src="https://github.com/user-attachments/assets/2fe239d9-000b-44f9-a8ac-18a3226b85a1" />

Configuring the root domain name (dale.local) when promoting the server to Active Directory Domain Services. The root domain is the top-level namespace for the entire Active Directory forest. Every computer, user, and group created afterward belongs to this domain, and it's what devices use to locate the domain controller for logins, Group Policy, and DNS resolution. Using `.local` keeps the domain isolated from real public DNS. 

---
<img width="914" height="631" alt="Installing prerecs" src="https://github.com/user-attachments/assets/1ec25455-9734-4ee4-adc9-75aa02195467" />

Installation of all the configurations.  

---

<img width="1060" height="925" alt="AD settings applyed" src="https://github.com/user-attachments/assets/31011ba4-662d-4af3-b4bd-3d09bd04f695" />

After the configurations are installed, it will prompt you to restart to apply the changes. As shown in this photo, the configurations were installed successfully, and the local domain of DALE is now installed.  

---
<img width="925" height="650" alt="Creating users and groups " src="https://github.com/user-attachments/assets/ab6eeeae-e134-4ad3-b61a-4249d69d4c54" />

Now that Active Directory has been installed, we want to create new users and Organizational Units. To add these, go to Tools on the top right and click on Active Directory Users and Computers. 

---
<img width="663" height="536" alt="image" src="https://github.com/user-attachments/assets/0aef0b4a-f6a5-4cce-8e30-59ed40595181" />

---
<img width="770" height="528" alt="Creating an OU in AD" src="https://github.com/user-attachments/assets/eaa5fcf5-9eaa-43c6-acae-e87cdf682ae2" />

---

<img width="500" height="500" alt="Created IT OU" src="https://github.com/user-attachments/assets/bd4fcb2c-aabb-4e4a-b301-90702108fb28" />


I created two OUs: one for IT and one for HR. James White of IT and John Smith of HR. The OU and user for HR will be created using PowerShell, and the IT OU will be created in Active Directory. To create a new Organizational Unit (OU), right-click on `dale.local`, go down to New, and click on Organizational Unit. After creating the new OU, it will appear. 

---
<img width="988" height="455" alt="image" src="https://github.com/user-attachments/assets/0eb8e23e-763b-423d-96fc-146c37e4824c" />

To add a new user to the OU, right-click on IT, go to New, and select User.   
---

<img width="833" height="538" alt="User created inside the new OU" src="https://github.com/user-attachments/assets/a627ef00-0ecc-4a31-8767-618cf3a9092e" />

---

<img width="768" height="510" alt="New user" src="https://github.com/user-attachments/assets/403a4f84-1d71-4df2-aaac-653f397a3f18" />

The user James White has been added to the IT Organizational Unit.

---
<img width="1036" height="148" alt="OU created via pwrSSH" src="https://github.com/user-attachments/assets/ebc2402b-5c88-47e5-9ce8-71474fe21a34" />

OU created via PowerShell.

---

<img width="798" height="170" alt="User created in the OU via PWRSSH" src="https://github.com/user-attachments/assets/090750b2-19e6-4449-8fd3-7a3c0e78be8d" />

User created via PowerShell. 

---

<img width="1012" height="176" alt="User created and verified" src="https://github.com/user-attachments/assets/525849cf-de8b-47ad-b6d1-12f00b6c3504" />

Verifying the user and OU were created via PowerShell. 








