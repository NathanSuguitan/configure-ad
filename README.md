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

- Setup Resources and VM in Azure.
- Set Domain Controller NIC to be static.
- Ensure connectivity between client and domain controller.
- Install Active Directory.
- Create an Admin and Normal User account in Active Directory.
- Join Client VM to Domain VM.
- Setup Remote Desktop for Non-Administrative users in Client VM.
- Create Users additional users to log into Client VM.

<h2>Setup Resources and VMs in Azure</h2>

<h3>Create Domain Controller VM and Resource Group.</h3>

1. Sign in to the [Azure portal](https://portal.azure.com).

2. In the search, enter **virtual machines**, and under **services** select virtual machines.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/6bd4142c-ef6e-4de3-9628-67739358c580)

3. In the **virtual machines** page select **create**, then select **azure virtual machines**.

4. Under Project Details, next to resource group, click *Create New* and enter a name for the resource group, then click OK. In this example, the name for the Resource Group will be **AD**.<br>

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/b42980c6-3214-4b18-aaf5-a83ff06d5a44)

5. Under Instance Details, next to Virtual Machine Name, enter the name for your Domain Controller VM. In this example, the name for the Domain Controller VM will be **DC-1**.
- For the **Region** select one that will be used for both the Domain Controller VM and Client VM.
- For the **Image** select **Windows Server 2022 Datacenter**.
- For the **Size** choose an option that has at least 2 vcpus. 

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/79fad714-36e6-4e9e-adb9-b6b8eea598eb)
![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/b2e4001f-782d-4b73-a834-6559f0cfc45a)


6. Under **Administrator Account** create a Username and Password.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/ca208d11-9c3e-4cf1-93e1-3fb9a3a0b83a)

7. Under Licensing check the box to confirm hosting rights.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/4ad26b7a-a92a-4f03-bc15-4078d661c8bb)

8. Leave the rest of the settings as default, and click **Review + Create** at the bottom of the page.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/4bb1c648-2cd7-4ad2-8616-68c62e14698d)


9. After validation has passed, select **create** at the bottom of the page.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/cf6dbfd1-aa0f-444d-b7d1-fc8853bf1263)
![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/25e4d0a9-5b5f-4969-be05-bce3ea0f404d)


<h3>Create Client VM and Resource Group</h3>

1. Repeat steps 1-3 from the previous section to get to the **Virtual Machines** page of Azure. 

2. Under **Project Details** in the **Resource Groups** dropdown select the group created in the previous section.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/587070ea-c9b6-4c6c-8b1e-0079108496c1)

3. Under **Instance Details**, create a name for the client VM.
- Select the same **Region** used for the **Domain Controller**.
- For the **Image** select **Windows 10 Pro**.
- For the **Size** choose an option that has at least 2 vcpus.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/f1fd5e30-3226-4804-b43e-c09d56a1d428)
![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/24ff1f7f-c35a-43d9-92ba-47916bfc31a7)

4. Under **Administrator Account** create a Username and Password.
5. Under Licensing check the box to confirm hosting rights.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/4ad26b7a-a92a-4f03-bc15-4078d661c8bb)

6. Leave the rest of the settings as default, and click **Review + Create** at the bottom of the page.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/d7b68015-1070-4a56-a119-ac848ba848ca)

7. After validation has passed, select **create** at the bottom of the page.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/cf6dbfd1-aa0f-444d-b7d1-fc8853bf1263)
![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/25e4d0a9-5b5f-4969-be05-bce3ea0f404d)

<h2>Set Domain Controller NIC to Static</h2>

1. Enter **virtual machines** in the search, and under services select **virtual machines**.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/41cba94c-4810-45b9-a13e-6e45b0dc5594)

2. Select the **Domain Controller VM**.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/fd14dce9-2ad7-47ae-b464-f45c7cea14ef)

3. Under **Settings** select **Networking**.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/51983d11-c408-4ebf-bfc5-132223298549)

4. In the **Networking** page click the **Network Interface** link.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/9880c837-29b1-4a7d-9003-b3db2dca5d82)

5. Unser **Setting** select **IP Configuration**.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/feba88d6-4176-4a82-9967-d8142defe881)

6. Under **name** select **ipconfig1** to open the **Edit IP configuration** side bar. 

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/e45913f1-ff15-4607-802e-81ead0bb447d)

7. Under **Private IP address settings** change the selection from **dynamic** to **static** and save. 

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/ef22318d-edcb-4d85-87a0-d2a83a571507)

<h2>Ensure Connectivity Between Client and Domain Controller</h2>

<h3>Access Client VM Using RDC</h3>

1. In Azure enter **virtual machines** into the search, and under **services** select **virtual machines**.

2. Select **client-1**.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/754fb4de-c877-4f65-bcd7-fb7659c4659b)

3. Under **Essentials** copy the VM's **Public IP address**.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/2fcf5d44-7044-43f9-996e-5110d7fa5c58)

4. On Windows click start and search for **Remote Desktop Connection**.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/ac7649e7-5de1-43f5-bf49-6b312288c2bd)

5. Enter the Client VM's IP address into the search bar and click **connect**.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/8c5250d2-287c-405a-b0ca-d9712d55ac2e)

6. Enter the Client VM's username and password and click **OK**.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/acf50af8-32a4-4701-b525-1204ee6544d3)

<h3>Access Domain Controller VM using RDC and Enable Connectivity from Client VM</h3>

1. Follow the steps to connect to the **Client VM** using the **public IP address** of the **Domain Controller**.

2. click start and search **mf.msc**.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/5e4ae9c8-df83-4be6-8a49-60a0b7cef6f0)

4. Under **Windows Defender Firewall with Advanced Security on Local Computer** select **Inbound Rules**.

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/3e006350-d0cf-4466-8624-5c236f210eab)

5. In the **Inbound Rules** page, select **protocol** to sort by protocol. 

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/9ac2de5f-e598-433d-a7c9-bbea216de587)

6. Right-Click and select **Enable Rule** for both options named **Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)**

![image](https://github.com/NathanSuguitan/configure-ad/assets/138082246/425e7af4-46c1-403b-bc6c-c44fb29b687d)

