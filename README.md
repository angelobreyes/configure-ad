<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
Welcome! This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines. If you don't have a Microsoft Azure account, go to <a href="https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account">Azure</a>  for a free account with $200 credit so you can start.

<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022 (Virtual Machine)
- Windows 10 (21H2) (Virtual Machine)

<h2>Overview</h2>

![image](https://github.com/user-attachments/assets/2b8eb2ea-0b82-40e5-a647-584ac40ac210)

- Preparing Active Directory infrastructure in Microsoft Azure

  - We need to create 2 virtual machines.

    - A Domain Controller <b>(DC-1)</b> which acts as the server that manages all users/resources 

    - A Client <b>(Client-1)</b> which simulates a user/computer that is a part of the domain

  - Change Client-1's DNS IP address to the same IP address as the Domain Controller
 
    - By default, Client-1's DNS IP address will connect to Azure's DNS Server. For Client-1 to find and
      join DC-1's domain, we will have to change Client-1's DNS IP address to the same IP address as DC-1      
       
- Deploying Active Directory
- Creating Users with Powershell

<br />

<b>*(the names used here (to name VMs, folders, usernames, passwords, etc.) are only for example. You can use your own preference)*</b>

<h2>Deployment and Configuration Steps</h2>

<h3>1. Preparing AD infrastructure in Azure</h3>
  
<h4>Setup Domain Controller in Azure (DC-1)</h4>

  <h5>Create a Resouce Group</h5>
  
  - Go to the Azure Portal -> Click Resource Groups -> Click Create
   
![image](https://github.com/user-attachments/assets/a1fc8d67-da53-42a7-b83a-a3d52e64d2f1)

![image](https://github.com/user-attachments/assets/89b72714-ff75-407d-b2e9-c53d2a6b7df0)

  - Fill Resource Group Name (for this example, I'll use Active-Directory) -> Click Review + Create -> Click Create

![image](https://github.com/user-attachments/assets/8f0181ef-161e-437d-8feb-5ff516314528)

![image](https://github.com/user-attachments/assets/eb5f5970-9fd9-4cb5-919a-065098354512)

![image](https://github.com/user-attachments/assets/72b03f31-ad35-4605-a9cc-5f326a5743d1)

<br />

  <h5>Create a Virtual Network and Subnet</h5>

  - On the search box, type virtual networks and click Virtual networks -> click Create

![image](https://github.com/user-attachments/assets/a354b9b3-594e-4b28-a9cf-fc40e95e1013)

![image](https://github.com/user-attachments/assets/6a6b99be-b7bf-46f6-8c21-82e17c68aa59)

  - *Make sure the Resource Group is set to what we previously created and Region is the same
  - Fill  Virtual Network Name -> Click Review + Create -> Click Create

![image](https://github.com/user-attachments/assets/ea1f23cf-abb8-4d24-a145-d524d018fd7c)

![image](https://github.com/user-attachments/assets/f0f65147-cf6f-49c2-9455-22b9cd592df7)

  - Wait until deployment is done

![image](https://github.com/user-attachments/assets/9f27f100-18dc-435d-b4a3-4d34930622f6)

<br />
  
  <h5>Create the Domain Controller VM (Windows Server 2022) named “DC-1”</h5>

  - On the search box, type Virtual Machines and click Virtual Machines

![image](https://github.com/user-attachments/assets/2f3ded1a-5bbd-4dc5-889c-a7d5d346c45e)

  - Click Create -> Click Virtual Machine

![image](https://github.com/user-attachments/assets/72e43c49-97a5-4a77-81c4-6de45ac6d3a2)

  - On Virtual Machine name, type DC-1 and make sure it is in the same Resource Group and Region we previously     created

![image](https://github.com/user-attachments/assets/5907fdf0-89c2-4e7f-b836-ca3fd74f827d)

  - Scroll down, on image choose Windows Server 2022

![image](https://github.com/user-attachments/assets/4823d3b3-9fa4-4ee2-91cf-3330f9d62fca)

  - Scroll down, on size, you can use 2 vcpus

![image](https://github.com/user-attachments/assets/07df19e7-3aec-4673-b6c9-88238ec7e980)

  - Scroll down, fill Username and Password

![image](https://github.com/user-attachments/assets/f91d10fa-99d2-4ed6-8fdb-0d03009a3346)

  - Scroll down, under Licensing, click the two checkboxes

![image](https://github.com/user-attachments/assets/533b7a81-2ebb-45a1-8089-e96c4732d8e3)

  - Scroll up and click Networking and make sure the Virtual Network is set to what we previously created 

![image](https://github.com/user-attachments/assets/027874ba-89af-4ba7-b749-973d079254e8)

![image](https://github.com/user-attachments/assets/46257d89-c829-4574-8479-cd719f7b1d9a)

  -  Scroll down and click Review + Create -> click Create

![image](https://github.com/user-attachments/assets/ab07c8a2-c7f5-4c54-ab51-9f238c3f1b36)

![image](https://github.com/user-attachments/assets/82ed6dfd-4e1d-440d-9661-c1dd2458d8bd)

  -  Wait until deployment is done 

![image](https://github.com/user-attachments/assets/05fe47da-334a-4cc6-b2ef-8ebd3529c7e0)

<br/>

<h4>Set DC-1's NIC Private IP address to static</h4>

  - On the search box, type virtual machines and click Virtual Machines

![image](https://github.com/user-attachments/assets/87b0870e-8ba2-4e42-a9c7-2a3836c9146f)

  - Click DC-1 -> click Networking then Network Settings

![image](https://github.com/user-attachments/assets/18054b7b-5f25-4a3c-b990-7d2bb281c64e)

![image](https://github.com/user-attachments/assets/9ccf8ca4-cfe1-4a48-9382-9f1eea84fd68)

  - On Network Settings, click the link under Network Interface

![image](https://github.com/user-attachments/assets/ba28fdaf-bf5c-4b7a-9522-5e2df016a374)

  - Click ipconfig1 -> click Static and Save 

![image](https://github.com/user-attachments/assets/02f6ec62-a659-4552-9cdc-a98046368dcc)

![image](https://github.com/user-attachments/assets/eb547106-12f1-42c4-91ec-31809c0eb841)

<br />

<h4>Setup Client-1 in Azure</h4>

  - Go back to Virtual Machines and click Create

![image](https://github.com/user-attachments/assets/82a547f4-4e91-491f-878f-f31244f4e4ab)

![image](https://github.com/user-attachments/assets/917b634c-c1a7-447d-9944-5116420263d8)

  - Make sure the Resource Group is set what we previously created -> Name : Client-1

![image](https://github.com/user-attachments/assets/c7ab502a-6151-469e-bb64-41e33d88164d)

  - Scroll down, on image choose Windows 10 -> on size, you can use 2 vcpus

![image](https://github.com/user-attachments/assets/35b27883-0fc8-4023-a6c4-2b107572b67e)

![image](https://github.com/user-attachments/assets/6d89b530-5c38-4d1d-843c-61be248a6237)

  - Scroll down, fill username and password

![image](https://github.com/user-attachments/assets/3aec44eb-5a92-48bd-b66b-189b6d871ff4)

  - Scroll down, under Licensing, make sure the click on the checkbox

![image](https://github.com/user-attachments/assets/d326aefa-d133-4c14-8345-8c58af76cd11)

  - Scroll up and click Networking, make sure the network is set to what we previously created

![image](https://github.com/user-attachments/assets/226b5716-2f17-4849-8bb2-0a5051028e68)

![image](https://github.com/user-attachments/assets/267a775f-914e-42bb-b5c2-b0601fdd86ed)

  -  Click Review + Create -> click Create

![image](https://github.com/user-attachments/assets/06864142-234b-42cb-99e5-e51a6319183d)

![image](https://github.com/user-attachments/assets/1a568b96-437c-435f-9366-0ada9eecd3bb)

  -  Wait until deployment is done

![image](https://github.com/user-attachments/assets/7fd2fc6d-7766-4004-a0c6-56503469dda0)

<br />

<h4>Set Client-1’s DNS settings to DC-1’s Private IP address</h4>

  -  On the search box, type virtual machine and click Virtual Machines

![image](https://github.com/user-attachments/assets/87b0870e-8ba2-4e42-a9c7-2a3836c9146f)

  - Click DC-1 and scroll down to look for Private IP address
  
![image](https://github.com/user-attachments/assets/3b4b205c-6aab-42d9-8ca1-29032cf802ef)

  - Double click on the Private IP address to highlight -> right click and copy

![image](https://github.com/user-attachments/assets/4fed8949-c7cf-48a2-b244-535a8b6cff3f)

  - Click Compute infrastracture to go back to the Virtual Machines page

![image](https://github.com/user-attachments/assets/92ee9f8b-431c-40e6-be01-9fbe814c1ba3)

  - Click Client-1 -> click Networking then Network Settings

![image](https://github.com/user-attachments/assets/6399bb56-cb54-4978-b557-474da34551e1)

![image](https://github.com/user-attachments/assets/c21769af-0213-4288-8469-2c185fe0e481)

  - Under Network Interface, click the link

![image](https://github.com/user-attachments/assets/67834960-21af-484d-b70d-5d69371c0766)

  - Under Settings. click DNS Servers

![image](https://github.com/user-attachments/assets/4496371a-7266-49e4-801e-89a7786c8529)

  - Click Custom -> right click and paste -> click Save

![image](https://github.com/user-attachments/assets/8f3caea6-c51f-4657-af06-b28077370299)

  - Click Compute Infrastracture then click Client-1

![image](https://github.com/user-attachments/assets/28724cfe-54bc-4125-80cc-aacb6760e93c)

![image](https://github.com/user-attachments/assets/0de7a8d9-b200-4623-b648-ca944b14225e)

  - Click Restart to make sure the changes are saved -> click Yes

![image](https://github.com/user-attachments/assets/a34b7ae0-bce7-4845-a4aa-24a619a8ef37)

![image](https://github.com/user-attachments/assets/ff612fdb-977e-47c2-add2-8bdd9cec05ea)

<br/>

<h3>2. Deploying Active Directory</h3>

<h4>Install Active Directory</h4>
  - Login to DC-1 via Remote Desktop Connnections 
    - Click Compute Infrastructure and look for DC-1's IP Address

![image](https://github.com/user-attachments/assets/7711b730-53b4-47e3-b198-7121efeac752)

  - Scroll right -> double click on IP address to highlight -> right click and copy   

![image](https://github.com/user-attachments/assets/1aebb264-3b42-4ed1-bd32-558c7fa8c6ce)

  - Press windows key or click Start button and type remote desktop and click Remote Desktop Connection

![image](https://github.com/user-attachments/assets/d565f403-c035-4ef5-9d21-27e6c43ac495)

  - Right click on text box and paste DC-1's IP Address

![image](https://github.com/user-attachments/assets/d68d17f9-51ad-4192-a5d9-e099906db066)

  - Fill in username and click connect

![image](https://github.com/user-attachments/assets/39ffea14-bfd2-45a9-a6f1-5292caf5b9a5)

  - Fill in password and click OK

![image](https://github.com/user-attachments/assets/f3e52530-c418-457b-865f-aad4129cea1e)

  - Within DC-1, Server Manager should open by default if not go to search box -> type Server Manager -> click Server          Manager

![image](https://github.com/user-attachments/assets/e3353376-f194-46e9-ba7c-1685b4fb76d6)

  - On Server Manager, click Add roles and features

![image](https://github.com/user-attachments/assets/ee0f0410-fdb2-4f4c-8bc4-d622a456d5fe)

  - Click Next -> Next -> Next 

![image](https://github.com/user-attachments/assets/7186d7a3-d4ed-4ad0-b977-3a34bc8cc377)

![image](https://github.com/user-attachments/assets/654f0c3d-48b2-4805-8725-bf2067f24847)

![image](https://github.com/user-attachments/assets/57b8e6e7-aaf6-4eb8-87fe-b774dfddb1b1)

  - Click on Active Directory Domain Services -> Add Features -> Next

![image](https://github.com/user-attachments/assets/255b85ee-8b8d-4106-b082-49105f3829cf)

![image](https://github.com/user-attachments/assets/19c8d74d-4c62-4e03-a129-fc7f40c2fcae)

![image](https://github.com/user-attachments/assets/1da7fcd7-a40a-4af9-b8c9-6db539a41ad3)

  - CLick Next -> Next -> click checkbox -> click Yes -> Install

![image](https://github.com/user-attachments/assets/bdffde99-6e79-444e-8b92-307d86f5dc91)

![image](https://github.com/user-attachments/assets/bd035353-ac5f-4b03-9d6d-cf85b16143f9)

![image](https://github.com/user-attachments/assets/470414f0-c6b3-4945-8175-206742cdefaa)

![image](https://github.com/user-attachments/assets/bbccb4fd-077d-4015-8206-942c7286b1a5)

  - Wait for installation to finish then close

![image](https://github.com/user-attachments/assets/418f9d6a-212f-4426-b199-f6347c7f0b00)

![image](https://github.com/user-attachments/assets/7fa91ab3-d92b-4865-90bd-ac8559e43fee)

<br />

<h4>Promote server to Domain Controller</h4>
   
  - Click the flag icon -> Promote this server to a domain controller

![image](https://github.com/user-attachments/assets/7bdc60e3-def4-4a52-a7d1-45192345e912)

![image](https://github.com/user-attachments/assets/68e2daeb-905b-4ed0-9f36-e3ce887b8042)

  - Click Add a new forest -> root domain name : mydomain.com -> click Next

![image](https://github.com/user-attachments/assets/d77c2c5b-b787-4898-af1f-2b7755f81457)

  - Fill password then click next -> next -> next -> next 

![image](https://github.com/user-attachments/assets/35ced272-f503-4177-b3b4-08478e02d6c8)

![image](https://github.com/user-attachments/assets/2a3167a4-653d-4032-973f-71139834c7ab)

![image](https://github.com/user-attachments/assets/845897c2-cf25-437b-833e-92d60613dc59)

![image](https://github.com/user-attachments/assets/e4586b97-1552-4910-ab84-92e8731f45ec)

  - Click next -> wait for prequisites to finish then Install

![image](https://github.com/user-attachments/assets/e220f32a-75bc-4684-a2ab-e1403f76f460)

![image](https://github.com/user-attachments/assets/442810f1-c5e1-4773-a841-a7aecd5e6e5a)

  <b>- When the installation is done, it will restart the DC-1 automatically</b>

<br />

<h4>Create a Domain Admin user within the domain</h4>

<h5>Create Organizational Unit _EMPLOYEES</h5>
 
  - On search box, type active directory and click Active Directory Users and Computers

![image](https://github.com/user-attachments/assets/a9686210-4b83-4d11-84ea-eff1a6479e17)

  - Click on the domain we created -> right click on blank space -> hover on New -> click Organizational Unit

![image](https://github.com/user-attachments/assets/942805b5-d6a8-4e4e-b765-1186170db43f)

![image](https://github.com/user-attachments/assets/54f4bb07-4fe3-4a6d-9e26-d7cddf2a9760)

  - Name the new Organizational Unit, _EMPLOYEES then click OK

![image](https://github.com/user-attachments/assets/a84d0db3-7f47-49f8-aaa1-13102233d6a0)

<h5>Create Organizational Unit _ADMINS</h5>

  - Right click on the domain -> hover on New -> click Organizational Unit

![image](https://github.com/user-attachments/assets/ecf8a8dc-8fef-45cb-ba5c-da25aaefae61)

  - Name the new Organizational Unit, _ADMINS then click OK

![image](https://github.com/user-attachments/assets/95bb0ceb-8838-4f34-831c-498242d590df)

<h5>Create a User and assign to Domain Admins </h5>

  - Right click on _ADMINS, hover on New and click User

![image](https://github.com/user-attachments/assets/cf23eac4-85fd-4379-899a-5b629a914e05)

  - Fill in all fields and click Next

![image](https://github.com/user-attachments/assets/7408a804-1e60-4213-b3b8-93b7bffeae4c)

  - Fill in password and uncheck the User must change password and Check Password never expires -> click Next -> Finish

![image](https://github.com/user-attachments/assets/085b687e-c268-4821-94f1-54625e0d5c0f)

![image](https://github.com/user-attachments/assets/96303cff-ec9a-4de4-8d96-290b06c116ab)

  - Right click on Jane Doe and click Add to Group

![image](https://github.com/user-attachments/assets/df63dc9e-943e-4286-9f2a-9fdac5ec13b8)

  - Enter Domain Admins -> Click Check Names -> Click OK -> Click OK

![image](https://github.com/user-attachments/assets/90f4ca64-45c2-402b-8dbc-c47d6e4b510b)


![image](https://github.com/user-attachments/assets/6a6c854e-6cd8-4684-bb88-b507d2a66dcc)

<h4>Log in as Jane (Admin)</h4>

  - Log out of DC-1 and log back in as "mydomain.com\janedoe"
  - Press Windows Key or click Start Button -> hover/click Power -> click Disconnect

![image](https://github.com/user-attachments/assets/6df8ffd4-a3af-43bd-ba59-b17cc616c88d)

![image](https://github.com/user-attachments/assets/ff4af785-c2c3-4e22-b2ea-70bf0a79b85f)

  - Click Start button -> type Remote Desktop -> Click Remote Desktop Connection

![image](https://github.com/user-attachments/assets/5ad9f780-549b-46cf-be97-972ab7493886)

  - Fill in IP Address if not already filled -> Click Show Options

![image](https://github.com/user-attachments/assets/bb570347-7dbd-4c73-8773-5c34a48f6249)

  - Type in mydomain.com\janeadmin and click Connect

![image](https://github.com/user-attachments/assets/f3573cee-b4bd-4d18-a273-bea172e5f5ff)

  - Enter Password and click OK

![image](https://github.com/user-attachments/assets/2aff0cfd-3edd-4994-9bf7-10b92029baca)

<br />

<h4>Join Client-1 to your domain (mydomain.com)</h4>

  - Go to Azure Portal -> hover on Virtual Machines and click Client-1

![image](https://github.com/user-attachments/assets/8e7a4f80-c21d-4c36-b5d4-672a98061a45)

  - Scroll down and under Networking, copy the IP address 

![image](https://github.com/user-attachments/assets/46d030e8-79e6-4dd0-89ff-d30761363294)

  - Open Remote Desktop Connections and paste the IP address -> click Show Options

![image](https://github.com/user-attachments/assets/0be535ad-4fe5-4c33-ab6f-5ec3fb83d5a5)

![image](https://github.com/user-attachments/assets/65f2abb9-d0be-4f8c-a2e9-a4761e07af55)

  - Fill username and click Connect

![image](https://github.com/user-attachments/assets/127448cd-9b3d-4c54-ac95-1db512e7f2cb)

  - Fill password and click OK -> click Yes

![image](https://github.com/user-attachments/assets/25841b2f-4421-4022-b3d8-ce96dfa67915)

![image](https://github.com/user-attachments/assets/3c3a34a0-0c49-40a7-806a-68f12b27ad1d)

  - Within Client-1, on the search box, type about and click About your PC

![image](https://github.com/user-attachments/assets/748eb2cb-4c6f-4ea3-b6d9-f6a854c19d63)

  - Within About your PC, click Rename this PC (advanced)

![image](https://github.com/user-attachments/assets/aa14cd17-05ec-4f62-b439-89c5244f7a55)

  - Click Change -> CLick Domain and type your domain name -> click OK

![image](https://github.com/user-attachments/assets/21013440-8b79-446d-ad03-923264f916a2)

![image](https://github.com/user-attachments/assets/ca73bfe3-8995-4d2a-a87e-15aa642fe458)

  - Fill in Jane Doe username and password -> Click OK -> Click OK -> Click OK

![image](https://github.com/user-attachments/assets/83182f70-8d0b-441b-adc0-05537598a18c)

![image](https://github.com/user-attachments/assets/bb35545a-428f-4208-8e47-b40be5a0196b)

![image](https://github.com/user-attachments/assets/f1c575ad-a7f0-4fbd-9243-a8d1132c2c98)

  - Click Restart Now

![image](https://github.com/user-attachments/assets/39a966eb-7646-4594-84d5-7d4aae8eb6f1)

<br />

<h5>Login to the Domain Controller and verify Client-1 shows up in Active Directory Users and Computers</h5>

![image](https://github.com/user-attachments/assets/bcb95af3-bcdd-4036-9347-7038d3a419df)

![image](https://github.com/user-attachments/assets/b39e459c-0d18-4445-91d6-0d9a306f87cf)

![image](https://github.com/user-attachments/assets/75edcc56-d07c-4253-b188-30b667fe7fcc)

![image](https://github.com/user-attachments/assets/bac5415b-03b0-409b-991d-f956ff91f83f)

  - Within DC-1, open Active Directory Users and Computers

![image](https://github.com/user-attachments/assets/fadff161-bdc8-4ef2-9498-a10d960b7894)

  -  Click drop down arrow besides Domain Name -> Computers -> see Client-1

![image](https://github.com/user-attachments/assets/b73c0b5e-f30f-4705-bce0-63ef800c7e7d)

<br />

<h5>Create a new Organizational Unit named “_CLIENTS” and drag Client-1 into there</h5>

  -  Right click on Domain name -> hover on New -> Click Organizational Unit

![image](https://github.com/user-attachments/assets/5f51f565-ec66-4b8b-bd9d-1b6cf8452444)

  - Name the new OU, _CLIENTS and click OK

![image](https://github.com/user-attachments/assets/6478e593-e87d-4e0f-a971-1bdb185e4c58)

  - Click Computers -> click and hold to drag Client-1 to _CLIENTS -> click Yes

![image](https://github.com/user-attachments/assets/8db47349-88f8-4941-899b-d930d8d763f9)

![image](https://github.com/user-attachments/assets/5b7265e2-5461-42f6-ad74-19f3057385ba)

![image](https://github.com/user-attachments/assets/70823c5a-e881-47ac-88ec-fda3fb31dd78)

<br />

<h4>Setup Remote Desktop for non-administrative users on Client-1</h4>

  - Log in to Client-1 as mydomain.com\janedoe
    
![image](https://github.com/user-attachments/assets/a12f430c-a522-4047-9c01-2fdb36f7f3f8)

![image](https://github.com/user-attachments/assets/a52a1d82-6777-4af2-80f4-73fcde5d0b94)

![image](https://github.com/user-attachments/assets/63534157-4311-4935-9e2e-bd5d5a23e02c)

  - Go to the search box -> type about -> click About Your PC -> click Remote desktop

![image](https://github.com/user-attachments/assets/546986be-9e2a-4504-8c2e-ede0aa4361f8)

![image](https://github.com/user-attachments/assets/3bcc36cb-a211-4479-8486-0a1c9d2c4476)

  - Click Select users that can remotely access this PC -> Click Add

![image](https://github.com/user-attachments/assets/54e4bcb7-d265-45d8-96b6-3043b0f4f0c7)

![image](https://github.com/user-attachments/assets/7e6ae3a3-857f-4d88-b2cb-90afa41f4223)

  - Type domain users -> click Check Names -> click OK -> Click OK

![image](https://github.com/user-attachments/assets/9b139c84-9bc3-424a-bf58-19135463bfad)

![image](https://github.com/user-attachments/assets/3c3fb4b2-6ff6-45e4-8c95-a125f3c229bc)

<br />

<h3>3. Creating Users with Powershell</h3>
<h4>Create additional users and attempt to log into client-1 with one of the users</h4>
<h4>*we will be running a script to create users to simulate a work setting with a lot of accounts*</h4>

  - Log in to DC-1 as Jane (Admin)

![image](https://github.com/user-attachments/assets/bcb95af3-bcdd-4036-9347-7038d3a419df)

![image](https://github.com/user-attachments/assets/b39e459c-0d18-4445-91d6-0d9a306f87cf)

![image](https://github.com/user-attachments/assets/75edcc56-d07c-4253-b188-30b667fe7fcc)

![image](https://github.com/user-attachments/assets/bac5415b-03b0-409b-991d-f956ff91f83f)

  - Within DC-1, type Powershell in Search box -> click PowerShell_ISE -> right click and click Run as Administrator

![image](https://github.com/user-attachments/assets/2425b649-27cc-46bc-821e-dee56682eec7)

  - Click New Script 

![image](https://github.com/user-attachments/assets/bdb5aa15-ce75-4b0f-bdde-6123665a0d3e)

  - Go to <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">Script</a>
  - Click Copy Raw File

![image](https://github.com/user-attachments/assets/be64265b-1c96-4841-9440-6b26505a88f6)

  - Go back to PowerShell and right click -> click Paste

![image](https://github.com/user-attachments/assets/d1165eb2-5e1c-4941-bc20-92e7dedf6e9e)

  - Click Run Script -> Observe the users being created -> Click Stop (whenever you feel you have enough users)

![image](https://github.com/user-attachments/assets/ff673227-bdc6-429c-8b81-ea846a334735)

![image](https://github.com/user-attachments/assets/7fdadcf1-777e-4e75-920d-2446b133c33a)

  - Once finished, open Active Directory Users and Computers and observe the accounts in the appropriate OU (_EMPLOYEES)

![image](https://github.com/user-attachments/assets/10ef47ea-2c06-4aeb-8173-4462cedd9181)

![image](https://github.com/user-attachments/assets/501451ad-9dda-4a3f-95c5-78374e470dfc)

<br />

<h4>Attempt to log into Client-1 with one of the accounts (take note of the password in the script)</h4>

  - Go to PowerShell -> check the password and take note for log in

![image](https://github.com/user-attachments/assets/94dbcab1-3d5a-4d86-8292-7c740d3a69e9)

  - Go to Active Directory Users and Computers -> check a user and take note for log in (you can choose anyone)

![image](https://github.com/user-attachments/assets/ed346b82-2abb-4ea5-8d27-f577288b6584)

  - Log in to Client-1 with a user of your choice (mydomain\username and Password1)

![image](https://github.com/user-attachments/assets/1ae32c2c-b51e-454d-a701-2f3a7d450317)

![image](https://github.com/user-attachments/assets/61c701ad-6978-44ac-89a0-e8d6d4310509)

![image](https://github.com/user-attachments/assets/0e926344-487f-4e8e-a075-d4fc126561b2)

<br />

<h2><a href="https://github.com/angelobreyes/ad-group-policies">Here's</a> the next part of this guide. </h2>
