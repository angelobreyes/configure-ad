<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
Welcome! This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines. If you don't have a Microsoft Azure account, go check https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account for a free account with $200 credit so you can start.

<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Overview</h2>

![image](https://github.com/user-attachments/assets/2b8eb2ea-0b82-40e5-a647-584ac40ac210)

- Preparing Active Directory infrastructure in Microsoft Azure

  - We need to create 2 virtual machines.

    - A Domain Controller <b>(DC-1)</b> which acts as the server that manages all users/resources 

    - A Client <b>(Client-1)</b> which simulates a user/computer that is a part of the domain

  - Change Client-1's DNS IP address to the same IP address as the Domain Controller
 
    - By default, Client-1's DNS IP address will connect to Azure's DNS Server. For Client-1 to find and              join DC-1's domain, we will have to change Client-1's DNS IP address to the same IP address as DC-1      
       
- Deploying Active Directory
- Creating Users with Powershell

<h2>Deployment and Configuration Steps</h2>

<h3>1. Preparing AD infrastructure in Azure</h3>
  
  - First, let's create the Domain Controller, DC-1
    - Go to  

<br/>
--------------------------------------------------------------
Setup Domain Controller in Azure
—
Create a Resource Group
Create a Virtual Network and Subnet
Create the Domain Controller VM (Windows Server 2022) named “DC-1”
Username: labuser
Password: Cyberlab123!
After VM is created, set Domain Controller’s NIC Private IP address to be static
Log into the VM and disable the Windows Firewall (for testing connectivity)

Setup Client-1 in Azure
—
Create the Client VM (Windows 10) named “Client-1”
Username: labuser
Password: Cyberlab123!
Attach it to the same region and Virtual Network as DC-1
After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address
From the Azure Portal, restart Client-1
Login to Client-1
Attempt to ping DC-1’s private IP address
Ensure the ping succeeded
From Client-1, open PowerShell and run ipconfig /all
The output for the DNS settings should show DC-1’s private IP Address
