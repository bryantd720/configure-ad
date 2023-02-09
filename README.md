<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

**Part 1 (Setup Resources in Azure)**
- Create the Domain Controller VM (Windows Server 2022) named "DC-1"
- Take note of the Resource Group and Virtual Network (Vnet) that get created
- Set Domain Controller's NIC Private IP address to be static
- Create the Client VM (Windows 10) named "Client-1". Use the same Resource Group and Vnet that was created initially
- Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher)

**Part 2 (Ensure Connectivity between the Client and Domain Controller)**
- Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
- Login to the Domain Controller and enable ICMPv4 via the local windows Firewall
- Check back at Client-1 to see the ping succeed

**Part 3 (Install Active Directory)**
- Login to DC-1 and install Active Directory Domain Services
- Promote as a DC: Setup a new forest as mydomain.com (or anything you want)
- Restart and then log back into DC-1 as user: mydomain.com\username

**Part 4 (Create an Admin and Normal User Account in AD)**
- In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called "_EMPLOYEES"
- Create a new OU named "_ADMINS"
- Create a new employee named "whoever" (same password) with the username of "whoever_admin"
- Add jane_admin to the "Domain Admins" Security Group
- Log out/close the Remote Desktop connection to DC-1 and log back in as "mydomain.com\whoever_admin"
- Use whoever_admin as your admin account from now on


**Part 5 (Join Client-1 to your domain (mydomain.com))**
- From the Azure Portal, set Client-1's DNS settings to the DC's Private IP address
- From the Azure Portal, restart Client-1
- Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
- Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the "Computers" container on the root of the domain
- Create a new OU named "_CLIENTS" and drag Client-1 into there


**Part 6 (Setup Remote Desktop for non-administrative users on Client-1)**
- Log into Client-1 as mydomain.com\whoever_admin and open system properties
- Click "Remote Desktop"
- Allow "Domain Users" access to remote desktop
- You can now log into Client-1 as a normal, non-administrative user

**Part 7 (Create a bunch of additional users and attempt to log into Client-1 with one of the users)**
- Login to DC-1 as whoever_admin
- Open PowerShell_ise as an administrator
- Create a new File and paste the contents of this useful script into it (https://github.com/bryantd720/configure-ad/blob/main/Generate-Names-Create-Users.ps1)
- Run the script and observe the accounts being created
- When finished, open ADUC and observe the accounts in the appropriate OU
- Attempt to log into Client-1 with one of the accounts (take note of the password in the script)


<h2>Example Screenshots</h2>

<p>
<img src="https://i.imgur.com/Eu8o4Z7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After creating Domain Server via Azure, the Network Interface Card (NIC) Private Ip Address is set to static so it will remain the same. 
</p>
<br />


<p>
<img src="https://i.imgur.com/WhReINo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Active Directory running within the Domain Controller (Server) virtual machine. 
</p>
<br />  
  

<p>
<img src="https://i.imgur.com/r21alrQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
A script running in Windows PowerShell ISE. Usernames being generated to populate Active Directory installed on DC-1 (Server). Domain Users can then log into the Client-1 (Windows) virtual machine using a preregistered name and password. 
</p>
<br />    
  
  
