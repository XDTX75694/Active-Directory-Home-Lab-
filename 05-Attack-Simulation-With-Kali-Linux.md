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

We want to verify that the changes were made, so right-click on the desktop their is an option called Terminal. Once clicking the terminal. The command `ip a` views the current IP configuration of the computer. As shown in purple, the IP has changed to $\color{purple}{\texttt{192.168.10.250}}$.   

---
<img width="504" height="260" alt="Ping AD Server" src="https://github.com/user-attachments/assets/f0398cb2-80ee-4f64-a2c1-f712a1e03517" />

The `ping command` was used to verify the connectivity of Kali to the Splunk server's IP, which is `192.168.10.10`. As shown in the photo, 5 packets were transmitted, and 5 packets were received.   

---
<img width="988" height="821" alt="Install of Crowbar" src="https://github.com/user-attachments/assets/91b16752-a2f6-4f27-a660-91a130ef49e2" />

Installing Crowbar on the Kali Linux machine with `sudo apt-get install -y crowbar`. `(Add making a files using mkddir ad-project to put all files in aka Crowbar files)`

---
<img width="792" height="796" alt="1st 40 lines and adding to different TXT" src="https://github.com/user-attachments/assets/08e0f7fa-e20c-447a-b608-4ad9c9355857" />

---
<img width="992" height="846" alt="Using this text file for the bruteforce attack with 40 lines of txt from rockyou txt" src="https://github.com/user-attachments/assets/0bd1adfb-3e12-4e72-aca1-26c486455b2b" />

---
<img width="894" height="658" alt="Options u can use with Crowbar" src="https://github.com/user-attachments/assets/805a4b2e-3965-411f-9a22-6b342c4b164d" />

---
<img width="658" height="60" alt="PWord FOUND!!!!" src="https://github.com/user-attachments/assets/ac7cd218-f8b1-4534-a59c-9935b0e8a81c" />

---
<img width="958" height="283" alt="searching endpoint from jsmith to see if there is an event " src="https://github.com/user-attachments/assets/bb7d2544-6db3-4a2c-ae9c-02ded19140fe" />

---
<img width="595" height="253" alt="event code 4625 " src="https://github.com/user-attachments/assets/e6ea6207-d836-4e9e-b480-db7bfee365e7" />

---
<img width="694" height="578" alt="Event shoud failed in login attempts meaing a Bruteforce attack has occured " src="https://github.com/user-attachments/assets/52dab609-2c8d-466a-8687-6865d01afc9b" />

---
<img width="694" height="335" alt="BF finding the right pword and logging on " src="https://github.com/user-attachments/assets/8d9edeb5-63f6-4964-b541-bba736fcca4d" />

---

<img width="711" height="584" alt="Shows pc name that login and ip of PC" src="https://github.com/user-attachments/assets/ff9bb29b-3303-4e88-b0d5-51e8bde6a8e1" />
