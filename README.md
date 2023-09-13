<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure connectivity between client and domain controller
- Install Active Directory
- Create an Admin and Normal User account in Active Directory
- Join Client VM to Domain VM
- Setup Remote Desktop for Non-Administrative users in Client VM
- Create Users additional users to log into Client VM

<h2>Deployment and Configuration Steps</h2>

   1. Create a Resource Group and Domain Controller Virtual Machine.<br>

A. Under Project Details, next to resource group, click *Create New* and enter a name for the resource group, then click OK. In this example, the name for the Resource Group will be **AD**.<br>

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/b42980c6-3214-4b18-aaf5-a83ff06d5a44)

B. Under Instance Details, next to Virtual Machine Name, enter the name for your Domain Controller VM. In this example, the name for the Domain Controller VM will be **DC-1**.
- For the *Region* select one that will be used for both the Domain Controller VM and Client VM.
- For the *Image* select **Windows Server 2022 Datacenter**
- For the *Size* choose an option that has at least 2 vcpu's. 

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/79fad714-36e6-4e9e-adb9-b6b8eea598eb)
![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/b2e4001f-782d-4b73-a834-6559f0cfc45a)


C. Under **Administrator Account** create a Username and Password.
D. Leave the rest of the settings as default, and click **Review + Create** at the bottom of the page. 

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/d7b68015-1070-4a56-a119-ac848ba848ca)
