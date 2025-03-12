<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure) Part 1 and 2 </h1>
This tutorial will guide you through the process of implementing on-premises Active Directory using Azure Virtual Machines. First, we’ll create a resource group and virtual network in your Azure subscription to organize and connect the resources. Next, we’ll deploy a Windows Server 2022 virtual machine (VM) and a Windows 10 Pro VM, which will be used to set up Active Directory Domain Services (AD DS) and its necessary dependencies. Once the VMs are deployed, we'll promote the Windows Server 2022 VM to a domain controller. As part of the prerequisites, we’ll configure the Windows Server 2022 VM’s IP settings to static, ensuring a stable connection. Then, we’ll update the DNS settings on the Windows 10 Pro VM to point to the static IP of the Windows Server 2022 VM, allowing the Windows 10 VM to join the domain. Additionally, we’ll create Organizational Units (OUs) within Active Directory to organize and manage users, computers, and other objects. We will also create a Domain Admin user within the domain to ensure proper administrative control.<br />




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


<h1>Deployment and Configuration Steps: Part 1 - Preparing AD infrastucture</h1>

<h2>Preparing AD infrastructure in azure </h2>

------------


![image](https://github.com/user-attachments/assets/8fd1e0f9-ebc8-40ff-86db-544ac4b11a50)


----------

<h2>Step 1: Create a resource group</h2>

----------


![image](https://github.com/user-attachments/assets/46368609-71f5-4db3-96ec-30aa38d0f42d)


----------------------

<h2>Step 2: Create a virtual network</h2>

------------

![image](https://github.com/user-attachments/assets/e533f9dd-d622-4c77-b097-547ee1380220)


---------------

<h2>Step 3: Create a Windows Server 2022 virtual machine named DC-1</h2>

-------------

![image](https://github.com/user-attachments/assets/d3a8cad7-1077-449d-9c78-1ac1d5b64eef)


---------

![image](https://github.com/user-attachments/assets/1bd8945a-de92-48da-aa9f-98eb95458564)


------------

<h2>Step 4: Create a Windows 10 Pro virtual machine named Client-1 </h2>

-----------

![image](https://github.com/user-attachments/assets/3c8fa121-7da7-434d-9a9c-e4ea1716ef12)



-----------

<h2>Step 5: Change DC-1’s NIC from Dynamic to Static in Azure</h2>



------------------



- Go to DC-1 -> under networking: select networking settings -> click on Network interface/ IP configuaration -> click on ipconfig1 towards the bottom -> under allocation: select Static -> save changes

------

![image](https://github.com/user-attachments/assets/050f8253-6ae5-4aa2-9203-ac2a7ebd1642)


----------




<h2> Step 6: Log onto DC-1 and disable the firewall so we are able to Ping DC-1 from our Client-1 </h2>

---------



- Type Run in the search bar -> Type the command wf.msc -> click on windows defender firewall properties -> turnoff the firewall state on the Domain profile, Private profile, and Public Profile -> click on apply and ok 

----------

![image](https://github.com/user-attachments/assets/794f96bc-20a5-42f6-acf6-0dd43a320ceb)


-----------


- Step 6 (continued): Type Run in the search bar -> Type the command wf.msc ->


--------------

  ![image](https://github.com/user-attachments/assets/71bf18e7-87eb-45f0-aae2-87acf9e073df)




----------


- Step 6 (continued): Click on windows defender firewall properties -> turnoff the firewall state on the Domain profile, Private profile, and Public Profile -> click on apply and ok

-----------


![image](https://github.com/user-attachments/assets/63ac8a66-4280-481b-83f8-a03140bcbbf3)


------------

<h2> Step 7: Change client-1’s DNS settings </h2>

---------

- 1st go to DC-1 and copy DC-1’s Private IP Address 10.0.0.4 -> next go to client-1 -> under Networking: select network settings -> click on Network interface / IP configuration -> under settings: select DNS servers -> under DNS servers: select custom -> Under DNS server: paste DC-1’s Private IP address -> save changes -> Restart Client-1

-------------

![image](https://github.com/user-attachments/assets/d65fda4b-3cee-443e-b2ea-866c2e91bed0)



----------


<h2>Step 8: Log in to Client-1 and Ping DC-1’s private IP address in powershell </h2>

---------


Log in to Client-1 -> open PowerShell and Ping DC-1’s Private IP address -> next type the command ipconfig /all and check the DNS server’s IP address 

---------------

![image](https://github.com/user-attachments/assets/860ce915-ce02-4942-9391-d82a25f78a7f)



---------

- Step 8 (continued): Open Power Shell and Ping DC-1’s Private IP address ->

----------


![image](https://github.com/user-attachments/assets/4e71dbb4-96a7-4f00-9105-f66ecec28652)



-------


- Step 8 (continued): Next type the command ipconfig /all and check the DNS server’s IP address -> the DNS server for Client-1 should have DC-1’s Private IP address

------------------

![image](https://github.com/user-attachments/assets/67de06f4-1825-4227-bda7-7376614cceb8)

------------

<h1>Part 2: Deploying Active Directory on Windows Server 2022 and Promoting Windows Server 2022 to a Domain Controller</h1>


-----------

<h2>Step 1: Log onto DC-1 and open Server Manager</h2>

--------

- Step 1 (continued): Click on Add roles and features -> under Server roles: select and check Active Dircetory Domain Services -> under Confirmation: click on install.

-----------


![image](https://github.com/user-attachments/assets/6e44406d-f90e-403f-b8b3-b287e688747c)


-----------


-  Step 1 (continued): under Confirmation: click on install -> under results: wait until it says (installation succeeded)


----------------


![image](https://github.com/user-attachments/assets/2058394b-eb6c-46cb-ad92-52467ff83151)



------------



<h2>Step 2: Promote Windows Server 2022 (DC-1) to a domain controller</h2>



-------------


- Click on the notification icon (the Flag) and click on Promote this server to a domain controller


-----------

![image](https://github.com/user-attachments/assets/96bca5d2-203b-4845-a3ee-7ddf221dad87)


---------

- Step 2 (continued): under Deployment Configuration: select Add a new forest-> under Root domain name: Type Mydomain.com

---------

![image](https://github.com/user-attachments/assets/7c183c4d-5704-4e50-abc4-d0d56a4fae27)


------

- Step 2 (continued): under domain controller options: enter Password and confirm Password


-------------

![image](https://github.com/user-attachments/assets/f0639682-ed82-488a-b7a8-7d446e888008)


-----------------



- Step 2 (continued): under DNS options: uncheck create DNS delegation


-----------------

![image](https://github.com/user-attachments/assets/a18adefc-92e8-4df1-926f-a3a51620c2c9)


--------------



- Step 2 (continued): under additional options: wait for the NetBIOS domain name to populate – then click on next


----------

![image](https://github.com/user-attachments/assets/3ee6d36c-58cd-432d-b13d-94c23ccfa330)


-------------


- Step 2 (continued): under Prerequisites Check: wait until you see a green check (all prerequisites checks passed successfully)  and click on install ->  wait for installation to finish


-------------

![image](https://github.com/user-attachments/assets/d72f9719-8551-4d35-bb05-ce56f9fa5271)


------------


<h2>Step 3: After installing Active Directory Domain Services and Promoting Windows Server 2022 to a domain controller, log in to DC-1 with your domain username </h2>


---------------


![image](https://github.com/user-attachments/assets/96483afa-339f-434f-a4c3-b283197d5b78)



------------


<h2> Step 4: Create OU’s (Organizational Unit) and Create a Domain Admin user within the domain </h2>


--------------

- Within DC-1 -> go to start -> select Windows Administrative Tools -> next click on Active Directory Users and Computers

-----------


![image](https://github.com/user-attachments/assets/9a5a23d2-f6b8-4edd-85ce-86ad646f710b)


-----------


- Click on mydomain.com -> Right click on Mydomain.com -> select New -> next, select Organizational Unit -> name it _EMPLOYEES

-----------


![image](https://github.com/user-attachments/assets/ab201f7c-18b6-4a53-afd6-31b8d635b74b)


----------


- Create an Organizational Unit named  _ADMINS -> 




----------




![image](https://github.com/user-attachments/assets/b6b3f00c-7d47-45da-969c-a40f1a45ff9e)




------------


- Create a new user in the _ADMINS OU named Jane Doe -> Right click the _ADMINS OU -> select new -> select user 


-----------



![image](https://github.com/user-attachments/assets/978ef12b-1b11-4867-8844-c95d0ba2c098)



--------------

- Name user Jane Doe ->Enter password for user->



---------



![image](https://github.com/user-attachments/assets/93ccb5b1-b547-41eb-906d-42e0e4c0dfc5)



----------------


- Step 4 (continued): Right click on Jane Doe -> click on properties -> choose the member of tab -> click on add -> type: Domain Admins -> click on check names -> click on OK -> click on apply -> ok


-----------



![image](https://github.com/user-attachments/assets/7ce245b3-f1ff-49cd-9788-7e92d40e66f7)


---------



<h2> Step 5: Join Client-1 to the domain </h2>


-------------


- Log in to client 1 as labusers-> go to settings -> go to system -> go to about -> then click on advanced system settings -> 


---------


![image](https://github.com/user-attachments/assets/a09ffd31-9f32-46dc-b92a-6563db9af718)


-----------

- Choose the computer name tab -> click on change -> select domain -> type: mydomain.com-> click on ok


-------


![image](https://github.com/user-attachments/assets/91484d83-a817-416c-95cc-dd027327e226)

---------


- Under Computer Name/Domain Changes: type Jane Doe’s username and password 


-------------



![image](https://github.com/user-attachments/assets/6a920e71-6aa5-44de-a615-f9c6d9a7ce06)



-------------

- You should get a welcome banner notification indicating you have joined the domain



------------------


![image](https://github.com/user-attachments/assets/7bcb12ef-6543-490e-bcd0-bc1a72eab776)




------------

<h2> Step 6: Create an OU named _CLIENTS and add Client-1 to _CLIENTS </h2>


-----------

- Within DC-1, go to Server Manager -> at the top right corner: click on tools-> click on Active Directory Users and Computers-> 

---------------


![image](https://github.com/user-attachments/assets/2e04787f-8daf-4c3a-ae6d-b7729c645cc7)

-------

- Click on Mydomain.com -> Right-click on mydomain.com -> click on New -> choose Organizational Unit -> Name it _CLIENTS ->Next, go to the Computers OU -> right-click on Client-1 -> click on move -> Choose _CLIENTS-> click on ok 


-----------


![image](https://github.com/user-attachments/assets/039a09f6-2983-434a-9978-b5e2a3179b8c)


--------













