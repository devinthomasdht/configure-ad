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

- Setup Domain Controller in Azure 
- Creat
- Step 3
- Step 4

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
Ping and Ipconfig /all: To verify "DC-1" and "Client-1" are on the same Virtual Network, in PowerShell we use ping to reach a host. In this example "DC-1's" IP address is 10.0.0.4 and we use command "ping 10.0.0.4 in the PowerShell". If it does not work, make sure the two VM's are on the same Virtual Network and "DC-1's" firewall is disabled. 
