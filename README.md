<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Installation in Azure (Virtual Machine) for Windows Users</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

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
- Setup Client-1 in Azure
- Install Active Directory
- Promote as a DC
- Create a Domain Admin user
- Join Client-1 to your domain



<p align center>  
<h2>Deployment and Configuration Steps</h2>
<p>
<img width="271" height="227" alt="image" src="https://github.com/user-attachments/assets/2c58d64a-b973-44f7-8991-1553b72f62b2" />

<h2>Setup Domain Controller in Azure</h2>

First of all we are going to:

- Create a Resource Group, called Active-Directory-Lab
  <p align left>
  <img width="525" height="143" alt="image" src="https://github.com/user-attachments/assets/6e151bf6-09af-4e50-8c1b-9ef0ac38984b" />
- Create a Virtual Network and Subnet, called Active-Directory-Vnet
  <p align left>
  <img width="517" height="145" alt="image" src="https://github.com/user-attachments/assets/52ec7ffb-7493-404d-a0bb-e66bf06fec23" />
- Create the Domain Controller VM (Windows Server 2022) named “DC-1”
  <p align left>
  <img width="503" height="178" alt="image" src="https://github.com/user-attachments/assets/0f5cb5a4-bb33-4155-986c-e796fa16bcf3" />

 After dc-1 is created, set Domain Controller’s NIC Private IP address to be static, to do this we have to click on dc-1, click on Networking, click on Network Settings
  <p align left>
  <img width="819" height="273" alt="Screenshot 2025-11-30 142640" src="https://github.com/user-attachments/assets/7a4b56b1-3d4e-410b-8752-ddb8f41231f1" />

 Click on Network Interface/IP configuration
 <p align left>
 <img width="615" height="250" alt="Screenshot 2025-11-30 143933" src="https://github.com/user-attachments/assets/3e0b9940-1293-4c8c-a172-1c598fb80f65" />

 In Private IP address settings change from Dynamic to Static, then click on Save at the bottom of the page
 <p align left>
 <img width="287" height="446" alt="Screenshot 2025-11-30 144310" src="https://github.com/user-attachments/assets/648347e3-7315-408e-80b9-a9d2e3bef867" />

 Just for the purpose of this lab we are going to log into the dc-1 and disable the Windows Firewall (for testing connectivity), using the public IP address, RDC and log in using the credentials previously created
 <p align left>
 <img width="303" height="183" alt="image" src="https://github.com/user-attachments/assets/bf18190b-88df-4024-ad3b-eac4ea1f3c72" />
 <img width="334" height="247" alt="image" src="https://github.com/user-attachments/assets/27110b56-1534-4a6b-af7d-19dad487c519" />
 <img width="293" height="299" alt="image" src="https://github.com/user-attachments/assets/c20ee217-1ce8-4531-8304-cbaffd5bb5f8" />

Once we are log into dc-1, rigth click on start and select run, type wf.msc to go to Windows Firewall
<p align left>
<img width="640" height="451" alt="image" src="https://github.com/user-attachments/assets/9bd7e6db-f190-4321-afa4-89faabcab0eb" />

Click on Windows Defender Firewall Properties, because we are going to disable the Firewall
<img width="457" height="371" alt="Screenshot 2025-11-30 145734" src="https://github.com/user-attachments/assets/2c0ffafb-6ff5-4eeb-bc4d-03a176a6ca86" />

Click on Firewall State and choose off in the Domain Profile Tab, Private Profile TAb and Public Profile Tab, click on Apply and then Ok
<p align left>
<img width="325" height="371" alt="Screenshot 2025-11-30 150112" src="https://github.com/user-attachments/assets/64164437-6d1d-4091-b643-568f933d5fc2" />



<h2>Setup Client-1 in Azure</h2>

After dc-1 has been created we are going to created another Virtual Machine Client VM (Windows 10) named “Client-1”
<p align left>
<img width="513" height="164" alt="image" src="https://github.com/user-attachments/assets/3e4daf29-7f8b-4d9c-9818-aa49734e3d17" />

After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address
<p align left>
<img width="447" height="389" alt="image" src="https://github.com/user-attachments/assets/15a5e214-4d39-48bf-ae17-de4f8696e03f" />

Click on dc-1 in Azure and get the Private IP address,
<p align left>
<img width="659" height="434" alt="Screenshot 2025-11-30 151519" src="https://github.com/user-attachments/assets/82324076-4a8d-42c1-beff-5614a1274f85" />

Click on client-1, then click on Networking, Network Settings
<p align left>
<img width="647" height="434" alt="Screenshot 2025-11-30 151839" src="https://github.com/user-attachments/assets/eb45a2de-f2e2-43d8-8203-6f1248a50bdc" />

Click on the Network Interface/IP configuration
<p align left>
<img width="652" height="430" alt="Screenshot 2025-11-30 152039" src="https://github.com/user-attachments/assets/357d2918-33f7-401d-a2b5-7671ecf131d6" />

On the left pane menu, click on DNS Servers, and change DNS servers from inherit from virtual network to custom, paste the private IP address from dc-1 (172.16.0.4), click on Save
<p align left>
<img width="455" height="292" alt="Screenshot 2025-11-30 152655" src="https://github.com/user-attachments/assets/33679030-f9e7-43ea-a9e2-d13b47cdda61" />

From the Azure Portal, restart Client-1, then log into client-1 using RDC and credentials you previuosly created
<p align left>
<img width="304" height="183" alt="image" src="https://github.com/user-attachments/assets/5680f6fb-280d-4271-92e7-63cd57d63a1f" />
<img width="332" height="243" alt="image" src="https://github.com/user-attachments/assets/4557f543-0ac2-49b0-b2c2-67ded1b3107c" />
<img width="286" height="305" alt="Screenshot 2025-11-30 153530" src="https://github.com/user-attachments/assets/514bcdb7-7d6d-4d7c-8f1f-c6cfb7ddaa18" />

Attempt to ping DC-1’s private IP address 172.16.0.4
<p align left>
<img width="534" height="235" alt="image" src="https://github.com/user-attachments/assets/02255b90-76a8-429b-9135-22536812dec9" />

From Client-1, open PowerShell and run ipconfig /all. The output for the DNS settings should show DC-1’s private IP Address 172.16.0.4
<p align left>
<img width="347" height="374" alt="Screenshot 2025-11-30 162625" src="https://github.com/user-attachments/assets/c99a6f9a-494f-4c02-be62-bd0fd40df6cb" />

<h2>Install Active Directory</h2>

Login to DC-1 and install Active Directory Domain Services, to do that click on Start, Server Manager and then click on Add Roles and features
<p align left>
<img width="944" height="334" alt="Screenshot 2025-11-30 184157" src="https://github.com/user-attachments/assets/205706d2-35d1-40b0-ae3f-e9d65b201ef7" />

Click Next on Before you begin,
<p align left>
<img width="515" height="364" alt="Screenshot 2025-11-30 184550" src="https://github.com/user-attachments/assets/56db92ec-7670-434e-97a8-b0bbbd2a4361" />

Installation type, click on Role-based on feature-based installation, click Next
<p align left>
<img width="514" height="367" alt="Screenshot 2025-11-30 184839" src="https://github.com/user-attachments/assets/2383492f-777f-45c4-b5a0-5102ad254d5b" />

Server Selection, Select a Server from the server pool, there should be only one which is dc-1
<p align left>
<img width="515" height="369" alt="Screenshot 2025-11-30 185221" src="https://github.com/user-attachments/assets/912c413a-6f71-4e9a-bbb6-54936bec047c" />

Server Roles, click on Active Directory Domain Services, a new window will open and then click on Add Features, click Next
<p align left>
<img width="516" height="368" alt="Screenshot 2025-11-30 185904" src="https://github.com/user-attachments/assets/fc79fda4-18e9-492d-9bbb-d7949b4e81d7" />

AD DS, click on Next
<p align left>
<img width="521" height="368" alt="Screenshot 2025-11-30 190136" src="https://github.com/user-attachments/assets/c39f7629-e06e-4f74-a9fd-eb454662e83e" />

Confirmation, click on Restart the destination server automatically if required, yes and click on Install
<img width="517" height="370" alt="Screenshot 2025-11-30 190538" src="https://github.com/user-attachments/assets/9375f3f2-6e12-4a15-946c-66a2ff8d1527" />

Wait until installation has finished, when installed click on close
<p align left>
<img width="586" height="416" alt="image" src="https://github.com/user-attachments/assets/ba72f7b1-bd78-42b3-9f17-8ae50d4da8b5" />

<h2> Promote as a DC</h2>

Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is), to do that we have to go back to dc-1 in the Server Manager, Dashboard click on the yellow sign , then click on Add a new forest, root domain name: mydomain.com
<p align left>
<img width="516" height="317" alt="Screenshot 2025-11-30 191926" src="https://github.com/user-attachments/assets/642bf157-b1a1-444d-ac7f-88a9b25a7bec" />

For the Directory Services Restore Password, we are not going to use this but just set the password as: Password1, click on Next
<p align left>
<img width="505" height="368" alt="Screenshot 2025-11-30 192633" src="https://github.com/user-attachments/assets/f557fd13-411a-41f3-aa13-a6314ec583d8" />

In DNS options, uncheck Create DNS delegation, click on Next
<p align left>
<img width="508" height="374" alt="Screenshot 2025-11-30 192908" src="https://github.com/user-attachments/assets/81458507-c7e0-45ed-9da0-44f25dbe4df5" />

Click Next on Additional Option
<p align left>
<img width="512" height="371" alt="Screenshot 2025-11-30 193148" src="https://github.com/user-attachments/assets/12797661-77ec-4122-a621-e1de3618e0b7" />

Click Next on Paths
<p align left>
<img width="505" height="373" alt="Screenshot 2025-11-30 193415" src="https://github.com/user-attachments/assets/4a4e850a-006e-4f74-9f32-7e3d3f2ddb08" />

Click Next on Review Options
<p align left>
<img width="501" height="373" alt="Screenshot 2025-11-30 193634" src="https://github.com/user-attachments/assets/6e35eb8f-2360-4658-a899-16248c0ee150" />

At this point just click on Install and wait until dc-1 turned into the domain controller
<p align left>
<img width="505" height="368" alt="Screenshot 2025-11-30 194121" src="https://github.com/user-attachments/assets/54eae63b-5f62-433e-a53c-4c5acf6d185f" />

Installation is in progress
<p align left>
<img width="511" height="374" alt="Screenshot 2025-11-30 194433" src="https://github.com/user-attachments/assets/82670e4e-08e3-4980-ad2a-559b27d01e4b" />

When this is installation has finished the connection will be restore, we have to log into dc-1 using mydomain.com]\labuser because now dc-1 has been configured as the domain controller
<p align left>
<img width="332" height="241" alt="image" src="https://github.com/user-attachments/assets/56dd8e64-a09c-4d5a-b4b8-3297ac93533d" />

<h2>Create a Domain Admin user</h2>

In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”. To do this in dc-1 click on Start, Administrative tools and then Active Directory Users and Computers
<p align left>
<img width="298" height="286" alt="Screenshot 2025-11-30 200027" src="https://github.com/user-attachments/assets/8093fa27-50b4-4231-9ed4-7a23fbc91ab5" />

On Active Directory User and Computers, right click on mydomain.com, click on New, 

<p align left>
<img width="404" height="362" alt="image" src="https://github.com/user-attachments/assets/ab53abea-e571-4808-9735-6ec0ef345300" />

Organizational Unit, type _EMPLOYEE
<p align left>
<img width="325" height="282" alt="image" src="https://github.com/user-attachments/assets/2d21c8b2-6397-4f38-833b-c42b986a9e1c" />

Create a new OU named “_ADMINS”
<p align left>
<img width="655" height="563" alt="image" src="https://github.com/user-attachments/assets/23d5a059-ca4b-4746-a77a-f3a11a0e95a2" />

Hit refresh on Active Directory Users and Computers and it will rearranged alphabetically 
<img width="885" height="463" alt="image" src="https://github.com/user-attachments/assets/19ec2526-2bd2-4a9a-a8fe-ba5fa22845a0" />

Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” / Cyberlab123! From the Admin folder, right click, click on New, then click on User 
<p align left>
<img width="392" height="333" alt="Screenshot 2025-11-30 202323" src="https://github.com/user-attachments/assets/d35e3d8b-0a29-480f-a1c4-a1d0e21ecf19" />

Uncheck User must change password at next logon and click on Password never expires
<p align left>
<img width="390" height="334" alt="Screenshot 2025-11-30 202649" src="https://github.com/user-attachments/assets/e3b04161-3ce4-4ddb-b378-a17e4d2a1930" />

When completed just click on Finish
<p align left>
<img width="387" height="329" alt="Screenshot 2025-11-30 202921" src="https://github.com/user-attachments/assets/78242686-78c8-4177-8171-fc7f492f6b6a" />

Add jane_admin to the “Domain Admins” Security Group, from the _ADMINS folder, right click on Jane Doe account, click on Properties, click on Member of, Click on Add, type Domain Admins, click on Check Names, click ok, click on Apply, and finally click Ok. Now Jane is an actual domain admin
<p align left>
<img width="490" height="368" alt="Screenshot 2025-11-30 203548" src="https://github.com/user-attachments/assets/e85fcd0c-5b39-40b1-838d-c4a14cf27877" />

Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”
<p align left>
<img width="335" height="236" alt="image" src="https://github.com/user-attachments/assets/4c7ccac1-f816-4851-9e34-48d5b38d56f1" />


<h2>Join Client-1 to your domain</h2>

Login to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart)
To get to this screen, right click on Start, click on System, then from the right menu click on Rename this PC (advanced), a new window will open from Computer Name tab, click on Change, a new window will open again this time click on member of and choose Domain (type mydomain.com) and then ok
<p align left>
<img width="515" height="275" alt="Screenshot 2025-11-30 205441" src="https://github.com/user-attachments/assets/0c019527-7545-4f36-97c4-42c626d7ac63" />
<img width="286" height="350" alt="Screenshot 2025-11-30 205948" src="https://github.com/user-attachments/assets/6b6d708b-9990-4278-953a-59fe4fe2658e" />

Use Jane's credentials to join the domain controller, 
<p align left>
<img width="408" height="259" alt="Screenshot 2025-11-30 210318" src="https://github.com/user-attachments/assets/3772e7c0-542f-45e7-8c24-3f8b5740b9b1" />

After this a new window will open welcoming to the mydomain.com, client-1 will restart to apply changes
<p aling left>
<img width="456" height="282" alt="image" src="https://github.com/user-attachments/assets/3bf499c4-6192-49af-9124-810a2ce064ba" />

Login to the Domain Controller and verify Client-1 shows up in ADUC. To verify this in dc-1, type in the search bar Active Directory Users and Computers or click on Start menu and click on Windows Administrative Tools and choose Active Directory Users and Computers.

From Active Directory Users and Computers, expand mydomain.com, click on Computers and you will be able to see that client-1 is there, right click on Properties to inspect more information
<p align left>
<img width="501" height="349" alt="Screenshot 2025-11-30 211555" src="https://github.com/user-attachments/assets/e5f42107-5e52-4476-8ff0-6464cbe6d614" />






























