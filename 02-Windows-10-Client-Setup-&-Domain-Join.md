# Windows 10 Configuration & Joining the Domain
---
## **<ins>Section Objective:</ins>**
This section covers configuring the Windows 10 client machine and joining it to the dale.local domain, and the steps that connect a regular workstation to the network managed by Active Directory. Once joined, the client can be logged into using domain user accounts, is visible and manageable from the domain controller, and is ready to receive Group Policy settings and forward logs to Splunk. Set up for Splunk will be shown in the next section.

---
<img width="1036" height="796" alt="Install complete" src="https://github.com/user-attachments/assets/34dba885-97e4-4a6f-8914-a105867097f2" />

The setup for the client machine is nearly identical, with the only difference being choosing Windows 10 Pro workstation. The Pro version is used because it includes enterprise features like the ability to join a domain, Group Policy managment, and the use of the Remote Desktop Protocol (RDP), which the Home edition lacks.

---
<img width="1027" height="726" alt="Changing PC name " src="https://github.com/user-attachments/assets/8522f7dd-562e-42cd-982d-1f0c3d72c675" />

Here we are changing the client's computer name to Target-PC, making it easier to identify within the lab environment. In a real-world setting, the computer's name would typically correlate with its department or device type. For example, a naming convention like `HR-LAPTOP-04` or `IT-DESKTOP-12`.      

---
<img width="828" height="653" alt="Change confirmed" src="https://github.com/user-attachments/assets/5e72998d-6813-4322-b283-068b731e8415" />

I went back to the settings to confirm the name change was successful. 

---

<img width="866" height="645" alt="ip change " src="https://github.com/user-attachments/assets/b4868872-6308-46ad-aa3e-d7303098d7f0" />

I set a static IP address `(192.168.10.100)` on the Windows 10 client, replacing the automatically assigned DHCP address shown in the original network diagram. A static IP keeps the client's connection to Splunk and other services stable across reboots, since a DHCP lease can change over time and break log forwarding or any firewall rules tied to a fixed address.

---
<img width="700" height="455" alt="image" src="https://github.com/user-attachments/assets/1d69e636-537b-4d59-a194-26c23801bf1c" />

To connect this computer to the domain, type "PC" into the search bar and select Properties. From there, scroll down to the About section and click Advanced system settings, as shown in the photo below.    

---

<img width="796" height="507" alt="Joining win 10 to the domain" src="https://github.com/user-attachments/assets/6db4d1d7-718f-4fa9-b099-f12b6a0a8db0" />

Once in Advanced system settings, click the Computer Name tab, then select Change. This opens a window where you can set the computer's name and choose whether to join a workgroup or domain, as shown in the photos below.       

---
<img width="795" height="625" alt="image" src="https://github.com/user-attachments/assets/5d738cac-c30e-46bf-91d6-b55347b7b40e" />

---
<img width="438" height="495" alt="image" src="https://github.com/user-attachments/assets/0c9b7dfd-056d-49b7-a9da-68bb9af346c0" />

---
<img width="669" height="473" alt="Welcome to the domain" src="https://github.com/user-attachments/assets/f99da47f-ced0-4073-a95a-2c42eaa9c03e" />

After entering the domain information and clicking OK, a confirmation message appears indicating the computer successfully joined the domain.

---
<img width="688" height="473" alt="image" src="https://github.com/user-attachments/assets/71a87900-5891-487c-93f4-d92d327301e0" />

Joining the domain requires a restart for the changes to take effect. After rebooting, the login screen now accepts domain accounts rather than only the local one, and I signed in as James White, the user created earlier in Active Directory, for the first time.
---

<img width="1038" height="835" alt="Loging in with JWHITE on the dale domain" src="https://github.com/user-attachments/assets/2f07c58a-134b-4271-a058-aee4e6138fcb" />

---

<img width="994" height="709" alt="Both users are on there" src="https://github.com/user-attachments/assets/93d77b00-0771-43c5-9776-9b9efd6ea493" />

I repeated the same process with John Smith's credentials, shown in the screenshots above. This confirms that both domain accounts, jsmith and jwhite, were created correctly and can authenticate against the domain controller.
























