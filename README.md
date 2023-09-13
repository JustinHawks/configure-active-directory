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

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

1. Create a Virtual Machine that will serve as the Domain Controller. Click "Create New" Resource Group and name it AD-Lab (AD = Active Directory). Name this Virtual Machine "DC-1" (DC = Domain Controler). Select a region. Select Windows Server 2022 for the Image.
   
![01_DC1](https://github.com/JustinHawks/configure-active-directory/assets/88342524/3f7fb7c6-cc64-41ba-a5de-99e7e22b9655)

2. Create a password for this VM. Check the box under licensing and follow the "Review + Create" process to deploy the domain controller.
   
![02_DC1](https://github.com/JustinHawks/configure-active-directory/assets/88342524/fa37730a-cbe2-41c4-b8b2-c98487ca41e4)

3. Now create another VM that will serve as a client machine. Make sure to select the same resource group that was created in Step 1 called "AD-Lab". Name this Virtual Machine "Client-1". Select the same region as DC-1. Instead of Windows Server, select Windows 10 Pro 22h2 for the Image.

![03_client1](https://github.com/JustinHawks/configure-active-directory/assets/88342524/5368ed66-b2f6-438f-bf3b-c74214fc282a)

4. Create a password for this VM. Check the box under licensing and follow the "Review + Create" process to deploy the domain controller. In the networking tab, verify that the Virtual Network for Client-1 is the same as the DC-1.

![04_client1](https://github.com/JustinHawks/configure-active-directory/assets/88342524/ef1be799-b047-4c45-b433-90c02521a186)
![05_client1](https://github.com/JustinHawks/configure-active-directory/assets/88342524/a7d9a415-2708-400a-8c50-5ad0cdb3cc15)
![07_same_vnet](https://github.com/JustinHawks/configure-active-directory/assets/88342524/9dda7368-6e39-41ef-858a-e4b2cb6dc21e)

5. Within the DC1 VM overview, set the NIC Private IP address to be static. Keep note of the private IP address here.
   
![06_DC1_staticIP](https://github.com/JustinHawks/configure-active-directory/assets/88342524/47d52480-7966-44fc-95a1-df5a890b681e)

6. Now open Remote Desktop Connection on your local PC. Enter the public IP address for DC-1. and log in.

![08_dc1_login](https://github.com/JustinHawks/configure-active-directory/assets/88342524/7225b392-2ddf-4160-9bc3-8985176f7b41)

7. Log into the client-1 VM Enter the public IP address for client-1 into the Remote Desktop Connection on your local PC and log in.

![09_client1_remdt](https://github.com/JustinHawks/configure-active-directory/assets/88342524/a8560c7e-7d78-47cd-96e4-121c950ac988)

8. Open the Command Prompt and ping DC-1's private IP address with "ping -t (IP address). This command will execute a perpetual ping from client-1 to DC-1. For now, the ping will fail. In order to establish a connection, we need to enable traffic on DC-1.

![10_client1_ping_dc1_fail](https://github.com/JustinHawks/configure-active-directory/assets/88342524/5750ed65-8d7e-4ece-a447-ded1dc31719e)

10. Switch over to the DC-1 VM. You may see the Server Manager interface that comes with Windows Server.

![11_DC-1](https://github.com/JustinHawks/configure-active-directory/assets/88342524/0346744b-c351-416e-bd51-27b2cd400a6c)

11. In the Windows search bar, search for "wf.msc" and open it.

![12_dc1_wfmsc](https://github.com/JustinHawks/configure-active-directory/assets/88342524/03d2346f-657f-4785-82b6-7a49e6821dcb)

12. Sort the protocol so that ICMPv4 is easily visible. Enable all ICMPv4 Traffic.
    
![13_dc1_enable1pv4traffic](https://github.com/JustinHawks/configure-active-directory/assets/88342524/61292c63-826a-4d93-9108-9c4ac40b67e3)

13. If you switch to the Client-1 VM, you should see a ping has been established successfully.
    
![14_client1_ping_success](https://github.com/JustinHawks/configure-active-directory/assets/88342524/af3a653b-d153-4949-8fa8-f432e09dd1b9)

14. Back in the DC-1 VM, Open the Server Manager Dashboard and select "Add roles and features." Within roles, make sure "Active Directory Domain Services" is selected and continue the install process.
    
![15_dc1_installAD](https://github.com/JustinHawks/configure-active-directory/assets/88342524/e906a9ed-1b2d-407b-a73f-7229756a6477)

15. In the Server Manager Dashboard. Select the drop-down menu with the "!" icon and select "Promote this server to a domain controller"
    
![16_dc1_promote](https://github.com/JustinHawks/configure-active-directory/assets/88342524/36310293-dcbb-44c5-8d7e-640255f0f802)

16. Select "Add a new forest" and specify a name for this Root Domain. In this example, I'll call it "hawk.com"
    
![17_DC1_addforest](https://github.com/JustinHawks/configure-active-directory/assets/88342524/1d8b8d53-c43a-4a05-95c1-b4b868c2b500)

17. Leave the domain controller options as default and set a password. Continue the installation process. At the end, you will be prompted to restart the VM.

![18_DC1_setpw](https://github.com/JustinHawks/configure-active-directory/assets/88342524/2c680e3e-d1b0-4f5a-b3d0-a94f7238f5a8)
![19_DC1_restart](https://github.com/JustinHawks/configure-active-directory/assets/88342524/e6bd0d4c-7db6-4e85-a0c4-f74158a6e0bb)

18. Re-establish a remote connection to DC-1. Log-in to DC-1, but this time set the context of the user with the domain name set up in step 16. "hawk.com\labuser"

![20_DC1_login](https://github.com/JustinHawks/configure-active-directory/assets/88342524/c32ea3b5-282b-4914-932f-60d8fdeb0a3e)
![21_DC1_loginasdomaincontroller](https://github.com/JustinHawks/configure-active-directory/assets/88342524/46ab5464-0dbf-4c35-b974-2c7727323467)

19. Active Directory is now installed on DC-1. You can access "Active Directory Users and Computers" in the top right panel, or you can search for it in the Windows start menu. This is the Active Directory user interface. We can see the hawk.com domain with several folders. Within "Users" you can see our "labuser" account.
    
![22_DC1_orgunits](https://github.com/JustinHawks/configure-active-directory/assets/88342524/95205bd8-be5e-42f6-82dc-671c2e8e93c9)
![23_AD_userinterface](https://github.com/JustinHawks/configure-active-directory/assets/88342524/6b2d3638-ba19-44d1-83b1-0dfd922e1dd0)

20. Right-click the domain name "hawk.com" and select New Object > Organizational Unit. Create one unit called "_EMPLOYEES" and another unit called "_ADMINS"

![24_orgunits](https://github.com/JustinHawks/configure-active-directory/assets/88342524/0881ece3-0afb-4506-9468-2698ceb554a5)
![25_orgunits](https://github.com/JustinHawks/configure-active-directory/assets/88342524/abcbc7af-d855-421a-b8cb-ad5463abcb07)

21. Right-click the "_ADMINS" folder and select New Object > User. Create an admin account for the domain.

![26_admins_newuser](https://github.com/JustinHawks/configure-active-directory/assets/88342524/090876c1-418c-4c27-9078-c28a74e95802)
![27_admins_newuser](https://github.com/JustinHawks/configure-active-directory/assets/88342524/a09296c9-ef0b-4b2c-ba5e-1f278906e476)

22. Admin permissions will need to be enabled for the new admin user. Right-click on the admin account and select properties > Member Of > Add... > and enter "Domain Admins". Now the admin user is a member of Domain Admins and Domain Users.
    
![28_admins_makeuseranadmin](https://github.com/JustinHawks/configure-active-directory/assets/88342524/93257b88-9026-4fbe-b004-f49a287d37eb)

23. To complete this process, log off and log back into DC-1 with the new admin account. I named my admin "justin_admin". I will log in as Justin_Admin in the context of hawk.com.
    
![29_DC1_admins_logoff](https://github.com/JustinHawks/configure-active-directory/assets/88342524/923e4d3c-4dc1-402b-93f8-dd7e00b2c71d)
![30_DC1_newadminlogin](https://github.com/JustinHawks/configure-active-directory/assets/88342524/3cc6a619-0f1c-4e34-9bfe-90182e63f1fb)

25. Back on Client-1 VM within the Azure portal, set the DNS settings to custom, and enter the Private IP address for DC-1. To complete this step, restart Client-1 within the Azure portal. Back in the DC-1 VM, verify that Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain. Create a new OU named “_CLIENTS” and drag Client-1 into it. Remote-in to Client-1 and log-in with the original "labuser" account.
    
![31_Client1_setDNS](https://github.com/JustinHawks/configure-active-directory/assets/88342524/998236e1-20e6-4b06-adb6-43cc09b38612)
![32_Client1_restart](https://github.com/JustinHawks/configure-active-directory/assets/88342524/570e5c6b-2780-450a-81b9-6ee296ea8201)
![33_Client1_login](https://github.com/JustinHawks/configure-active-directory/assets/88342524/35507a78-e230-4a56-af9e-62681c1b624f)

26. Confirm that Client-1 is connected to the DC-1 DNS Server. Run the command prompt and run "ipconfig /all"

![34_Client1_confirmDNS](https://github.com/JustinHawks/configure-active-directory/assets/88342524/70f734e2-2859-48e2-b260-4bdb5ebc4ca6)

27. In Client-1, right-click the Start menu > settings > rename this PC (advanced) > change > domain > and enter the name of the domain. I'll enter hawk.com > OK. A prompt will appear to enter account information for the domain admin.
    
![35_client1_domainchanges](https://github.com/JustinHawks/configure-active-directory/assets/88342524/0acb7f35-02ef-428c-b99a-98d2b0802aba)
![36_client1_success](https://github.com/JustinHawks/configure-active-directory/assets/88342524/1046329b-1c56-4769-82d4-a61ac807a3b7)

28. The Client will need to restart. Remote-in to Client-1 and now you can log in with the admin credentials in the context of the domain. I'll use "hawk.com/justin_admin"
    
![37_Client1_login_asadmin](https://github.com/JustinHawks/configure-active-directory/assets/88342524/e2d19646-845c-4101-a4c4-a28ada93472f)


