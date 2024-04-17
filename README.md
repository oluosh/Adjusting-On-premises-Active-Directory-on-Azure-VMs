<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial details how to set up an on-premises Active Directory environment using Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>

<p>
</p>
<p>
In this lab, we'll establish two VMs within the same VNET. One VM will function as a Domain Controller, while the other will serve as a Client machine. We'll configure the Domain Controller with a static IP address since it provides Active Directory services to the client machine. Next, we'll join the Client machine to the domain and manage its DNS settings to ensure it utilizes the Domain Controller as its DNS server.
</p>
<br />

<p>
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
DC-1 requires a static private IP address. To verify connectivity, Client-1 will attempt to ping DC-1. Initially, the ping will fail. To resolve this issue, we need to enable ICMPv4 on the firewall of DC-1. Once this is done, Client-1 will successfully ping DC-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Next, we'll log back into DC-1 to install Active Directory Users and Computers. We'll then promote the VM to a Domain Controller and configure a new forest named "mydomain.com". After completing these steps, we'll restart the VM and log back in as the user "mydomain.com\labuser". If the steps were executed correctly, you should be able to access and run Active Directory Users and Computers as demonstrated below.
</p>
<img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
Great! We can now begin creating Organizational Units (OUs). First, let's create an OU named "_EMPLOYEES" and another one named "_ADMINS". To do this, right-click on the domain area, select "New" -> "Organizational Unit", and fill out the required fields. Then, navigate inside your newly created OU, right-click, select "New", choose "User", and fill in the details for the new user. The user, named Jane Doe, will hold an administrative role, so her username should be "Jane_admin". Finally, add Jane to the "Domain Admins" security group.
</p>
<img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
Going forward, we'll utilize "Jane_admin" as the administrator account. Now, we'll proceed to join Client-1 to the domain (mydomain.com). From the Azure portal, we'll adjust Client-1's DNS settings to point to the DC's private IP address. After making this change, restart Client-1 from within the Azure portal. The image below provides confirmation that Client-1 is using DC-1's DNS.
</p>
<img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<p>
To join Client-1 to the domain, navigate to your system settings and click on "About". Then, select "Rename this PC (advanced)" on the right side. Next, choose to change the domain and enter "mydomain.com". Provide your credentials from "mydomain.com\labuser". After that, your computer will restart, and Client-1 will become a part of "mydomain.com".
</p>
<br />
<p>
  <p>
<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Wonderufl Client-1 is now a part of the domain. Now we will set up remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open system properties. Click on "Remote Desktop", allow "domain users" access to remote desktop. After completing those steps you should be able to log into Client-1 as a normal user.
</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally, to confirm that regular users can RDP into Client-1, we'll utilize a script to generate thousands of users within the domain. We'll input the script into PowerShell, and once the users are created, we'll select one and initiate an RDP session into Client-1.
</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
The PowerShell script has successfully generated a user with the username "bab.hubo". Using these credentials, we were able to log in to Client-1 as a regular user.
</p>
