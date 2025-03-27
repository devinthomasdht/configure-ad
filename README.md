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
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Install Active Directory
- Setup Domain Controller in Azure 
- Create a Domain Admin user within the domain
- Join Client-1 VM to the domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Attempt to logon using incorrect password and unlock user account

<h2>Deployment and Configuration Steps</h2>

<p>
<img width="1278" alt="Resource Group_ Active Directory Lab" src="https://github.com/user-attachments/assets/a904b334-d793-4d2d-907c-0c82bda1df78" />


</p>
<p>
Creating a Resource Group: The purpose of creating a resource group in Azure is to organize and manage related Azure resources (like virtual machines, storage accounts, etc.) as a single unit. It helps with efficient resource management, access control, and billing, while also enabling easier deployment and monitoring. This is vital to create the virtual machines needed for Active Directory and Domain Controller. 
</p>
<br />

<p>
<img width="1894" alt="Created dc-1 vm" src="https://github.com/user-attachments/assets/34f86e08-497d-4746-8a0a-ae99ae5c3f7f" />

</p>
<p>
Creat a Virtual Machine: I created a virtual machine with Windows Server 2022 named "DC-1". The purpose of this virtual machine is to set up Active Directory and Domain Controller from AD Domain Services. 
</p>
<br />

<p>
<img width="938" alt="Set DC-1 to static IP so it would not change, so client-1 can recognize it as DNS as well" src="https://github.com/user-attachments/assets/191f34e1-aa14-46e0-95ee-95cba32481b4" />  
<img width="1502" alt="Disabled the domain,private, and public profile in firewall to ping for DNS from client-1" src="https://github.com/user-attachments/assets/ad4f08c3-ed76-43c3-8d0a-0aed4fa5f152" />


</p>
<p>
Setup static IP: We want the IP address of the virtual machine to be static so the other virtual machine we create can recognize this IP address as DNS and join the Domain. In order for this to happen the virtual machine needs to resolve names on the domain created and not the real world server (Azure). Also logged into VM and disabled Windows Firewall.
</p>
<br />

<p>
<img width="1855" alt="Created client-1 vm" src="https://github.com/user-attachments/assets/7a7323d8-e7bb-4acf-a0bd-5ffb9880b650" />
<img width="1279" alt="DNS settings for Client-1" src="https://github.com/user-attachments/assets/94bb34ec-7aaa-42f8-bde2-91f79ff67d1a" />


</p>
<p>
Created a Virtual Machine: I created a virtual machine and with Windows 10 PRO named "Client-1". The purpose of this virtual machine is to use as Admin or User. Also to join this VM to the Domain "DC-1" which is mydomain.com. In order for this to happen we need to set this VM's DNS settings to "DC-1's" private IP address.
</p>
<br />

<p>
<img width="1364" alt="DNS servers shows static IP to dc-1" src="https://github.com/user-attachments/assets/5e6163ab-2cc0-4cae-a65b-1c9edd73332b" />

</p>
<p>
Ping and Ipconfig /all: To verify "DC-1" and "Client-1" are on the same Virtual Network, in PowerShell we use ping to reach a host. In this example "DC-1's" IP address is 10.0.0.4 and we use command "ping 10.0.0.4 in the PowerShell". If it does not work, make sure the two VM's are on the same Virtual Network and "DC-1's" firewall is disabled. The use of ipconfig /all is to view comprehensive network configuration information. In this case we want to confirm the DNS server to show "DC-1's" private IP address.
</p>
<br />

<p>
<img width="1520" alt="Installilng Acitve directory Domain Services" src="https://github.com/user-attachments/assets/958d57b8-44a7-4fea-bd9a-aa1315401d4b" />
<img width="1502" alt="Installing Active directory Domain Service p2" src="https://github.com/user-attachments/assets/8aa82809-a489-46fb-9ee5-2c228a5a0ab3" />
<img width="1385" alt="configuring AD as a Domain Controller, mydomain com" src="https://github.com/user-attachments/assets/f676300c-cdb5-4213-84f1-ea142ca3de24" />

</p>
<p>
Install Active Directory: Open Server Manager > Add Roles and Features > In wizard, select Role-based or feature-based installation > Select server from the server pool (should be auto populated) > Next > Under Roles, check Active Directory Domain Services > Follow prompts to install and then click Install. After installation, top right, click the notification flag in the Server Manager. Click promote this server to a domain controller > Add a new forest > In Root domain name, mydomain.com > Follow prompts to finish install. Once created the computer should restart.
</p>
<br />

<p>
<img width="1010" alt="Created New User in _Admins, Jane Doe p3" src="https://github.com/user-attachments/assets/206ad513-9076-4235-9c6d-739fa2751faa" />
  
</p>
<p>
Now that we have the domain installed, a Domain Admin user must be created within the domain. We navigate to Active Directory Users and Computers and create an Organizational Unit (OU) called "_EMPLOYEES". We also create another Organizational Unit called "_ADMINS". To do this we right click mydomain.com and select New and select Organizational Unit to input the name. After that we click on "_ADMINS" and right click to select user to enter information for the admin user. This will include first and last name, user logon name, password, and options regarding password. Once this is completed, we still need to add this user to the "Domain Admins" Security Group. Right click the user and select properties, then select members of to search groups. When populated search "Domain Admins" and then select apply and ok. To manage domain wide settings, create and manage user accounts, configure security policies, an Admin User must be added and created.
</p>
<br />

<p>
<img width="1472" alt="Join client-1 to domain" src="https://github.com/user-attachments/assets/6011cc73-c819-4f87-9f2f-8083eab72c36" />
<img width="1184" alt="Verify client-1 shows up in ADUC in dc-1" src="https://github.com/user-attachments/assets/615610d7-ace1-4739-9603-eba85b50a39a" />

</p>
<p>
From "Client-1" click start and then select system. After that select Rename this PC (advanced), under computer name select change and Computer Name/Domain Changes should generate. In this window under Member of type mydomain.com and select ok. The computer will recognize the domain and asks you to sign in and you will be using Domain Admin you created. In this case I created Jane Doe as a Domain Admin and would sign in under her. The computer prompts you on joining mydomain.com and prompt a restart. I can also go back to "DC-1" to verify the virtual machine, or computer shows up in ADUC.
</p>
<br />

<p>
<img width="1259" alt="From client-1, (jane_admins) allow domain users access to remote desktoop" src="https://github.com/user-attachments/assets/6c70712c-4f2f-4cde-a2b8-885df319bd3d" />

</p>
<p>
 Allow "Domain Users" access to remote desktop, after creating users we can log in as normal for a non-administrative user. Click start > System > Remote Desktop > Under User Accounts, Select users that can remotely access this PC > Enter Domain Users in the object names to select. Now all domain users can log in remotely. Normally you would do this with Group Policy that allows you to change many systems at once. 
</p>
<br />

<p>
<img width="608" alt="Configured Domain Group policy to lockout after five times" src="https://github.com/user-attachments/assets/4c42559e-6e32-4e4d-9419-a281731dacb3" />
<img width="1540" alt="Unlocking account because user failed password more than five times" src="https://github.com/user-attachments/assets/ff2e5c14-f1ed-41cc-a294-207fc583ae4e" />
<img width="1916" alt="Observation of user logs p2" src="https://github.com/user-attachments/assets/9aa83d35-74d1-4c13-b610-3a6d4fa314d3" />

</p>
<p>
After creating domain users, I configured the Domain Group Policy to lockout users for incorrect passwords after five attempts. After configuration I tried to sign in with a domain user purposely using an incorrect password five times. The user’s account was locked due to many attempts to sign incorrectly. I signed up using the Domain Admin account to unlock the users account to have access. To check the attempts, I then went into Event Viewer, Windows Logs, and then Security. Normally 4625 will have specific information on network and IP address information. Event Viewer does show if the audit was a success or failure pertaining to logging into account. 
</p>
<br />
