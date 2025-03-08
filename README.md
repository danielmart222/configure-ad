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

- Part 1 Preparing AD infrastucture
- Part 2 Deploying Active Directory 
- Part 3 Creating users with Power shell
- Part 4 Group Policy and Managing Accounts

<h2>Deployment and Configuration Steps: Part 1 - Preparing AD infrastucture</h2>

- Preparing AD infrastructure in azure

------------


![image](https://github.com/user-attachments/assets/8fd1e0f9-ebc8-40ff-86db-544ac4b11a50)


----------

- Step 1 Create a resource group

----------


![image](https://github.com/user-attachments/assets/66ad7e5c-5d86-4c54-ae70-f20fbb7bc764)

----------------------

- Step 2 Create a virtual network

------------

![image](https://github.com/user-attachments/assets/79e22a53-ff4a-45e8-9498-5c6d249c1c96)



---------------

- Step 3 Create a Windows Server 2022 virtual machine named DC-1

-------------
![image](https://github.com/user-attachments/assets/d1a82508-dada-40e5-9cf2-d4abcc82b521)

---------

![image](https://github.com/user-attachments/assets/2c3c02be-ee18-473c-83d5-b652ae9bed6a)

------------

- Step 4 Create a Windows 10 Pro virtual machine named Client-1

-----------

![image](https://github.com/user-attachments/assets/914a8f7a-05c2-4a76-827d-5f6826c121fd)


-----------

- Step 5 Change DC-1’s NIC from Dynamic to Static in Azure



------------------



- Go to DC-1 -> under networking: select networking settings -> click on Network interface/ IP configuaration -> click on ipconfig1 towards the bottom -> under allocation: select Static -> save changes

------

![image](https://github.com/user-attachments/assets/232ea8c1-b8c4-41f8-a3f2-5e86dad63e2d)

----------




- Step 6 log onto DC-1 and disable the firewall so we are able to Ping DC-1 from our Client-1

---------



- Type Run in the search bar -> Type the command wf.msc -> click on windows defender firewall properties -> turnoff the firewall state on the domain profile, Private profile, and Public Profile -> click on apply and ok 

----------

![image](https://github.com/user-attachments/assets/9ec484b2-06a0-45f9-a366-83ed07ab691d)


-----------


- Step 6 Type Run in the search bar -> Type the command wf.msc ->


--------------

  ![image](https://github.com/user-attachments/assets/71bf18e7-87eb-45f0-aae2-87acf9e073df)




----------


- Step 6 Click on windows defender firewall properties -> turnoff the firwall state on the domain profile, Private profile, and Public Profile -> click on apply and ok

-----------


![image](https://github.com/user-attachments/assets/b08f6ec2-3a4c-4b76-a55a-fb20b25edefd)



------------

- Step 7 Change client-1’s DNS settings

---------

- 1st go to DC-1 and copy DC-1’s Private IP 10.0.0.4 -> next go to client-1 -> under Networking: select network settings -> click on Network interface / IP configuration -> under settings: select DNS servers -> under DNS servers: select custom -> Under DNS server: paste DC-1’s Private IP address -> save changes -> Restart Client-1

-------------

![image](https://github.com/user-attachments/assets/58da03ce-32da-4e7f-a2dd-8861bf9a1fc3)


----------


- Step 8  log in to Client-1 and Ping DC-1’s private IP address in powershell

---------


Log in to Client-1 -> open PowerShell and Ping DC-1’s IP address -> next type the command ipconfig /all and check the DNS server’s IP address 

---------------

![image](https://github.com/user-attachments/assets/860ce915-ce02-4942-9391-d82a25f78a7f)



---------

- Step 8 (continued) Open Power Shell and Ping DC-1’s IP address ->

----------


![image](https://github.com/user-attachments/assets/76f40ab7-9013-4746-878a-fed12bd0a1c7)


-------


- Step 8 (continued) Next type the command ipconfig /all and check the DNS server’s IP address -> the DNS server for Client-1 should have DC-1’s Private IP address

------------------

![image](https://github.com/user-attachments/assets/a9387ba6-1720-402a-8706-e6f772c2f558)


<h2>Deployment and Configuration Steps: Part 2 - Deploying Active Directory</h2>

- Part 2 Deploying Active Directory on Windows Server 2022 and Promoting Windows Server 2022 to a Domain Controller

-----------

- Step 1: Log onto DC-1 and open Server Manager

--------

- Click on Add roles and features -> under Server roles: select and check Active Dircetory Domain Services -> under Confirmation: click on install.

-----------


![image](https://github.com/user-attachments/assets/89381791-44bc-4aa9-90f7-ecd411b80f74)

-----------


- under Confirmation: click on install -> under results: wait until it says (installation succeeded)


----------------


![image](https://github.com/user-attachments/assets/a96a3e09-93a7-4a41-8a92-7ce68e681d1b)


------------



- Step 2: Promote Windows Server 2022 (DC-1) to a domain controller 



-------------

- Click on the notification icon (the Flag)  and click on Promote this server to a domain controller -> under Deployment Configurations: select Add a new forest -> under Root domain name: Mydomain.com -> under domain controller options: enter Password and confirm Password -> under DNS options: uncheck create DNS delegation -> under additional options: wait for the NetBIOS domain name to populate – then click on next -> under Prerequisites Check: wait until you see a green check (all prerequisites checks passed successfully)  and click on install -> wait for installation to finish ->


-------------

- Click on the notification icon (the Flag) and click on Promote this server to a domain controller


-----------

![image](https://github.com/user-attachments/assets/8df440ac-26cd-4514-9caf-393b5f26025d)



---------

- under Deployment Configuration: select Add a new forest-> under Root domain name: Type Mydomain.com

---------

![image](https://github.com/user-attachments/assets/73b96e10-62a8-4bf7-b174-ecb1095d53f9)



------

- under domain controller options: enter Password and confirm Password


-------------

![image](https://github.com/user-attachments/assets/9551f427-16dc-4cc6-9362-00759d290c85)


-----------------



- under DNS options: uncheck create DNS delegation


-----------------

![image](https://github.com/user-attachments/assets/c75ae157-453a-4155-8ace-37521fd723b7)



--------------



- under additional options: wait for the NetBIOS domain name to populate – then click on next


----------

![image](https://github.com/user-attachments/assets/8e3f166b-13c4-4faa-b487-92373ba18853)




-------------


- under Prerequisites Check: wait until you see a green check (all prerequisites checks passed successfully)  and click on install


-------------

![image](https://github.com/user-attachments/assets/a703ee85-8613-4f66-8894-c16ec2860336)



------------


- Step 3: After installing Active Directory Domain Services and Promoting Windows Server 2022 to a domain controller log in to DC-1 with your domain username


---------------


![image](https://github.com/user-attachments/assets/96483afa-339f-434f-a4c3-b283197d5b78)



------------


- Step 4: Create OU’s (Organizational Unit) and Create a Domain Admin user within the domain 




















