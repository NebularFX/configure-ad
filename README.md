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

<p>
<img src="https://i.imgur.com/Xvmv7Dk.png" height="80%" width="80%" alt="VM Settings"/>
</p>
<p>
1234132 Server 2022 with at least 2 vcpus (make sure you use at least 2 otherwise it'll be slow).
</p>
<br />

<p>
<img src="https://i.imgur.com/Xvmv7Dk.png" height="80%" width="80%" alt="VM Settings"/>
</p>
<p>
1234123pus (make sure you use at least 2 otherwise it'll be slow).
</p>
<br />
