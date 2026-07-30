# Attack Simulation With Kali Linux
---
## **<ins>Section Objective:</ins>**

This section shows the execution of an RDP brute-force simulation using Crowbar on Kali Linux to demonstrate how automated attacks exploit weak credentials, reinforcing the critical need for strict password length, complexity, and uniqueness policies

---
<img width="1915" height="968" alt="image" src="https://github.com/user-attachments/assets/50caebd2-1097-4e18-b31f-cdd12852cbdf" />

This is the Graphical User Interface (GUI) for Kali Linux. Looks similar to the Windows GUI but includes built-in hacking tools accessible via the terminal, which is the same layout as Windows Command Prompt. To have Kali on the same network as the other machines, we need to connect with a static IP. To configure a static IP, first click on the Ethernet icon on the top right.     

---

<img width="822" height="649" alt="Static IP created" src="https://github.com/user-attachments/assets/d29e2e56-7551-4a77-b7b3-d8619a5e66eb" />

After clicking on the Ethernet icon, click on Wired Connection 1 and then the cog symbol. After clicking on IPv4, you will be able to configure a static IP. The IP that will be used is `192.168.10.250`, with the gateway being `192.168.10.2` (router). The DNS (Domain Name System) IP that will be used is `8.8.8.8`, which is Google's DNS.    

---

<img width="617" height="198" alt="Confirming change of IP" src="https://github.com/user-attachments/assets/733df954-6e08-449e-b0dc-8d9914106538" />

We want to verify that the changes were made, so right-click on the desktop their is an option called Terminal. Once you click the terminal. The command `ip a` views the current IP configuration of the computer. As shown in purple, the IP has changed to $\color{purple}{\texttt{192.168.10.250}}$.   

---
<img width="504" height="260" alt="Ping AD Server" src="https://github.com/user-attachments/assets/f0398cb2-80ee-4f64-a2c1-f712a1e03517" />

The `ping command was used to verify the connectivity of Kali to the Splunk server's IP, which is `192.168.10.10`. As shown in the photo, 5 packets were transmitted, and 5 packets were received.   

---
<img width="988" height="821" alt="Install of Crowbar" src="https://github.com/user-attachments/assets/91b16752-a2f6-4f27-a660-91a130ef49e2" />

Installing Crowbar on the Kali Linux machine with `sudo apt-get install -y crowbar`.

---

<img width="959" height="603" alt="NEW MKDIR FILE" src="https://github.com/user-attachments/assets/d30a1089-f199-414c-88a9-7358ebb69044" />

Creating a new directory called `ad-project` using the command `mkdir ad-project`. Creates new files to store a file called `rockyou.txt` that is a popular word list used in brute force attacks.  

---

<img width="820" height="490" alt="Coping rockyou txt and moving it to AD-PROJECTS" src="https://github.com/user-attachments/assets/213464bc-3678-4f6c-9662-f3fc905479b1" />

Copying the `rockyou.txt` file.

---

<img width="702" height="659" alt="Using Head command to only show the 1st 40 lines of the file" src="https://github.com/user-attachments/assets/21c8f032-fe94-4e3b-a7f6-66ab2a1251c6" />

Using the `head command` to list only 40 lines, as it contains 14 million lines of weak passwords.  

---

Shown in the screenshots below is editing the password.txt file using the `nano command` so I can add a commonly used password. In a real-world scenario, there would be lots of reconnaissance work before doing this kind of attack. After saving the changes in the file to double-check that it was saved, and then use the `cat` command to display the current contents of the file.    

<img width="827" height="118" alt="Editing Passoword txt with NANO" src="https://github.com/user-attachments/assets/c4c296dc-0569-46ca-955f-8f33a8602701" />

<img width="993" height="708" alt="CAT command used to show output of fiel" src="https://github.com/user-attachments/assets/950b6fc1-8ae3-4dab-9376-4af27be07f0c" />

<img width="825" height="680" alt="USING NANO TO EDIT TXT fiel" src="https://github.com/user-attachments/assets/976dd99e-9528-4566-9481-ff5644dc68ba" />

---

<img width="894" height="658" alt="Options u can use with Crowbar" src="https://github.com/user-attachments/assets/805a4b2e-3965-411f-9a22-6b342c4b164d" />

Before using Crowbar, you can do `crowbar -h` for help and see what options are available.  

---

<img width="826" height="316" alt="Comamnd used with crowbar " src="https://github.com/user-attachments/assets/97389407-f1d0-4970-b35a-b1782993df53" />

Explain what the command is and what it's doing. 

---

<img width="658" height="60" alt="PWord FOUND!!!!" src="https://github.com/user-attachments/assets/ac7cd218-f8b1-4534-a59c-9935b0e8a81c" />

The password was found.

---
<img width="958" height="283" alt="searching endpoint from jsmith to see if there is an event " src="https://github.com/user-attachments/assets/bb7d2544-6db3-4a2c-ae9c-02ded19140fe" />

Logging back into the Splunk interface and searching under the index that was created to see if there were any events logged. 

---
<img width="595" height="253" alt="event code 4625 " src="https://github.com/user-attachments/assets/e6ea6207-d836-4e9e-b480-db7bfee365e7" />

---
<img width="694" height="578" alt="Event shoud failed in login attempts meaing a Bruteforce attack has occured " src="https://github.com/user-attachments/assets/52dab609-2c8d-466a-8687-6865d01afc9b" />

---
<img width="694" height="335" alt="BF finding the right pword and logging on " src="https://github.com/user-attachments/assets/8d9edeb5-63f6-4964-b541-bba736fcca4d" />

---

<img width="711" height="584" alt="Shows pc name that login and ip of PC" src="https://github.com/user-attachments/assets/ff9bb29b-3303-4e88-b0d5-51e8bde6a8e1" />
