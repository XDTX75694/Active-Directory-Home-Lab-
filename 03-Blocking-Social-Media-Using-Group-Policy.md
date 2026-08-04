# Blocking Social Media Using Group Policy
---
## **<ins>Section Objective:</ins>**

This section covers blocking access to social media sites (`Facebook`, `Instagram`, and `YouTube`) across the domain using Group Policy. Group Policy is an `Active Directory` feature that lets administrators enforce settings on computers and users from a central location rather than configuring each machine by hand.

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

<img width="502" height="524" alt="Sharing new folder over the network" src="https://github.com/user-attachments/assets/dd0b7c48-1da8-4b81-968b-8384483a7fae" />

In order for this file to reach other clients, the folder has to be shared over the network with the right permissions otherwise, the Group Policy won't be able to copy the file to each machine.

I right-clicked on the folder and selected Properties. From there, select Advanced sharing.    

---

<img width="788" height="541" alt="Making sure permissions are set to at lease read only " src="https://github.com/user-attachments/assets/1fe17b87-fefa-4366-b60b-2e52a9f3578f" />

Once in `Advanced Sharing`, I checked `Share this folder`, then clicked Permissions and confirmed that Everyone is set to `Read-only`. This matters because clients only need to read the file to copy. The other options are left unchecked so no one can tamper with the hosts file on the share.

---
<img width="722" height="156" alt="New hosts file in the new share folder" src="https://github.com/user-attachments/assets/06e42564-95cc-4280-93c5-241942e58607" />

After saving the share permissions, I dragged and dropped the new hosts file into the `Social Media Blocking` folder. This now makes it so this file will be copied when the new Group Policy is created.  

---

<img width="305" height="263" alt="Copying network path" src="https://github.com/user-attachments/assets/981c778e-4d9c-4070-b69c-da01bd75a8f0" />

Back on the Sharing tab, it now displays the network path as `\\ACDC01\Social Media Blocking`, which I copied to use as the source location in Group Policy.

---

<img width="570" height="737" alt="image" src="https://github.com/user-attachments/assets/6bc351fa-37d5-4177-a265-5fae56bda70e" />

<img width="1022" height="484" alt="Opeing GPO and AD users " src="https://github.com/user-attachments/assets/849373ff-1126-4898-af2c-06555e009798" />

On the Windows Server I opened Server Manager and used the Tools menu to launch `Active Directory Users and Computers` and `Group Policy Management`.

---

<img width="994" height="519" alt="image" src="https://github.com/user-attachments/assets/85c91cd1-9a15-46a4-9c8d-bce05300ccb1" />

With both consoles open, I went into `Group Policy Management`, right-clicked the `dale.local` domain, and selected `"Create a GPO in this domain, and Link it here..."` to  create a new policy.

---

<img width="562" height="402" alt="image" src="https://github.com/user-attachments/assets/05da23b8-a6b3-411a-9b63-ac244983a844" />

In the New GPO window, I named the policy `Social Media Blocking_GPO` and then clicked OK. Giving the GPO a descriptive name makes it easy for an administrator reviewing the domain's policies of changes are ever needed. 

---

<img width="435" height="491" alt="image" src="https://github.com/user-attachments/assets/c91caae9-eb19-4ac1-a757-7b6abc8efd57" />

With a new Group Policy created, I then right-clicked `Social Media Blocking_GPO` and selected Edit to open the `Group Policy Management Editor` where the file-deployment settings are configured.

---

<img width="782" height="541" alt="image" src="https://github.com/user-attachments/assets/0517c37b-3d8f-41e2-88e3-0f35084f158e" />

<img width="491" height="393" alt="RC for new file " src="https://github.com/user-attachments/assets/c41f5550-3f8e-4166-9b98-7409042149ab" />

In the editor, I navigated to `Computer Configuration`, `Preferences`, `Windows Settings`, `Files`, the section that allows Group Policy to copy or replace files on target computers.

Once in the Files section, I right-clicked in the empty pane and selected `New File`. This opens the properties window. 

---

<img width="596" height="516" alt="image" src="https://github.com/user-attachments/assets/aeb5d16b-7857-448e-b1fc-6a53d087bec1" />

In the New File Properties window, I set the Action to `Replace`, the Source to the shared network path `\\ACDC01\Social Media Blocking\hosts`, and the Destination to `C:\Windows\System32\drivers\etc\hosts` on the client. This now swaps the client's `local hosts file` for the customized one connected to the `dale.local` domain.

---

<img width="840" height="646" alt="FILE WAS CREATED" src="https://github.com/user-attachments/assets/bb2bcfb1-e80d-4d71-b201-79b0bbedd7f1" />

The Files section now shows that the hosts rule with the Replace action, network source, and local target has been applied. 

---
<img width="889" height="585" alt="Linking new GPO to IT" src="https://github.com/user-attachments/assets/6f2c55f3-1c5f-4c72-bc6f-7b2e0e9060e4" />

---
<img width="754" height="449" alt="SMB Blocking link in IT OU" src="https://github.com/user-attachments/assets/758c147b-2ac9-4d70-b2ed-18a18100e0ab" />

---
<img width="583" height="163" alt="image" src="https://github.com/user-attachments/assets/31165d2e-8789-4ebb-9e9c-824817aa3eed" />

Once the Group Policy is created, it has to be linked to the `Organizational Unit` for it to apply to its users. I right-clicked the first Organizational Unit and selected `"Link an Existing GPO..."`, then in the Select GPO window I chose `Social Media Blocking_GPO` and clicked OK. I repeated the same steps for the second OU, and the photo above shows the policy now linked under both `HR` and `IT`.

---
<img width="987" height="523" alt="Updating GPO in CMD" src="https://github.com/user-attachments/assets/0432e73c-44f0-40ce-91f2-57d9413566db" />

To apply the changes right away instead of waiting for the automatic Group Policy refresh, which can take a while to apply, I opened `Command Prompt` as `Administrator` and ran `gpupdate /force`. This command reapplies all policies rather than only the changed ones.

---
After applying the new Group Policy, I logged into the `Windows 10 client` to verify the configuration and confirmed that `Facebook`, `YouTube`, and `Instagram` were all blocked. Each site returned with the page being blocked, provided that the Group Policy had been successfully deployed.  

<img width="963" height="504" alt="FB is blocked" src="https://github.com/user-attachments/assets/5a71dca4-7740-41f0-b038-6d01044c3f4d" />
<img width="991" height="766" alt="IG is blocked" src="https://github.com/user-attachments/assets/8d8ee637-8693-4c42-b472-83f6ef345e5a" />
<img width="1019" height="554" alt="YT is blocked " src="https://github.com/user-attachments/assets/e578b9d8-e354-4f2d-8e98-605b52518f20" />















