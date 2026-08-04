# Blocking Social Media Using Group Policy
---
## **<ins>Section Objective:</ins>**

This section covers blocking access to social media sites (`Facebook`, `Instagram`, and `YouTube`) across the domain using Group Policy. Instead of configuring each computer manually, I created a custom `hosts file` that blocks these sites, shared it from the Domain Controller, and used a GPO File Preference to push it automatically to HR and IT Organizational Units.

---

<img width="910" height="745" alt="File explore and go into sys32 and look for drivers" src="https://github.com/user-attachments/assets/5f4311e8-2418-431c-a504-d925d2dc166c" />

---

<img width="913" height="795" alt="Host file on drivers" src="https://github.com/user-attachments/assets/8cafe505-1b16-446f-8aee-1ac1d498108f" />



In File Explorer, I navigated to `C:\Windows\System32\drivers` and opened the etc folder. This folder contains the `hosts file`, which is a system file that Windows checks before asking the Domain Name System (DNS) to look up a website. Because it's checked first, any entry in this file overrides normal name resolution, which makes it a simple and reliable way to block specific sites. (Shown in the photos below).

---
<img width="677" height="532" alt="creating new hosts txt file " src="https://github.com/user-attachments/assets/60b19414-a751-4e31-9094-5d0aa0ac186a" />

On the desktop, I right-clicked and selected `New`, `Text Document`. This blank file will become the new `hosts file` used to block the targeted websites.

---

<img width="856" height="581" alt="0 0 0 0 for the Websites IPs " src="https://github.com/user-attachments/assets/721756ba-b590-408b-b445-17fd34485ef7" />

I opened the new file in Notepad and added `0.0.0.0` to each targeted domain. 0.0.0.0 is used because it is a non-routable address, so when a computer using this file tries to reach any of these sites, the lookup dead-ends and the page can never load, essentially blocking these sites.

---
<img width="577" height="350" alt="Removing txt to make it the new host file " src="https://github.com/user-attachments/assets/151ef257-ac24-4939-9bf6-f9c884789916" />

After saving the new text file, I renamed `hosts.txt` to just `hosts`. Windows will only recognize the file by that exact name, and if the .txt extension were left on, then the file would be ignored.

---

<img width="774" height="271" alt="Made new file on the C drive" src="https://github.com/user-attachments/assets/dbf36380-067e-4eb5-a39a-70a34fdf5197" />

After saving the new hosts file, I created a new folder on the `C: drive` named `Social Media Blocking`. This folder acts as the central location where the file is stored and shared out to the rest of the clients on the domain.

---
In order for this file to reach other clients, the folder has to be shared over the network with the right permissions otherwise the Group Policy won't be able to copy the file to each machine.  `(Seen in the 4 photos below)`

<img width="502" height="524" alt="Sharing new folder over the network" src="https://github.com/user-attachments/assets/dd0b7c48-1da8-4b81-968b-8384483a7fae" />

---

<img width="788" height="541" alt="Making sure permissions are set to at lease read only " src="https://github.com/user-attachments/assets/1fe17b87-fefa-4366-b60b-2e52a9f3578f" />

---
<img width="722" height="156" alt="New hosts file in the new share folder" src="https://github.com/user-attachments/assets/06e42564-95cc-4280-93c5-241942e58607" />

---
<img width="305" height="263" alt="Copying network path" src="https://github.com/user-attachments/assets/981c778e-4d9c-4070-b69c-da01bd75a8f0" />







