 # Windows Server 2022 Setup
---
## **<ins>Section Objective:</ins>**
This section covers building the Windows Server 2022 virtual machine from scratch and turning it into a domain controller — the central server that manages user accounts, computers, and security policies for the entire network. 

---

<img width="1919" height="1024" alt="server 22 ISO" src="https://github.com/user-attachments/assets/1ccf8153-2217-48d9-9566-33ff4d38dfbb" />
Using the setup wizard for installing Windows Server 2022 from an ISO file, the disc image is used to set up the operating system. 

---


<img width="1919" height="1000" alt="Server specs " src="https://github.com/user-attachments/assets/c760c1d4-5432-4874-8f97-dd21be9e7006" />
The virtual machine was given 50GB of storage, 4GB of memory, and 4 CPU cores — enough resources to run a domain controller comfortably in a lab environment. 

---


<img width="908" height="668" alt="Server version " src="https://github.com/user-attachments/assets/35aae4af-955d-4c94-abf5-d9868f55534f" />

Selecting the version of Windows Server 2022 that includes a graphical interface, rather than the command-line-only version, to make the server easier to manage.

---
<img width="1203" height="820" alt="Admin" src="https://github.com/user-attachments/assets/11847fde-1e74-40ef-a852-20ee8e3bd9f1" />

Setting the administrator username and password used to log into and manage the server.

---
<img width="1099" height="819" alt="Complete install" src="https://github.com/user-attachments/assets/4dd9642a-ab1d-4b32-964a-f990f7fdfbbb" />

The server installation finished successfully and is ready to be configured.

---
<img width="1014" height="982" alt="Changing IP" src="https://github.com/user-attachments/assets/66555a7e-d571-4066-9e0e-7bec9cd31338" />
The server needs a static IP address so it's always reachable at the same location on the network — if it changed every time it restarted, other computers wouldn't be able to consistently find it. To set this up, open the network adapter settings and switch the server from an automatically assigned IP address to a fixed (static) one.

---
<img width="500" height="500" alt="IP Version 4 " src="https://github.com/user-attachments/assets/0c63eccf-a362-4b00-8ffc-873609cde5a0" />

After clicking Properties and double-clicking on Internet Protocol Version 4 (IPv4). Making the IP static is important since other computers need to consistently connect to it. (The static IP configuration is shown below)

---

<img width="600" height="550" alt="Static IP" src="https://github.com/user-attachments/assets/dd092605-1868-475f-8529-b0067d2ff99a" />

[Reference the network diagram above](#network-diagram) for how this IP fits into the overall lab layout.

---

<img width="970" height="323" alt="Checking IP change" src="https://github.com/user-attachments/assets/5480ebb1-9ea3-4dc9-b1ea-2918d0ae0c9f" />

After configuring the static IP, the next step is to verify the change actually took effect. This can be confirmed using Command Prompt and the ipconfig command, which displays the current IP address assigned to the machine. As shown in the screenshot, the static IP was applied successfully.

---
<img width="923" height="654" alt="Changing NAT network to have slash 24" src="https://github.com/user-attachments/assets/6aa917f2-007a-4ee7-aee4-025c5170da17" />
<img width="937" height="788" alt="Win server on network" src="https://github.com/user-attachments/assets/b72b68bd-8b38-4809-ad45-a3f4ac65db50" />

All the virtual machines in this lab needed to communicate with each other, so I created a shared NAT (Network Address Translation) network using a /24 subnet, giving each machine its own address within the same private network while still allowing internet access. A /24 subnet was used because it provides 254 usable addresses, and it's the standard size used for small networks like this one, similar to what you'd find on a typical home or small office network. 

The first screenshot shows this network being created, and the second shows the server being connected to it.

---

<img width="1001" height="887" alt="Name change" src="https://github.com/user-attachments/assets/cb923e51-5834-4b36-8fdd-0e35d28e0371" />

Renaming the server to ACDC01 (short for "Active Directory Domain Controller 01") to make it easy to identify the server on the network if needed.

---

<img width="1014" height="760" alt="Adding users and roles " src="https://github.com/user-attachments/assets/411ab064-0703-42b7-8c9a-bc7ac74622ef" />

To install Active Directory, I clicked "Manage" in the top right of Server Manager, then selected "Add Roles and Features." This opens the Add Roles and Features wizard, the tool used to install Active Directory Domain Services onto the server, as shown in the screenshots below.

<img width="997" height="706" alt="The settings " src="https://github.com/user-attachments/assets/7c4a6334-6a34-4678-9f8b-c45788580cce" />

---

<img width="688" height="345" alt="AD Domin services " src="https://github.com/user-attachments/assets/15c20627-d16d-4f4c-9ee2-446cc3c5e06e" />

---
<img width="944" height="633" alt="Installing features " src="https://github.com/user-attachments/assets/b81461e7-6469-4470-8658-a73e8834b18b" />

---
<img width="453" height="261" alt="Promoting server to a domain controler" src="https://github.com/user-attachments/assets/aac7c92f-86d2-4338-acfb-9c6376c2bb88" />

Once installation is complete, the server is promoted to a domain controller. This is important because this turns a regular server into the "brain" of the network and is responsible for verifying logins, enforcing security policies, and keeping track of every user and computer that's part of the domain.

---
<img width="772" height="568" alt="Deployment config" src="https://github.com/user-attachments/assets/2fe239d9-000b-44f9-a8ac-18a3226b85a1" />

Setting the root domain name (dale.local) when promoting the server to Active Directory, which is the top-level name for the entire network. Every user, computer, and group created afterward belongs under this domain, and it's what devices use to locate the domain controller for logins, security policies, and DNS. Using .local keeps the domain isolated from public DNS.

---
<img width="914" height="631" alt="Installing prerecs" src="https://github.com/user-attachments/assets/1ec25455-9734-4ee4-adc9-75aa02195467" />

Applying the Active Directory configuration settings.

---

<img width="1060" height="925" alt="AD settings applyed" src="https://github.com/user-attachments/assets/31011ba4-662d-4af3-b4bd-3d09bd04f695" />

After the configurations are installed, the server prompts for a restart to apply the changes. As shown in this photo, the restart completed successfully and the new domain, "dale.local," is now up and running.

---
<img width="925" height="650" alt="Creating users and groups " src="https://github.com/user-attachments/assets/ab6eeeae-e134-4ad3-b61a-4249d69d4c54" />

With Active Directory installed, the next step is creating users and Organizational Units (OUs) — folders used to group and organize accounts by department or team. To do this, go to Tools in the top right and select "Active Directory Users and Computers," the tool used to create and manage user accounts, groups, and organizational units shown below.

---
<img width="663" height="536" alt="image" src="https://github.com/user-attachments/assets/0aef0b4a-f6a5-4cce-8e30-59ed40595181" />

---
<img width="770" height="528" alt="Creating an OU in AD" src="https://github.com/user-attachments/assets/eaa5fcf5-9eaa-43c6-acae-e87cdf682ae2" />

---

<img width="500" height="500" alt="Created IT OU" src="https://github.com/user-attachments/assets/bd4fcb2c-aabb-4e4a-b301-90702108fb28" />

Two Organizational Units were created — one for IT and one for HR — to represent different departments, with James White added to IT and John Smith added to HR. The HR OU and user were created using PowerShell, while the IT OU was created through the graphical interface, to demonstrate both methods. To create an OU this way, right-click on `dale.local`, go to New, then click Organizational Unit — once created, it appears in the directory tree.

---
<img width="988" height="455" alt="image" src="https://github.com/user-attachments/assets/0eb8e23e-763b-423d-96fc-146c37e4824c" />

To add a new user to the OU, right-click on IT, go to New, then select User — shown below is the new user account form for adding someone to the IT Organizational Unit.

---

<img width="833" height="538" alt="User created inside the new OU" src="https://github.com/user-attachments/assets/a627ef00-0ecc-4a31-8767-618cf3a9092e" />

---

<img width="768" height="510" alt="New user" src="https://github.com/user-attachments/assets/403a4f84-1d71-4df2-aaac-653f397a3f18" />

The new user James White has been successfully added as a user inside the IT Organizational Unit.

---
<img width="1036" height="148" alt="OU created via pwrSSH" src="https://github.com/user-attachments/assets/ebc2402b-5c88-47e5-9ce8-71474fe21a34" />

Creating the HR Organizational Unit using PowerShell instead of the GUI is a faster method commonly used for repetitive or large-scale organizations adding a new influx of users.
---

<img width="798" height="170" alt="User created in the OU via PWRSSH" src="https://github.com/user-attachments/assets/090750b2-19e6-4449-8fd3-7a3c0e78be8d" />\

Creating the HR user account (John Smith) using PowerShell command line. 

---

<img width="1012" height="176" alt="User created and verified" src="https://github.com/user-attachments/assets/525849cf-de8b-47ad-b6d1-12f00b6c3504" />

Confirming in Active Directory Users and Computers that the PowerShell-created OU and user appear correctly — verifying that both methods, GUI and PowerShell, produce the same result.

