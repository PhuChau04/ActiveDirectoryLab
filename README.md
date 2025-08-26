
<h1>Active Directory Home Lab</h1>
<h2>Description</h2>
Active Directory (AD) is a directory service developed by Microsoft, allowing users to manage and organize users, computers, and other resources on a Windows network. In this lab we will be building a small Windows server in Oracle VirtualBox, a virtualization software used to create and run virtual machines. At the end of this, we will have a working server with 1000+ users.
<br />
<br />

<p align="center">
<img width="750" height="400" alt="image" src="https://github.com/user-attachments/assets/6ddd4942-4b22-49aa-b1af-7060366a70c9" />

<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b>
- **Oracle VirtualBox**

<h2>Environments Used </h2>

- <b>Windows 10</b>
- <b>Windows Server 2019</b>

<h2>Prerequisites</h2>

- <b>Windows Server 2019 Installation Media:</b> Can be obtained on Microsoft’s official website and will be used to set up the domain controller. [Windows 2019](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019)
- <b>Virtualization Platform:</b> For a controlled testing environment, platforms like VirtualBox or Hyper-V. [Oracle VirtualBox Download](https://www.oracle.com/virtualization/technologies/vm/downloads/virtualbox-downloads.html)
- <b>System Requirements:</b> Ensure your hardware meets the [minimum requirements](https://learn.microsoft.com/en-us/windows-server/get-started/hardware-requirements?tabs=cpu&pivots=windows-server-2019) for Windows Server 2019
- <b>Create Accounts Script (Optional):</b> This script will automate the creation of about 1000 users using  PowerShell. Created by Josh Madakor ([Script](https://github.com/joshmadakor1/AD_PS/archive/master.zip)). Copy the link into the internet explorer of the DC to download it on the DC VM.

<h2>Lab walk-through:</h2>

<h3>Step 1 (Setting up our domain controller):</h3>
<p align="center">
<img width="750" height="4000" alt="image" src="https://github.com/user-attachments/assets/684fd056-b2b0-48c9-a916-c64b034c5c2d" />
<br />
<br />
  
1.	<ins>Creating our domain controller (DC):</ins>
- Open virtualization platform and create a new VM
- Allocate appropriate resources (4GB RAM, 50GB storage, can increase CPU cores for faster processing)
- Attach Windows Server ISO to VM
-	Go to properties and enable 2nd network adapter for the internal network (see [Figure 1](https://github.com/user-attachments/assets/f289e889-467c-486b-9f5b-910ac4100e39))
-	Custom install either “desktop experience" Windows version (see [Figure 2](https://github.com/user-attachments/assets/3cfa5a11-08b7-4ea1-82b7-7a24ad2a667c))

<p align="center">

<br />
<img width="750" height="400" alt="Figure 1" src="https://github.com/user-attachments/assets/f289e889-467c-486b-9f5b-910ac4100e39" />
<p align="center">
 Figure 1: Enable 2nd Adapter for internal network
<br />
<br />

<br />
<img width="750" height="400" alt="Figure 2" src="https://github.com/user-attachments/assets/3cfa5a11-08b7-4ea1-82b7-7a24ad2a667c" />
<p align="center">
 Figure 2: Installing a non-desktop experience only gives us a command line
<br />
<br />

2. <ins>Setting up IP addressing:</ins>
<p align="center">
<img width="750" height="400" alt="image" src="https://github.com/user-attachments/assets/85c55518-1d69-4f92-ab3c-de5dddac6808" />
<br />
The NIC that is on the internet will use DHCP to assign an IP address automatically. However, we will have to assign the IP address for the internal NIC manually.
 <br />
  
- Open up network & internet settings and click on change adapter options (see [Figure 3](https://github.com/user-attachments/assets/d1c9526c-c640-4b4a-876f-5c4122a7e055))
  
- Identify which NIC is the external/internal. The external one will have received packets, and when accessing Network Connection Details, it would also have a “10.” something IP address (see [Figure 4](https://github.com/user-attachments/assets/fd8bd2f6-a052-4039-b1a6-b5694c4ade18)). You can rename these accordingly.
  
- In that same window go to your internal NIC’s properties and change the IP address. Double click “Internet Protocol Version 4” to change IP address (see [Figure 5](https://github.com/user-attachments/assets/c198263a-9b76-4069-a3af-f6a76e7b028c))
<br />

<p align="center">
<img width="750" height="400" alt="Figure 3" src="https://github.com/user-attachments/assets/d1c9526c-c640-4b4a-876f-5c4122a7e055" />
<p align="center">
 Figure 3: Click on change adapter options to see the two NICs
<br />
<br />

<img width="750" height="400" alt="Figure 4" src="https://github.com/user-attachments/assets/fd8bd2f6-a052-4039-b1a6-b5694c4ade18" />
<p align="center">
 Figure 4: In this case, my "Ethernet" NIC is my external
<br />
<br />

<img width="750" height="400" alt="Figure 5" src="https://github.com/user-attachments/assets/c198263a-9b76-4069-a3af-f6a76e7b028c" />
<p align="center">
 Figure 5: Setting IP address manually. Following the diagram and addresses provided.
<br />
<br />

3. <ins>Install Active Directory Domain Services and Create Domain:</ins>
<p align="center">
<img width="750" height="400" alt="image" src="https://github.com/user-attachments/assets/6110dac9-647f-4111-8ff4-1a57fcf4f7ff" />
<br />
  
- In the server manager, click “Add roles and features” and click next until you get to server roles and install “Active Directory Domain Services” (see [Figure 6](https://github.com/user-attachments/assets/bed9d567-9c64-435f-a831-d29040e43c27))
  
- Promote the server to a domain controller using the “Add new forest” deployment configuration. It can be named whatever you’d like and use any password you’d like. Leave the rest default and install (see Figures [7](https://github.com/user-attachments/assets/e6cde47d-9ae6-4edc-9e02-b9bafc1e332d) and [8](https://github.com/user-attachments/assets/dee373ef-97ed-4f63-bfdd-c8b4abe67e88))
  
- After logging in again, you can create a new admin user instead of the built-in admin account. Search for “Active Directory Users and Computers” and in your domain, create a new organizational unit (can be done under Action-New-Organizational Unit dropdown) for Admins. (see [Figure 9](https://github.com/user-attachments/assets/bebf0b6a-d2d2-47c9-9939-ac4a8d950f28))

- In new OU create new user and name it whatever and use a conventional naming convention for logon name (e.g. a-firstintiallastname) (see [Figure 10](https://github.com/user-attachments/assets/67ab57cc-5baa-42e2-b8e0-31ccc602fc77)). Now make this new account an admin by adding it to a new member of “Domain Admins” in the account properties (see [Figure 11](https://github.com/user-attachments/assets/d0055dd5-6966-4be7-8144-33951916b080)). Login to new account (see [Figure 12](https://github.com/user-attachments/assets/bf1ad47b-64e7-4c74-8182-51bcc1cc1524))

<p align="center">
<img width="750" height="400" alt="Figure 6" src="https://github.com/user-attachments/assets/bed9d567-9c64-435f-a831-d29040e43c27" />
<p align="center">
 Figure 6: Install Active Directory Domain Services
<br />
<br />

<img width="750" height="400" alt="Figure 7" src="https://github.com/user-attachments/assets/e6cde47d-9ae6-4edc-9e02-b9bafc1e332d" />
<p align="center">
 Figure 7: Click on the flag and promote server to a domain controller
<br />
<br />
  
<img width="750" height="400" alt="Figure 8" src="https://github.com/user-attachments/assets/dee373ef-97ed-4f63-bfdd-c8b4abe67e88" />
<p align="center">
 Figure 8: Deploying new domain
<br />
<br />
  
<img width="750" height="400" alt="Figure 9" src="https://github.com/user-attachments/assets/bebf0b6a-d2d2-47c9-9939-ac4a8d950f28" />
<p align="center">
 Figure 9: New OU for Admins
<br />
<br />
  
<img width="750" height="400" alt="Figure 10" src="https://github.com/user-attachments/assets/67ab57cc-5baa-42e2-b8e0-31ccc602fc77" />
<p align="center">
 Figure 10: Create new user
<br />
<br />
  
<img width="750" height="400" alt="Figure 11" src="https://github.com/user-attachments/assets/d0055dd5-6966-4be7-8144-33951916b080" />
<p align="center">
 Figure 11: Add new account to admins
<br />
<br />
  
<img width="750" height="400" alt="Figure 12" src="https://github.com/user-attachments/assets/bf1ad47b-64e7-4c74-8182-51bcc1cc1524" />
<p align="center">
 Figure 12: Login to new account
<br />
<br />

4. <ins>Installing Remote Access Server and Network Address Translation<ins/>
<p align="center">
<img width="750" height="400" alt="image" src="https://github.com/user-attachments/assets/8a9e0a7a-d36d-4979-81fa-b532ab4a5b17" />
<br />
<p align="center">
This will enable our clients to access the internet via the domain controller while being on a private virtual network.
  
- First, we’re going to install Remote Access in server roles. This is done exactly like how we installed AD DS, but we will be choosing “Remote Access” this time (see [Figure 13](https://github.com/user-attachments/assets/07179fdf-9757-4b6c-81ee-c08bae4338c4)). Add Routing and DirectAccess and VPN (RAS) to the Remote Access role services when prompted. Everything else defaults
  
- After installing remote access we must configure NAT and that is done using the “Routing and Remote Access” tool (see [Figure 14](https://github.com/user-attachments/assets/63c0d31a-4c65-436c-aff1-d04fef1f4e22)). Now in the Routing and Remote Access Server Setup Wizard choose NAT as the configuration and choose your external NIC that connects to the internet (see Figures [15](https://github.com/user-attachments/assets/b33965c0-78b2-4251-9e4d-a714b485896f) and [16](https://github.com/user-attachments/assets/024f35fc-24d8-4605-81e3-b3c47e26d591)). If the top option is grayed out when choosing a public interface, restart the wizard ([Figure 16](https://github.com/user-attachments/assets/024f35fc-24d8-4605-81e3-b3c47e26d591)).

<p align="center">
<img width="750" height="400" alt="Figure 13" src="https://github.com/user-attachments/assets/07179fdf-9757-4b6c-81ee-c08bae4338c4" />
<p align="center">
 Figure 13: Install remote access and add Routing and DirectAccess and VPN (RAS) to role services for Remote Access when prompted
<br />
<br />
  
<img width="750" height="400" alt="Figure 14" src="https://github.com/user-attachments/assets/63c0d31a-4c65-436c-aff1-d04fef1f4e22" />
<p align="center">
 Figure 14: Open Routing and Remote Access menu to configure NAT
<br />
<br />
  
<img width="750" height="400" alt="Figure 15" src="https://github.com/user-attachments/assets/b33965c0-78b2-4251-9e4d-a714b485896f" />
<p align="center">
 Figure 15:Choose NAT
<br />
<br />
  
<img width="750" height="400" alt="Figure 16" src="https://github.com/user-attachments/assets/024f35fc-24d8-4605-81e3-b3c47e26d591" />
<p align="center">
 Figure 16: Pick your external NIC used to access the internet
<br />
<br />

5. <ins>Setting up DHCP<ins/>
<p align="center">
<img width="750" height="400" alt="image" src="https://github.com/user-attachments/assets/a0ad8467-1026-4428-9b0f-1361bca6e01b" />
<p align="center">
Allows our clients to get an IP address from the DC and browse the Internet. The scope is the range of IP addresses that will be given

- Add DHCP Server role to the server, same steps as when we installed AD DS and RAS, click next to all and install (see [Figure 17](https://github.com/user-attachments/assets/c8c1f2d0-49b8-4ed3-bde3-4994d73a72d9))
  
- Access DHCP control panel in the tools section of the Server Manager (see [Figure 18](https://github.com/user-attachments/assets/edcd136b-7e13-4b77-9200-ca301e5f89a4))
  
- Create a new scope and name it; the name can be anything, but in this lab, we’re going to name it after the scope (see [Figure 19](https://github.com/user-attachments/assets/ada61f1c-2c09-4492-828d-4807f3ce4016)). Define the range in the Wizard (see [Figure 20](https://github.com/user-attachments/assets/f355d1f4-174b-4059-9ca3-f315f7d4e876)).
  
- Click Yes until you get to the Router (Default Gateway) window. We do not want to exclude any IP addresses in this lab, and with this being a lab, Lease Duration does not matter. Lease Duration is the time a computer can have the IP address before it has to be refreshed, this duration depends on your use case.
  
- Add the IP address 172.16.0.1 as our default gateway. This will then route the request of the client to our DC allowing them access to the Internet. Do not forget to click add (see [Figure 21](https://github.com/user-attachments/assets/a7bf4285-7f98-435a-ae87-5a1d55281e76)). Then click through the Wizard until finished
  
- Right click the DC, authorize and refresh it 

<p align="center">
<img width="750" height="400" alt="Figure 17" src="https://github.com/user-attachments/assets/c8c1f2d0-49b8-4ed3-bde3-4994d73a72d9" />
<p align="center">
 Figure 17: Install DHCP Server
<br />
<br />

<img width="750" height="400" alt="Figure 18" src="https://github.com/user-attachments/assets/edcd136b-7e13-4b77-9200-ca301e5f89a4" />
<p align="center">
 Figure 18:DHCP control
<br />
<br />

<img width="750" height="400" alt="Figure 19" src="https://github.com/user-attachments/assets/ada61f1c-2c09-4492-828d-4807f3ce4016" />
<p align="center">
 Figure 19: Create new scope then name it
<br />
<br />

<img width="750" height="400" alt="Figure 20" src="https://github.com/user-attachments/assets/f355d1f4-174b-4059-9ca3-f315f7d4e876" />
<p align="center">
 Figure 20: Define range and subnet mask
<br />
<br />

<img width="750" height="400" alt="Figure 21" src="https://github.com/user-attachments/assets/a7bf4285-7f98-435a-ae87-5a1d55281e76" />
<p align="center">
 Figure 21: Add gateway
<br />
<br />

<h3>Optional Step (Creating users with Script):</h3>
<br />

- Paste the link of the script into Internet Explorer on the DC VM and save it on the desktop, then extract the folder AD_PS-master to desktop (see [Figure 22](https://github.com/user-attachments/assets/7e5056df-607f-46e7-a0e5-4ebc3988afb9)).

- You can add your name to the top of the “names” text file so that an account can also be created for you (see [Figure 23](https://github.com/user-attachments/assets/ceea8311-75b6-45c5-91de-a3cd3e54b7b1)).
  
- Run Windows PowerShell ISE as an administrator

- Click file-open and open up the “1_CREATE_USERS.ps1” script found in the folder of AD_PS-master.
  
- For this script to run and for lab purposes, we’re going to set the execution policy to unrestricted (see [Figure 24](https://github.com/user-attachments/assets/2cb7502f-eafa-4716-91ed-65a8d1caa4fe))
  
- Change directory to where the script is located (e.g. C:\users\a-pchau\Desktop\AD_PS-master) (see [Figure 25](https://github.com/user-attachments/assets/d2389ca5-3fd1-4b47-b42c-44f0b3861d75)). Check that you are is the right directory using “ls”, this should show that the “names” text file and the PowerShell script are there. If you see AD_PS-master only, then change directory to (C:\users\a-pchau\Desktop\AD_PS-master\AD_PS-master\AD_PS-master\AD_PS-master\ AD_PS-master) and it should work. Of course, replacing “a-pchau” with the account you’re logged into. (see [Figure 26](https://github.com/user-attachments/assets/5a702c9d-7f42-4ac4-8c3b-0403c398cc43))
  
- Run the script
  
- Check results of the script in Active Directory Users and Computers in the (see [Figure 27](https://github.com/user-attachments/assets/e613aea4-9d13-4b67-9207-d4bcfbc5a770))

<p align="center">
<img width="750" height="400" alt="Figure 22" src="https://github.com/user-attachments/assets/7e5056df-607f-46e7-a0e5-4ebc3988afb9" />
<p align="center">
 Figure 22: Save script on desktop then extract
<br />
<br />

<img width="750" height="400" alt="Figure 23" src="https://github.com/user-attachments/assets/ceea8311-75b6-45c5-91de-a3cd3e54b7b1" />
<p align="center">
  Figure 23: Add your name
<br />
<br />

<img width="750" height="400" alt="Figure 24" src="https://github.com/user-attachments/assets/2cb7502f-eafa-4716-91ed-65a8d1caa4fe" />
<p align="center">
  Figure 24: Set Execution policy to unrestricted
<br />
<br />

<img width="750" height="400" alt="Figure 25" src="https://github.com/user-attachments/assets/d2389ca5-3fd1-4b47-b42c-44f0b3861d75" />
<p align="center">
 Figure 25: Change directory to where the script is before running
<br />
<br />

<img width="750" height="400" alt="Figure 26" src="https://github.com/user-attachments/assets/5a702c9d-7f42-4ac4-8c3b-0403c398cc43" />
<p align="center">
 Figure 26: This should be the result of "ls" command
<br />
<br />

<img width="750" height="400" alt="Figure 27" src="https://github.com/user-attachments/assets/e613aea4-9d13-4b67-9207-d4bcfbc5a770" />
<p align="center">
  Figure 27:Result of the Script
<br />
<br />

<h3>Step 2 (Setting up client): (</h3>
<br />
<p align="center">
<img width="750" height="400" alt="image" src="https://github.com/user-attachments/assets/9de11304-d30b-4560-9f51-6a8770918303" />
<br />

1. <ins>Creating a new client</ins>

- Open virtualization platform and create a new VM

- Allocate appropriate resources (4GB RAM, 50GB storage, can increase CPU cores for faster processing)

- Attach Windows 10 ISO to VM

- Go to properties and change the network adapter to internal network (see [Figure 28](https://github.com/user-attachments/assets/1c701f52-1048-4774-a773-2440de4aad83))

- Launch and Custom install “Windows 10 Pro" without a product key

- Finish setting up windows, skip unnecessary things. Use the username “user” and click next on password

- Check if the internet is working using the command prompt. Use command “ipconfig” to check that the computer is given an IP address in the scope we defined and that the default gateway is the IP address of the DC. Then ping a website like www.google.com. (see [Figure 29](https://github.com/user-attachments/assets/27f53bd2-bc1a-4713-add0-62a91396aca1))

- Rename the PC and join the domain. In System setting navigate to “about” section and click on “Rename this PC (advanced)” then click “Change” top rename computer and change the name and add it to a member of your domain (see [Figure 30](https://github.com/user-attachments/assets/b42d650f-2b0d-4a53-a576-3b37cd122859)), You can use the login of the domain admin account you created to confirm changes or your normal account created by the script.

<p align="center">
<img width="750" height="400" alt="Figure 28" src="https://github.com/user-attachments/assets/1c701f52-1048-4774-a773-2440de4aad83" />
<p align="center">
Figure 28: Change to Internal Network
<br />
<br />


<img width="750" height="400" alt="Figure 29" src="https://github.com/user-attachments/assets/27f53bd2-bc1a-4713-add0-62a91396aca1" />
<p align="center">
Figure 29: Check that internet works
<br />
<br />

<img width="750" height="400" alt="Figure 30" src="https://github.com/user-attachments/assets/b42d650f-2b0d-4a53-a576-3b37cd122859" />
<p align="center">
Figure 30: Changing computer name and joining domain at the same time
<br />
<br />



