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
We need to start by creating a virtual machine. Here you can see I created a new resource group (AD-Lab), named the vm (DC-1), and used Windows Server 2022 with at least 2 vcpus (make sure you use at least 2 otherwise it'll be slow).
</p>
<br />

<p>
<img src="https://i.imgur.com/ydYE1iw.png" height="80%" width="80%" alt="NIC settings"/>
</p>
<p>
<img src="https://i.imgur.com/3UvROEL.png" height="80%" width="80%" alt="NIC settings"/>
</p>
<p>
Now we need to set the IP configuration to a static IP address. Just go to the newly created vm and follow what I have highlighted. This will get you to where you can change the address from dynamic to static. 
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
1323122131to start by creating a virtual machine. Here you can see I created a new resource group (AD-Lab), named the vm (DC-1), and used Windows Server 2022 with at least 2 vcpus (make sure you use at least 2 otherwise it'll be slow).
</p>
<br />

<p>
<img src="https://i.imgur.com/GOJSm9O.png" height="80%" width="80%" alt="Firewall Settings"/>
</p>
<p>
12321erver 2022 with at least 2 vcpus (make sure you use at least 2 otherwise it'll be slow).
</p>
<br />

<p>
<img src="https://i.imgur.com/oDlXX9H.png" height="80%" width="80%" alt="Response from Domain"/>
</p>
<p>
4321123rver 2022 with at least 2 vcpus (make sure you use at least 2 otherwise it'll be slow).
</p>
<br />

<p>
<img src="https://i.imgur.com/rZPwHJj.png" height="80%" width="80%" alt="Enable ICMPv4 Inbound Traffic"/>
</p>
<p>
432114rver 2022 with at least 2 vcpus (make sure you use at least 2 otherwise it'll be slow).
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
