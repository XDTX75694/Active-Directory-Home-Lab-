# Splunk Server & Sysmon Deployment
---
## **<ins>Section Objective:</ins>**

This section covers deploying Splunk as the lab's SIEM (Security Information and Event Management) system on an Ubuntu server, and installing Sysmon on the Active Directory server and Windows 10 client. Splunk collects and centralizes logs from across the network, while Sysmon provides detailed endpoint telemetry, such as process creation and network connections, which is forwarded to Splunk. Together, these tools make it possible to search, analyze, and detect suspicious activity. 

---

<img width="550" height="500" alt="Making sever more beefy" src="https://github.com/user-attachments/assets/544d667e-edfa-4f43-837d-2c2378544085" />

The Splunk Server is a bit beefier than the rest of the VMs because it will be ingesting data and running searches. The specs of the server are 95GB of storage, 8 GB of RAM, and 4 CPU cores.  

---

<img width="1283" height="410" alt="Account setup" src="https://github.com/user-attachments/assets/3f335b50-cbf3-4003-9150-eb4ea1cbb868" />

The configuration screen looks different from the Windows config screen because some Linux distributions and versions are command-line interface only.

---

<img width="600" height="250" alt="Installing most UTD version " src="https://github.com/user-attachments/assets/96210262-1ff2-4e2b-b5b9-cee8da24c77c" />

I use sudo apt-get update && sudo apt-get upgrade -y to keep the server current. The first part checks what updates are available, and the second part installs them all automatically without needing manual approval, which keeps the system secure and up to date with one command.

---

<img width="887" height="619" alt="Sever is updated " src="https://github.com/user-attachments/assets/42e51c78-9d1e-4e17-81cb-5406f3e2a05e" />

Once the command completes, a confirmation screen verifies the updates were successfully installed, and the server restarts its services. A good practice is to reboot the system to ensure every change is fully applied if not prompted to.

---
<img width="657" height="170" alt="installing network with static ip" src="https://github.com/user-attachments/assets/25c5a192-ac8a-4e93-8588-75d3e83ecd30" />

Editing the Netplan configuration file (00-installer-config.yaml) to set a static IP — this gives the server a permanent network address, which is essential for services like DNS and Active Directory that other machines depend on.

---

<img width="863" height="671" alt="Setting up static ip" src="https://github.com/user-attachments/assets/2a7f2878-1291-4ac3-a210-6d521a056e8a" />

Configuring the server's static IP, gateway, and DNS in YAML, a human-readable configuration format that Ubuntu's networking system requires. YAML is strict about spacing and indentation and requires precise formatting to work. Defining these settings manually makes the server's IP static. 

---

<img width="956" height="661" alt="Settings applyed" src="https://github.com/user-attachments/assets/bbffb83e-d041-4ef1-b970-e67a2453b932" />

Running `sudo netplan apply` confirms that the server's static IP was applied and saved. 

---

<img width="830" height="263" alt="IP has changed" src="https://github.com/user-attachments/assets/2bbe4ae5-ebe5-4042-8329-e431bdb74d52" />

Using the `ip a command` to display the server's network details, this is a second way to verify that the static IP address was applied correctly.

---

<img width="813" height="320" alt="Insalling tool to help install splunk" src="https://github.com/user-attachments/assets/0c691b97-8f16-4ca4-85d7-4b96fbe8366b" />

Before the Splunk deployment, the Linux virtual machine was prepared with open-vm-tools — VMware's guest utilities — to ensure stable performance and reliable communication with the virtualization 

---

<img width="759" height="730" alt="adding splunk file for download" src="https://github.com/user-attachments/assets/4ba536ca-f8d6-4b26-86bc-7e937213114d" />
<img width="761" height="730" alt="File Path to splunk folder" src="https://github.com/user-attachments/assets/00208c25-5dd5-4317-8d87-1dfcd19e5895" />

In VMware's settings, I enabled Shared Folders, a feature that bridges my computer and the virtual machine so files can be passed directly between them. This is how the Splunk installer gets transferred onto the Linux server, as shown in the two photos above. 

---

<img width="903" height="251" alt="Creating a directory to install splunk from shared folder" src="https://github.com/user-attachments/assets/6acce36a-b58f-4d9f-a8cb-520d8d6427af" />

After enabling Shared Folders in VMware, I connected it inside the Linux server using the commands below. The Splunk folder from my host computer is now visible on the server.

`sudo mkdir /mnt/hgfs` — Creates a new folder for the shared files.

`sudo mount ...` — Links the shared folder to the file location.

`cd /mnt/hgfs and ls` — The cd command changes the folder directory, and the ls command lists the files inside the directory. 

---

<img width="749" height="265" alt="Inside folder" src="https://github.com/user-attachments/assets/74d2ade4-632d-45fb-97bf-6f35d21a6b83" />

Inside the shared folder, I changed into the Splunk installer file and ran the installation command `sudo dpkg -i splunk*.deb` to install the software for Splunk.

---

<img width="622" height="213" alt="Download complete" src="https://github.com/user-attachments/assets/65d5769e-7fb7-4eef-bb18-b28d247e6453" />

Installing Splunk using `dpkg.` The first attempt failed because Linux filenames are case-sensitive. Correcting the case resolved the error, and the package installed successfully. 

---

<img width="826" height="383" alt="Users and groups belong to splunk" src="https://github.com/user-attachments/assets/182e187b-ab62-4ecf-a845-0aaf87b96583" />

Confirming the Splunk installation directory (/opt/splunk) is owned by a dedicated Splunk user and group, rather than root, which is a good security practice so Splunk runs with only the permissions it actually needs instead of full system privileges.

---

<img width="844" height="687" alt="Terms and conditons " src="https://github.com/user-attachments/assets/9af87055-84b4-44de-98d9-1a05607cdeb5" />

Reviewing and accepting Splunk's license agreement (y) to complete the installation.

---

<img width="713" height="180" alt="Installed and making splunk start up apon booting" src="https://github.com/user-attachments/assets/1e45a8f8-8f3e-407c-8c0c-b8782acb0173" />

Splunk is now accessible through its web interface at `192.168.10.10:8000`. After exiting the Splunk user session, boot-start is enabled so Splunk automatically launches under the splunk user any time the server restarts.

---

<img width="508" height="48" alt="WIll now run when booted " src="https://github.com/user-attachments/assets/416fcad5-3864-4de3-95ce-8d2dffceb20e" />

Confirming the boot-start init script was installed and configured to run automatically at boot.

---

<img width="1031" height="766" alt="Checking server access" src="https://github.com/user-attachments/assets/67340e46-c6f6-461b-be98-8b97e8289e0a" />

After installing Splunk on the Linux server, I switched to the Windows client to confirm that the Splunk interface was reachable at 192.168.10.10:8000 via web browser. 

---

<img width="941" height="787" alt="insatlling the UF for splunk" src="https://github.com/user-attachments/assets/dd30e44e-aa8d-4164-87a1-1fd73c346057" />

After configuring the Splunk server, the next step is to install Universal Forwarder for Splunk. This is a lightweight agent installed as the central hub that stores and analyzes data behind the scenes, collecting each machine's logs and sending them back to the Splunk server to anaylsis. 

---

<img width="861" height="632" alt="Installer popup" src="https://github.com/user-attachments/assets/93ef9747-fdab-4ff7-8ada-55b00ed60bda" />

---

<img width="793" height="584" alt="image" src="https://github.com/user-attachments/assets/aecb1333-6a42-46ea-b5a0-4f5060be7768" />

Running the Universal Forwarder installer (shown in the screenshot above) launches a setup wizard. In the setup wizard, I pointed the forwarder to the Splunk server using the static IP `192.168.10.10`and using `port 9997`, which is the standard port Splunk uses to receive logs. This tells the forwarder where to send data that is being collected. 

---

<img width="816" height="417" alt="image" src="https://github.com/user-attachments/assets/ad5b2e38-5317-4907-801e-c5d089291ef3" />

Next, Sysmon will be installed. Sysmon is a free Microsoft tool that enhances Windows' basic logging to record detailed activity, such as program launches, network connections, and file changes. The Universal Forwarder then ships logs to the Splunk server. 

---

<img width="1027" height="761" alt="Symon olaf to with splunk" src="https://github.com/user-attachments/assets/0079951d-cad6-4505-a40a-6b53536ea53c" />

Sysmon needs a configuration file telling it which events to record, so rather than writing one from scratch, there is an industry-trusted Sysmon-Modular configuration by Olaf Hartong. This open-source community-maintained project acts as a preconfiguration for Sysmon. This captures the telemetry needed to detect real-time activities to watch for and log to ensure the right visibility to hunt for threats.

---

<img width="661" height="317" alt="Sysmon installed success " src="https://github.com/user-attachments/assets/e392f58e-8fbf-4945-8f90-0b54eeb4d58e" />

<img width="770" height="138" alt="Command to install Sysmon using powershell" src="https://github.com/user-attachments/assets/e577c5f0-8cbb-4d89-8f59-63ed28760965" />

With the configuration file downloaded, I installed the Sysmon config file by running `PS C:\Sysmon.exe -i ..\sysmonconfig.xml` in PowerShell as administrator. The `-i flag` is used to install Sysmon as a Windows service and applies the Olaf config that dictates what to monitor. The output confirms that Sysmon is now running and monitoring any changes in the background.

---

<img width="807" height="617" alt="Inputs conf file for slunk server" src="https://github.com/user-attachments/assets/5cb48341-2005-4cd6-bde6-395d8bb2bbdb" />

<img width="799" height="613" alt="Config file saved in the local file for splunk UF" src="https://github.com/user-attachments/assets/757b1f3a-27e3-44db-8f07-d151408345c6" />

The next step is telling the Universal Forwarder which logs to send to Splunk, and that's controlled by a file called `inputs.conf.` The first screenshot shows the default version file that is used with the forwarder that is located in the program's `default folder`. The second screenshot, a `new inputs.conf` created in the `local folder`. The reason you want to create a new config file in the `local folder` is so you can fall back on it if a configuration mistake is ever made. The new config file collects  logs from Application, Security, System, and Sysmon events.

<img width="857" height="605" alt="In services tab to chance to local host for logs to save " src="https://github.com/user-attachments/assets/e115a6f0-48df-476b-9ba1-5fddd519d399" />

After saving the `new config file`, I opened Windows Services and adjusted the SplunkForwarder service to log on as the Local System account. By default, the forwarder runs under a limited account that can't read protected logs like Security events. Switching to `Local System` gives the service the permissions it needs to access all the logs specified in `inputs.conf` and forward them to Splunk.

---

<img width="752" height="513" alt="restarting service to update it " src="https://github.com/user-attachments/assets/4bc1695c-ef41-49e0-a095-e7a431c1a0c9" />

Restarting the SplunkForwarder service is required for it to load the new configuration and permissions. Once restarted, it begins collecting logs and starts forwarding them to Splunk. 

---

<img width="1021" height="743" alt="Login to Splunk " src="https://github.com/user-attachments/assets/b4829e97-3393-4ad8-afcf-635bec7e90e1" />

Logged in to Splunk's main page, which is where the SIEM console is, where all forwarded logs arrive, and where security analysis happens.

---

<img width="1005" height="706" alt="Creating new index named ENDPOINT" src="https://github.com/user-attachments/assets/10d59a1a-fa72-4949-ad3d-3376520379c9" />

Creating a new index called endpoint (Settings → Indexes → New Index). An index is where Splunk stores incoming data and the forwarder's `inputs.conf` sends all logs to an index named "endpoint". This must exist for the data to arrive. 

---

<img width="1012" height="74" alt="Endpoint created" src="https://github.com/user-attachments/assets/ce039570-28b8-426b-8591-78ff5f794c8e" />

The new index now shows up as the named endpoint that was created.

---
<img width="1214" height="512" alt="image" src="https://github.com/user-attachments/assets/ba543ab4-f24f-463a-91fb-6673362fe60d" />

---
<img width="1251" height="322" alt="image" src="https://github.com/user-attachments/assets/e94a2750-a199-436f-99f5-43cbe01b7f73" />

---
<img width="1003" height="506" alt="Config of port for Splunk" src="https://github.com/user-attachments/assets/e22f9737-9ea5-489e-9292-8f2ebf2389c3" />

The final piece is telling Splunk to listen for incoming data. In `Settings → Forwarding and receiving`, I clicked `Configure receiving`, and since no port existed yet, I used port 9997 for the forwarder to send to. With both sides matching, the pipeline is complete.


---
<img width="1278" height="357" alt="image" src="https://github.com/user-attachments/assets/154c6e5e-5244-4401-ae68-a17d934a5cb7" />

---
<img width="1034" height="752" alt="Slunk is working " src="https://github.com/user-attachments/assets/c3dffa6f-237d-4f97-8bf8-8bda046b1d6d" />

---
<img width="824" height="829" alt="image" src="https://github.com/user-attachments/assets/28d3464b-5cbb-4aa7-ba39-72c63d1ecc59" />

With everything configured, I verified the index by going to `Apps → Search & Reporting` and searching `index=endpoint`. The results show 1,596 events from the client machine (Target-PC), with the source field confirming `Security`, `Application`, `System`, and `Sysmon logs` data being collected. The same setup was then repeated on the Windows Server machine.













