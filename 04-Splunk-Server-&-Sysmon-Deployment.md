# Splunk Server & Sysmon Deployment
---
## **<ins>Section Objective:</ins>**

This section covers deploying Splunk as the lab's SIEM (Security Information and Event Management) system on an Ubuntu server, and installing Sysmon on the Active Directory server and Windows 10 client. Splunk collects and centralizes logs from across the network, while Sysmon provides detailed endpoint telemetry, such as process creation and network connections, which is forwarded to Splunk. Together, these tools make it possible to search, analyze, and detect suspicious activity. 

---

<img width="550" height="500" alt="Making sever more beefy" src="https://github.com/user-attachments/assets/544d667e-edfa-4f43-837d-2c2378544085" />

The Splunk server is more powerful than the rest of the VMs since it handles the heavy lifting of the lab, ingesting logs from every machine and running searches across them. I allocated it 95GB of storage, 8GB of RAM, and 4 CPU cores.

---

<img width="1283" height="410" alt="Account setup" src="https://github.com/user-attachments/assets/3f335b50-cbf3-4003-9150-eb4ea1cbb868" />

Unlike the Windows installers, Ubuntu Server uses a text-based interface, since server editions of Linux typically leave out the graphical desktop to save resources.

---

<img width="600" height="250" alt="Installing most UTD version " src="https://github.com/user-attachments/assets/96210262-1ff2-4e2b-b5b9-cee8da24c77c" />

After installation, I ran sudo apt-get update && sudo apt-get upgrade -y to bring the server up to date. The first command refreshes the list of available updates, and the second installs them, with the -y flag automatically approving the installation so the whole process runs with a single command. Keeping packages current is one of the simplest and most important security practices on any Linux server.
---

<img width="887" height="619" alt="Sever is updated " src="https://github.com/user-attachments/assets/42e51c78-9d1e-4e17-81cb-5406f3e2a05e" />

The output confirms the updates installed successfully. As a good practice, I rebooted the server afterward to ensure every change was fully applied, since some updates, like new kernels, only take effect after a restart.

---
<img width="657" height="170" alt="installing network with static ip" src="https://github.com/user-attachments/assets/25c5a192-ac8a-4e93-8588-75d3e83ecd30" />

Editing the Netplan configuration file (00-installer-config.yaml) to set a static IP. This gives the server a permanent network address, which is essential because every machine in the lab forwards its logs to Splunk at this address, and that would break if the IP changed on reboot.

---

<img width="863" height="671" alt="Setting up static ip" src="https://github.com/user-attachments/assets/2a7f2878-1291-4ac3-a210-6d521a056e8a" />

Inside the file, I defined the server's static IP, gateway, and DNS servers. Netplan uses `YAML`, a human-readable format that is strict about indentation, so even one misplaced space will cause the configuration to fail. With these values set manually instead of assigned by DHCP, the server's address is now permanent.

---

<img width="956" height="661" alt="Settings applyed" src="https://github.com/user-attachments/assets/bbffb83e-d041-4ef1-b970-e67a2453b932" />

<img width="830" height="263" alt="IP has changed" src="https://github.com/user-attachments/assets/2bbe4ae5-ebe5-4042-8329-e431bdb74d52" />

Running `sudo netplan apply` activates the new network configuration. To confirm the static IP took effect, I ran `ip a`, which shows the server now using the address defined in the Netplan file as shown above.  

---

<img width="813" height="320" alt="Insalling tool to help install splunk" src="https://github.com/user-attachments/assets/0c691b97-8f16-4ca4-85d7-4b96fbe8366b" />

Before deploying Splunk, I installed `open-vm-tools`, VMware's guest utilities for Linux. These improve VM performance and enable features like proper time synchronization, graceful shutdowns from the host, and clipboard/file sharing, all of which make the VM more stable to work with.

---

<img width="759" height="730" alt="adding splunk file for download" src="https://github.com/user-attachments/assets/4ba536ca-f8d6-4b26-86bc-7e937213114d" />
<img width="761" height="730" alt="File Path to splunk folder" src="https://github.com/user-attachments/assets/00208c25-5dd5-4317-8d87-1dfcd19e5895" />

In VMware's settings, I enabled Shared Folders, a feature that bridges the host computer and the virtual machine so files can be passed directly between them. This is how the Splunk installer was transferred onto the Linux server, as shown in the two photos above.
---

<img width="903" height="251" alt="Creating a directory to install splunk from shared folder" src="https://github.com/user-attachments/assets/6acce36a-b58f-4d9f-a8cb-520d8d6427af" />

After enabling Shared Folders in VMware, I mounted the share inside the Linux server using the commands below. The Splunk folder from the host computer is now visible on the server.

`sudo mkdir /mnt/hgfs` — Creates the mount point, the folder where the shared files will appear.

`sudo vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other` — Mounts the VMware shared folder to that location.

`cd /mnt/hgfs then ls` — Changes into the directory and lists its contents, confirming the shared folder and the Splunk installer are visible.

---

<img width="749" height="265" alt="Inside folder" src="https://github.com/user-attachments/assets/74d2ade4-632d-45fb-97bf-6f35d21a6b83" />

Inside the shared folder, I changed into the Splunk installer file and ran the installation command `sudo dpkg -i splunk*.deb` to install the software for Splunk.

---

<img width="622" height="213" alt="Download complete" src="https://github.com/user-attachments/assets/65d5769e-7fb7-4eef-bb18-b28d247e6453" />

I installed the Splunk package using `sudo dpkg -i`, the Debian package installer. The first attempt failed because Linux treats filenames as case-sensitive, so Splunk and splunk are two different names. Retyping the filename with the correct case resolved the error, and the installation completed successfully.

---

<img width="826" height="383" alt="Users and groups belong to splunk" src="https://github.com/user-attachments/assets/182e187b-ab62-4ecf-a845-0aaf87b96583" />

Confirming the Splunk installation directory (/opt/splunk) is owned by a dedicated splunk user and group rather than root. This is a security best practice known as least privilege, meaning Splunk runs with only the permissions it needs instead of full system access.

---

<img width="844" height="687" alt="Terms and conditons " src="https://github.com/user-attachments/assets/9af87055-84b4-44de-98d9-1a05607cdeb5" />

On first launch, Splunk displays its license agreement. Accepting it by entering y completes the setup, after which Splunk prompts for the administrator username and password.
---

<img width="713" height="180" alt="Installed and making splunk start up apon booting" src="https://github.com/user-attachments/assets/1e45a8f8-8f3e-407c-8c0c-b8782acb0173" />

Splunk is now accessible through its web interface at `192.168.10.10:8000`, the static IP configured earlier plus Splunk's default web port. After exiting the Splunk user session, I enabled boot-start so Splunk launches automatically under the Splunk user whenever the server restarts.

---

<img width="508" height="48" alt="WIll now run when booted " src="https://github.com/user-attachments/assets/416fcad5-3864-4de3-95ce-8d2dffceb20e" />

Confirming the boot-start init script was installed and configured, meaning Splunk will now start automatically every time the server boots.

---

<img width="1031" height="766" alt="Checking server access" src="https://github.com/user-attachments/assets/67340e46-c6f6-461b-be98-8b97e8289e0a" />

After installing Splunk on the Linux server, I switched to the Windows client to confirm that the Splunk interface was reachable at `192.168.10.10:8000` via a web browser. 

---

<img width="941" height="787" alt="insatlling the UF for splunk" src="https://github.com/user-attachments/assets/dd30e44e-aa8d-4164-87a1-1fd73c346057" />

After configuring the Splunk server, the next step is installing the Splunk Universal Forwarder on the Windows machines. The forwarder is a lightweight agent that runs quietly in the background on each endpoint, collecting that machine's logs and sending them to the Splunk server, which acts as the central hub that stores and analyzes the data.

---

<img width="861" height="632" alt="Installer popup" src="https://github.com/user-attachments/assets/93ef9747-fdab-4ff7-8ada-55b00ed60bda" />

---

<img width="793" height="584" alt="image" src="https://github.com/user-attachments/assets/aecb1333-6a42-46ea-b5a0-4f5060be7768" />

Running the Universal Forwarder installer launches a setup wizard, shown in the screenshot above. During setup, I pointed the forwarder to the Splunk server using the static IP `192.168.10.10` and port `9997`, the standard port Splunk uses to receive forwarded logs. This tells the forwarder exactly where to send the data it collects.

---

<img width="816" height="417" alt="image" src="https://github.com/user-attachments/assets/ad5b2e38-5317-4907-801e-c5d089291ef3" />

Next, I installed Sysmon, a free Microsoft tool that enhances Windows' basic logging by recording detailed system activity such as program launches, network connections, and file changes. Sysmon writes this activity to the Windows event log, where the Universal Forwarder picks it up and ships it to the Splunk server.

---

<img width="1027" height="761" alt="Symon olaf to with splunk" src="https://github.com/user-attachments/assets/0079951d-cad6-4505-a40a-6b53536ea53c" />

Sysmon needs a configuration file that tells it which events to record. Rather than writing one from scratch, I used Sysmon-Modular by `Olaf Hartong, an industry-trusted, open-source configuration maintained by the security community. It comes pre-tuned to capture the telemetry that matters most for threat hunting, giving Sysmon the right visibility without logging so much noise that the important events get buried.

---

<img width="661" height="317" alt="Sysmon installed success " src="https://github.com/user-attachments/assets/e392f58e-8fbf-4945-8f90-0b54eeb4d58e" />

<img width="770" height="138" alt="Command to install Sysmon using powershell" src="https://github.com/user-attachments/assets/e577c5f0-8cbb-4d89-8f59-63ed28760965" />

With the configuration file downloaded, I installed the Sysmon config file by running `PS C:\Sysmon.exe -i ..\sysmonconfig.xml` in PowerShell as administrator. The `-i flag` is used to install Sysmon as a Windows service and applies the Olaf config that dictates what to monitor. The output confirms that Sysmon is now running and monitoring any changes in the background.

---

<img width="807" height="617" alt="Inputs conf file for slunk server" src="https://github.com/user-attachments/assets/5cb48341-2005-4cd6-bde6-395d8bb2bbdb" />

<img width="799" height="613" alt="Config file saved in the local file for splunk UF" src="https://github.com/user-attachments/assets/757b1f3a-27e3-44db-8f07-d151408345c6" />

 ---
The next step is telling the Universal Forwarder which logs to send to Splunk, which is controlled by a file called `inputs.conf`. The first screenshot shows the default version of the file in the forwarder's default folder. The second shows a `new inputs.conf` I created in the `local folder`. Splunk is designed so that files in the local folder override the defaults and are never touched by software updates, so custom settings belong there while the originals in the default folder stay intact as a safe fallback. The new config file collects logs from the `Application`, `Security`, and `System event logs`, plus Sysmon's operational log. 

<img width="857" height="605" alt="In services tab to chance to local host for logs to save " src="https://github.com/user-attachments/assets/e115a6f0-48df-476b-9ba1-5fddd519d399" />

---

After saving the `new config file`, I opened Windows Services and adjusted the SplunkForwarder service to log on as the Local System account. By default, the forwarder runs under a limited account that can't read protected logs like Security events. Switching to `Local System` gives the service the permissions it needs to access all the logs specified in `inputs.conf` and forward them to Splunk.

---

<img width="752" height="513" alt="restarting service to update it " src="https://github.com/user-attachments/assets/4bc1695c-ef41-49e0-a095-e7a431c1a0c9" />

Restarting the SplunkForwarder service is required for it to load the new configuration. Once restarted, the forwarder begins collecting the specified logs and forwarding them to the Splunk server.

---

<img width="1021" height="743" alt="Login to Splunk " src="https://github.com/user-attachments/assets/b4829e97-3393-4ad8-afcf-635bec7e90e1" />

Logging into the Splunk web interface at `192.168.10.10:8000` brings up the main console. This is the heart of the SIEM, where every log forwarded from the Windows machines arrives and where all searching and analysis takes place.

---

<img width="1005" height="706" alt="Creating new index named ENDPOINT" src="https://github.com/user-attachments/assets/10d59a1a-fa72-4949-ad3d-3376520379c9" />

Creating a new index called endpoint (`Settings`, `Indexes`, `New Index`). An index is where Splunk stores incoming data, and the forwarder's inputs.conf was configured to send all logs to an index named endpoint, so this index must exist on the server for the data to arrive. If it did not, the incoming logs would have nowhere to land.

---

<img width="1012" height="74" alt="Endpoint created" src="https://github.com/user-attachments/assets/ce039570-28b8-426b-8591-78ff5f794c8e" />

The new `endpoint index` now appears in the list, confirming it was created successfully.

---
<img width="1214" height="512" alt="image" src="https://github.com/user-attachments/assets/ba543ab4-f24f-463a-91fb-6673362fe60d" />

---
<img width="1251" height="322" alt="image" src="https://github.com/user-attachments/assets/e94a2750-a199-436f-99f5-43cbe01b7f73" />

---
<img width="1003" height="506" alt="Config of port for Splunk" src="https://github.com/user-attachments/assets/e22f9737-9ea5-489e-9292-8f2ebf2389c3" />

The final piece is telling Splunk to listen for incoming data. In `Settings`, `Forwarding and receiving`, I clicked `Configure receiving` and, since no listening port existed yet, created one on `port 9997`, the same port the forwarder was configured to send to. With both sides matching, the pipeline is complete.

---
<img width="1278" height="357" alt="image" src="https://github.com/user-attachments/assets/154c6e5e-5244-4401-ae68-a17d934a5cb7" />

---
<img width="1034" height="752" alt="Slunk is working " src="https://github.com/user-attachments/assets/c3dffa6f-237d-4f97-8bf8-8bda046b1d6d" />

---
<img width="824" height="829" alt="image" src="https://github.com/user-attachments/assets/28d3464b-5cbb-4aa7-ba39-72c63d1ecc59" />

With everything configured, I verified the pipeline by going to `Apps`, `Search & Reporting` and searching `index=endpoint`. The results returned `1,596 events` from the client machine (Target-PC), with the source field confirming that `Security`, `Application`, `System`, and `Sysmon logs` are all being collected. The same forwarder and Sysmon setup was then repeated on the Windows Server machine.












