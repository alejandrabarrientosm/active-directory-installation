<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Infrastructure in Azure (Virtual Machine) for Windows Users</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines. We are going to create 2 Virtual Machines, one that serves as the domain control and will be name: dc-1 and the second Virtual Machine which will be getting the DNS from the domain control and will name: client-1.<br />

<h2>Video Demonstration</h2>

- ### [Youtube: Creating Windows and Linux Virtual Machines](https://www.youtube.com/watch?v=ISIau7bI14E) 

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Register account in Azure
- Create a Resource Group
- Setup Domain Controller in Azure
- Setup Client-1 in Azure
- Install Active Directory

<h2>Installation Steps</h2>
You must register for an Azure account in order to construct virtual machines. Examine the various alternatives that best fit your company or the projects you are working on. After creating your account you can create a Resource Group. First you have to create Resource Group, to start you can click in the search bar or click <img width="47" height="16" alt="image" src="https://github.com/user-attachments/assets/03be3787-9799-43fe-8ada-89b456bab4c3" />

- To create a Resource Group you can search resource group in the search bar or you can choose it from the dashboard
<p align left> 
<img width="729" height="365" alt="Screenshot 2025-11-25 103136" src="https://github.com/user-attachments/assets/a9a8699a-1e32-483c-b8ff-e2da9179601e" />

<img width="803" height="376" alt="Screenshot 2025-11-25 122156" src="https://github.com/user-attachments/assets/1a9d244a-f522-4dfe-bea5-02f1dc660880" />

Create the Resource Group you choose the correct subscription, name your Resource (Active-Directory-Lab), choose the region (Canada East) you want to deploy your Virtual Machine and then  click on review and create at the bottom of the page
<p align left>
<img width="1210" height="1009" alt="Screenshot 2025-11-06 154216" src="https://github.com/user-attachments/assets/e597e939-f874-44ea-af85-be69c1f1e650" />

After created the Resource Group, come back to your dashboard and click on Resource Group
<p align left>
<img width="803" height="376" alt="Screenshot 2025-11-25 122156" src="https://github.com/user-attachments/assets/870976e3-abd7-4c13-b946-1122185697e2" />

For this lab we created a Resource Group, called Active-Directory-Lab.
<p align left>
<img width="525" height="143" alt="image" src="https://github.com/user-attachments/assets/6e151bf6-09af-4e50-8c1b-9ef0ac38984b" />

To create a Virtual Network, search in the search bar for Virtual Network, then click on Virtual Network. For this lab we are going to create a Virtual Network and Subnet, called Active-Directory-Vnet
  <p align left>
  <img width="517" height="145" alt="image" src="https://github.com/user-attachments/assets/52ec7ffb-7493-404d-a0bb-e66bf06fec23" />
  

<p align center>  
<h2>Preparing Active Directory Infrastructure in Azure and Configuration Steps</h2>
<p>
<img width="271" height="227" alt="image" src="https://github.com/user-attachments/assets/2c58d64a-b973-44f7-8991-1553b72f62b2" />

<h2>Setup Domain Controller in Azure</h2>

This Virtual Machine dc-1 will serve as the Domain Control, to which our second Virtual Machine client-1 will get the local host. First of all we are going to create dc-1 which is the Domain control, to do that we are going to follow the next steps:

- After created the Resource Group you can continue with the second step which is creating a Virtual Machine, to do that you can click again the search bar   and look for Virtual Machines or in the dashboard click on Virtual Machines.
  <p align left>
  <img width="586" height="203" alt="Screenshot 2025-11-25 123544" src="https://github.com/user-attachments/assets/429c2ec6-fb03-4ac5-826e-e3c874c3e540" />

  Choose the first option
  <p align left> 
  <img width="686" height="386" alt="Screenshot 2025-11-25 123917" src="https://github.com/user-attachments/assets/85acb37f-f7e0-42f5-b547-0f214d793d70" />

Put in the appropriate subscription, in the resource group (Active-Directory-Lab) click on the one you just created, Choose a name for your Virtual Machine (Windows Server 2022), for Region choose the same you use in the Resource Group (Canada East), for Image use Windows Server 2022. Picture is just for reference
</p>
<img width="350" height="430" alt="Screenshot 2025-11-25 131103" src="https://github.com/user-attachments/assets/54a4c1c8-e74f-47a7-85a6-e534a3028954" />

For size try to use 2vcpus for a faster deployment, create a username and password for the Virtual Machine and always make sure to click in Licensing

<img width="311" height="326" alt="Screenshot 2025-11-25 132238" src="https://github.com/user-attachments/assets/03308153-0eea-4e95-97cd-f374847e0be9" />

Then click next for Disks
<p align left>
<img width="311" height="326" alt="Screenshot 2025-11-25 132238" src="https://github.com/user-attachments/assets/3e668943-9da0-42da-9ab7-167e8dc161d0" />


Click next for Network, then click on Review + Create 
<p align left>
<img width="393" height="455" alt="Screenshot 2025-11-25 133817" src="https://github.com/user-attachments/assets/6828c53e-21a4-460a-944f-655a575b8e18" />

After validation passed, click on Create at the bottom of the page to initialize deployment of the Virtual Machine
<p align left>
<img width="451" height="454" alt="Screenshot 2025-11-25 134704" src="https://github.com/user-attachments/assets/413c969e-8f81-45de-a779-148ac91090c5" />

- And that's how you create the Domain Controller Virtual Machine with Windows Server 2022 named “DC-1”
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

 Just for the purpose of this lab we are going to log into the dc-1 and disable the Windows Firewall (for testing connectivity), using the public IP address, RDC and log in using the credentials previously created (username: labuser and password: Cyberlab123!)
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

From the Azure Portal, restart Client-1, then log into client-1 using RDP and credentials you previuosly created (username: labuser and password: Cyberlab123!)
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

Wait until installation has finished, when installed click on close, follow with the next lab which is Deploying Active Directory.
<p align left>
<img width="586" height="416" alt="image" src="https://github.com/user-attachments/assets/ba72f7b1-bd78-42b3-9f17-8ae50d4da8b5" />































