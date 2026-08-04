# Attack Simulation With Kali Linux
---
## **<ins>Section Objective:</ins>**

This section shows the execution of an RDP brute-force simulation using `Crowbar` on Kali Linux to demonstrate how automated attacks exploit weak credentials. This demonstration will show why having a strict password policy is important for keeping users and organization secure.  
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

The `ping command` was used to verify the connectivity of Kali to the Splunk server's IP, which is `192.168.10.10`. As shown in the photo, 5 packets were transmitted, and 5 packets were received.   

---
<img width="988" height="821" alt="Install of Crowbar" src="https://github.com/user-attachments/assets/91b16752-a2f6-4f27-a660-91a130ef49e2" />

Installing Crowbar on the Kali Linux machine with `sudo apt-get install -y crowbar`.

---

<img width="959" height="603" alt="NEW MKDIR FILE" src="https://github.com/user-attachments/assets/d30a1089-f199-414c-88a9-7358ebb69044" />

Using `mkdir ad-project` to create a dedicated project folder. `mkdir ("make directory")` is the Linux command for creating a new folder, which was made to store the attack files, including the `rockyou.txt` wordlist used in the brute-force simulation.

---

<img width="820" height="490" alt="Coping rockyou txt and moving it to AD-PROJECTS" src="https://github.com/user-attachments/assets/213464bc-3678-4f6c-9662-f3fc905479b1" />

Locating and copying the `rockyou.txt` wordlist into the project folder. Navigating to `/usr/share/wordlists` (where Kali stores its built-in wordlists), I used `cp rockyou.txt ~/Desktop/ad-project` to copy the file into my `ad-project directory`. Running `ls -lh` afterward confirms it copied successfully,

---

<img width="702" height="659" alt="Using Head command to only show the 1st 40 lines of the file" src="https://github.com/user-attachments/assets/21c8f032-fe94-4e3b-a7f6-66ab2a1251c6" />

Using the command `head -n 40 rockyou.txt` to display only the first 40 lines of rockyou.txt. The full wordlist contains over 14 million passwords. 

---

Using the `nano command`, I edited `passwords.txt` to add a commonly used password (shown below). In a real attack, this step would follow significant reconnaissance to build a targeted list. After saving, I ran the `cat command` to display the file and verify the change was saved.

<img width="827" height="118" alt="Editing Passoword txt with NANO" src="https://github.com/user-attachments/assets/c4c296dc-0569-46ca-955f-8f33a8602701" />

<img width="825" height="680" alt="USING NANO TO EDIT TXT fiel" src="https://github.com/user-attachments/assets/976dd99e-9528-4566-9481-ff5644dc68ba" />

<img width="993" height="708" alt="CAT command used to show output of fiel" src="https://github.com/user-attachments/assets/950b6fc1-8ae3-4dab-9376-4af27be07f0c" />

---

<img width="894" height="658" alt="Options u can use with Crowbar" src="https://github.com/user-attachments/assets/805a4b2e-3965-411f-9a22-6b342c4b164d" />

Running `crowbar -h` shows the help menu and available options. Crowbar can brute-force several services, including `RDP` and `SSH`. The main flags are `-b (service)`, `-s (target IP)`,`-u (username)`, and `-C (password list)`.

---

<img width="826" height="316" alt="Comamnd used with crowbar " src="https://github.com/user-attachments/assets/97389407-f1d0-4970-b35a-b1782993df53" />

The command crowbar `-b rdp -u jwhite -C passwords.txt -s 192.168.10.100/32 -v` launches the brute-force attack. It tells Crowbar to target `RDP (-b rdp)` on a single machine `(-s ...100/32)`, attempting every password in passwords.txt `(-C)` against the jwhite account `(-u)`, with `-v` showing each attempt as it runs.

---

<img width="787" height="552" alt="image" src="https://github.com/user-attachments/assets/67d952a2-24df-4679-9faa-b62549443b90" />


The attack succeeds. Working through the password list one attempt at a time, the tool tries each entry against the jsmith account until it lands on the match:

`[3389][rdp] host: 192.168.10.100 login: jsmith password: P@$$word123!
1 of 1 target completed, 1 valid password found`

The account was compromised through nothing more than a common, weak password.

---

<img width="958" height="283" alt="searching endpoint from jsmith to see if there is an event " src="https://github.com/user-attachments/assets/bb7d2544-6db3-4a2c-ae9c-02ded19140fe" />

Back in Splunk, I searched `index="endpoint"` jsmith to check whether the attack was logged. This filters to all events tied to the targeted account.

---

<img width="1013" height="405" alt="400 events on splunk" src="https://github.com/user-attachments/assets/d38f4ba8-1964-4ac3-af2f-9b437d2c2e7e" />

Splunk captured 474 events for the jsmith account that show activity in the timeline of the last 30 minutes. 

---

<img width="888" height="542" alt="Event code of the brute forece" src="https://github.com/user-attachments/assets/83f49f8f-7343-4309-ab2c-753981e21359" />

Clicking into the EventCode field breaks down exactly what type of activity was logged. If we look at the `Event Code 4625` with 39 occurrences, this event code means that there was a  failed logon attempt. 

---

<img width="794" height="599" alt="Show miltiple attemps to login every second" src="https://github.com/user-attachments/assets/47b8d048-cbf2-409f-be6d-1085fee1dfb0" />

Clicking into an individual event shows the full detail Splunk captured. Repeated login attempts against the `jsmith` user account share the same 6:43 timestamp within the same second, showing that a brute-force attack took place. 

The impact here is limited since jsmith is a `standard user` with restricted permissions. Against an   `administrator account` however could be far worse: privileged accounts carry high-level access that, if compromised, can be used to damage systems and spread across an entire environment.


---

<img width="711" height="584" alt="Shows pc name that login and ip of PC" src="https://github.com/user-attachments/assets/3fc8a2de-adb8-4d88-bdac-504ed90b4f9b" />

Looking at the event reveals the attacker's origin. The log captures the Workstation Name `("kali")` and Source IP `(192.168.10.250)` and traces the attack back to the Kali machine. This is `Event ID 4624`, the `Windows code for a successful logon`. The log also shows the authentication used `NTLM` which is a common protocol brute-force tools target. The event IDs are evidence for a security analyst to determine if a cyberattack has happened.



