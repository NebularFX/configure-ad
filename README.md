<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<h3>Setup Resources in Azure</h3>

<p>
<img src="https://i.imgur.com/Xvmv7Dk.png" height="80%" width="80%" alt="VM Settings"/>
</p>
<p>
Let's start this off by creating a virtual machine in Azure. Here you can see I created a new resource group (AD-Lab), named the VM (DC-1), and used Windows Server 2022 with at least 2 vcpus (make sure you use at least 2 otherwise it'll be slow).
</p>
<br />

<p>
<img src="https://i.imgur.com/ydYE1iw.png" height="80%" width="80%" alt="NIC settings"/>
</p>
<p>
<img src="https://i.imgur.com/3UvROEL.png" height="80%" width="80%" alt="NIC settings"/>
</p>
<p>
Now we'll set the IP configuration to a static IP address. Just go to the newly created VM and follow what I have highlighted. This will allow you to change the address from dynamic to static. 
</p>
<br />

<p>
<img src="https://i.imgur.com/jrmDxRo.png" height="80%" width="80%" alt="VM settings"/>
</p>
<p>
<img src="https://i.imgur.com/Wk64agZ.png" height="80%" width="80%" alt="VM settings"/>
</p>
<p>
Create another VM, but this time use Windows 10 and name it "Client-1". Go to the network tab and make sure you are using the same Virtual Network (vnet) as the one that DC-1 is using (DC-1-vnet).
</p>
<br />

<h3>Ensure Connectivity between the Client and Domain Controller</h3>

<p>
<img src="https://i.imgur.com/iBHTcmA.png" height="80%" width="80%" alt="Ping the domain controller"/>
</p>
<p>
This step is optional, but I'm doing it just to show prove that we can connect to the domain controller. Form a Remote Desktop connection with both Client-1 and DC-1 by using the their respective public IP addresses found in the Azure Portal. Open up command prompt on Client-1 and type "ping -t" followed by the private IP address of DC-1. You'll notice that our requests are timing out and that it will continuously ping DC-1 until we stop it.
</p>
<br />

<p>
<img src="https://i.imgur.com/GOJSm9O.png" height="80%" width="80%" alt="Firewall Settings"/>
</p>
<p>
<img src="https://i.imgur.com/rZPwHJj.png" height="80%" width="80%" alt="Enable ICMPv4 Inbound Traffic"/>
</p>
<p>
We are going to go to DC-1 to allow these packets through the firewall. Begin by typing "firewall" in the search bar and click on the one I have selected above. Then go to inbound rules and filter the list by protocol and find the rules that utilize ICMPv4. Here you will find the two options pictured above that you need to right-click and enable. 
</p>
<br />

<p>
<img src="https://i.imgur.com/oDlXX9H.png" height="80%" width="80%" alt="Response from Domain"/>
</p>
<p>
Once you've completed that, you will see a response from DC-1 in Client-1's command prompt.
</p>
<br />



<h3>Install Active Directory</h3>

<p>
<img src="https://i.imgur.com/YKjfd1W.png" height="80%" width="80%" alt="Server Manager to Install AD"/>
</p>
<p>
<img src="https://i.imgur.com/Sbveb3A.png" height="80%" width="80%" alt="Install AD"/>
</p>
<p>
Now we can focus on installing Active Directory. First, open the DC-1 VM. You can open up command prompt and type hostname to quickly differntiate between both VMs. Head over to the server manager which can be found through the search bar or on the task bar. Next click on add roles and features. Here you will pretty much just click next until you get to server roles. This is where you will click on "Active Directory Domain Services". Just click next for the rest and install to finish.
</p>
<br />

<p>
<img src="https://i.imgur.com/priIfMu.png" height="80%" width="80%" alt="Promote server to domain controller"/>
</p>
<p>
<img src="https://i.imgur.com/SxZFeHV.png" height="60%" width="60%" alt="Root Domain Name"/>
</p>
<p>
In order to finish installing AD, we need to click on promote server to domain controller and then add a new forest where you will set your desired domain name.
</p>
<br />

<p>
<img src="https://i.imgur.com/6yeflKg.png" height="80%" width="80%" alt="DSRM Password"/>
</p>
<p>
You need to set a password here, but it won't be used for our purposes. You simply need to click next for the rest of the steps and then click install. Once it has installed, you will get notified that you will be signed out. This means you will need to reestablish the connection after you are disconnected from RDP.
</p>
<br />

<p>
<img src="https://i.imgur.com/3k4raoi.png" height="60%" width="60%" alt="Sign in under the new domain"/>
</p>
<p>
When you try to sign back in, use the domain name you used followed by a backslash and the username (lab.com\labuser). The password is still the same.
</p>
<br />

<h3>Create an Admin and Normal User Account in AD</h3>

<p>
<img src="https://i.imgur.com/EOIk6jx.png" height="80%" width="80%" alt="Navigate to AD UandC"/>
</p>
<p>
Now we need to navigate to Active Directory Users and Computers
</p>
<br />

<p>
<img src="https://i.imgur.com/d4xCCv6.png" height="80%" width="80%" alt="Create OUs"/>
</p>
<p>
Here we will create two Organizational Units and name them "_EMPLOYEES" and "_ADMINS".
</p>
<br />

<p>
<img src="https://i.imgur.com/r1OnT8Y.png" height="80%" width="80%" alt="Create new Admin"/>
</p>
<p>
Here we will create a new user in the admin OU. So just click on the "_ADMINS" OU folder then right click and create a new user. Make sure that you don't forget the password you used. 
</p>
<br />

<p>
<img src="https://i.imgur.com/cHMFKuy.png" height="80%" width="80%" alt="Add the user to Domain Admins"/>
</p>
<p>
Now right-click on the user Jane and click "Add to a group...". Here you will type "domain" in the box and click "Check Names" and then click on Domain Admins and hit 'ok'.
</p>
<br />

<p>
<img src="https://i.imgur.com/xG6w2GM.png" height="60%" width="60%" alt="Sign in as jane_admin"/>
</p>
<p>
Close out of the RDP connection and sign back in using jane_admin. This is the Admin account we will use for the rest of the lab. 
</p>
<br />

<h3>Join Client-1 to your Domain</h3>

<p>
<img src="https://i.imgur.com/KL0LoGc.png" height="60%" width="60%" alt="Find DC-1 private ip"/>
</p>
<p>
<img src="https://i.imgur.com/5QXTjYC.png" height="80%" width="80%" alt="Update Client-1 DNS"/>
</p>
<p>
Head over to the azure portal and get the private IP address for DC-1. We will then navigate to the network settings of the Client-1 VM where we will click on the Network Interface. Here you will be able to access the DNS servers settings and change it to custom where you will insert the private IP of DC-1 and save the settings. 
</p>
<br />


<p>
<img src="https://i.imgur.com/6aW03Fn.png" height="80%" width="80%" alt="Restart Client-1 VM"/>
</p>
<p>
<img src="https://i.imgur.com/E7lVK1i.png" height="80%" width="80%" alt="Ipconfig dns servers"/>
</p>
<p>
Now we need to go to the Client-1 VM and restart it. Once it restarts and you login, you can open up command prompt and type "ipconfig /all". This will verify the address of the DNS Server address you setup in the azure portal.
</p>
<br />

<p>
<img src="https://i.imgur.com/51Hvb09.png" height="80%" width="80%" alt="Right click start menu"/>
</p>
<p>
<img src="https://i.imgur.com/UsH50uP.png" height="80%" width="80%" alt="Configure domain member"/>
</p>
<p>
In order to configure Client-1 as a workstation within the domain, we will right click the start menu and click on system. Here you will find "Rename this PC (advanced)" and click on "change" within the System Properties window. Here you can enter the domain name (lab.com). You will need to sign in under the jane_admin credentials.
</p>
<br />

<p>
<img src="https://i.imgur.com/3kp0aGw.png" height="60%" width="60%" alt="Configure domain member"/>
</p>
<p>
Once you have finished you will get this pop-up and be instructed to restart. Let it restart and sign in with lab.com\jane_admin.
</p>
<br />

<p>
<img src="https://i.imgur.com/XWkmkkQ.png" height="60%" width="60%" alt="Verify Client-1 in AD"/>
</p>
<p>
Now go back to DC-1 and open AD Users and Computers again. Navigate to Computers under lab.com and you should see that our client has joined the domain.
</p>
<br />

<h3>Setup Remote Desktop for Non-Administrative Users on Client-1</h3>

<p>
<img src="https://i.imgur.com/lRO2HB5.png" height="60%" width="60%" alt="Verify Client-1 in AD"/>
</p>
<p>
If we go back to Client-1 and go back to system settings as we did earlier, you will find "Remote Desktop". Inside of that window you will find User accounts where you will add "Domain Users" that can remotely access the pc. We will be creating a long list of non-administrative users that will have the ability to sign in to this client using RDP.
</p>
<br />

<h3>Create a bunch of additional users and attempt to log into client-1 with one of the users</h3>

<p>
<img src="https://i.imgur.com/9Vd2RGt.png" height="80%" width="80%" alt="Open Powershell"/>
</p>
<p>
<img src="https://i.imgur.com/7CRmKLl.png" height="80%" width="80%" alt="Running Powershell Script"/>
</p>
<p>
Sign back in to DC-1 using jane_admin if you haven't already. You will open Powershell ISE and run it as administrator. Once it opens you will create a new script and paste the code from this link (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1). Once you have done that you can run the script and watch the users being created. 
<p>
*note: I recommend changing the amount of users to 1000 or less to speed up the process.
</p>
</p>
<br />

<p>
<img src="https://i.imgur.com/LJ7V1uW.png" height="60%" width="60%" alt="Verify User Creation"/>
</p>
<p>
Go back to AD Users and Computers and you will find our list of users are being created within our _EMPLOYEES folder. 
</p>
<br />

<p>
<img src="https://i.imgur.com/JaXXLFn.png" height="60%" width="60%" alt="Sign in with newly created account"/>
</p>
<p>
Now we can open up a new RDP connection with Client-1 and use one of the new users we have created with our script. Simply choose a user within the _EMPLOYEES folder and use the password within the script (Password1) to sign in to Client-1 with that account. 
</p>
<br />

<p>
<img src="https://i.imgur.com/It0gyFl.png" height="60%" width="60%" alt="Whoami to show success"/>
</p>
<p>
That concludes this tutorial, I hope you enjoyed it.
</p>
<p>
Don't forget to clean up your Azure resources once you are done. Nobody likes a surprise bill at the end of the month!
</p>
<br />
