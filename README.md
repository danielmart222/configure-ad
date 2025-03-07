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

- Part 1 Prepareing AD infrastucture
- Part 2 Deploying Active Directory 
- Part 3 Creating users with Power shell
- Part 4 Group Policy and Managing Accounts

<h2>Deployment and Configuration Steps Part 1: Preparing AD infrastucture</h2>

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

- Step 3 Create a windows servers 2022 named DC-1

-------------
![image](https://github.com/user-attachments/assets/d1a82508-dada-40e5-9cf2-d4abcc82b521)

---------

![image](https://github.com/user-attachments/assets/2c3c02be-ee18-473c-83d5-b652ae9bed6a)

------------

- Step 4 Create a windows 10 Pro named Client-1

-----------

![image](https://github.com/user-attachments/assets/914a8f7a-05c2-4a76-827d-5f6826c121fd)


-----------

- Step 5 Change DC-1’s NIC from Dynamic to Static in Azure

----------

- Go to DC-1 -> under networking: select networking settings -> click on Network interface/ IP configuaration -> click on ipconfig1 towards the bottom -> under allocation: select Static -> save changes

------

![image](https://github.com/user-attachments/assets/e06b1311-cee3-44f2-9b97-4a5a6f37996a)



---------

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


