<p align="center">
<img src="https://i.imgur.com/gKZkTaP.png" alt="VM AD logo"/>
</p>

<h1>Configuring Active Directory inside the Virtual Machine</h1>
This tutorial shows my deeper dive into the Configuring of Active Directory inside the Virtual Machine<br />


<h2>Repersentation of Active Directory inside the Virtual Machine</h2>


<p>
  
<p>
<img src="https://i.imgur.com/pG07kCh.png" height="80%" width="80%" alt="AD VM"/>
</p>


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)
- PowerShell

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)
- Windows Server 2022 Datacenter

<h2>Virtual Machine Deployment</h2>

<h3>Step 1: Create Azure Resources</h3>

- Log in to the Azure Portal.

- Create a new Resource Group (e.g., "MyResourceGroup").

- Create a new Virtual Network (VNet) in the same region as the Resource Group.

- Make note of the Resource Group and VNet names as you'll need them later.
<p>
<img src="https://i.imgur.com/YaeIK2N.png" height="80%" width="80%" alt="Domain Users in a VM"/>
</p>
<h3>Step 2: Configure DC-1 (Domain Controller)</h3>

- Create a Windows Server 2022 virtual machine named "DC-1" in the Resource Group and VNet created in Step 1.

- Set the NIC (Network Interface Card) Private IP address of DC-1 to a static IP.

- Ensure that DC-1 is in the same VNet as the Client-1.

<p>
<img src="https://i.imgur.com/4cHJHts.png" height="80%" width="80%" alt="Domain Users in a VM"/>
</p>
<h3>Step 3: Ensure Connectivity</h3>

- Use Azure Network Watcher to confirm that both DC-1 and Client-1 are in the same VNet.

- Log in to Client-1 using Remote Desktop.

- Open Command Prompt and ping DC-1's private IP address using the command: ping -t <DC-1's IP address>.

- Log in to DC-1, open Windows Firewall settings, and enable ICMPv4 for pinging.

<p>
<img src="https://i.imgur.com/gs1tJZe.png" height="80%" width="80%" alt="Domain Users in a VM"/>
</p>

<p>
<img src="https://i.imgur.com/I0uMSeR.png" height="80%" width="80%" alt="Domain Users in a VM"/>
</p>
<p>
<h2>Active Directory Setup</h2>

<h3>Step 4: Set Up Active Directory on DC-1</h3>

-  Log in to DC-1 using Remote Desktop.

Install Active Directory Domain Services:
- Open Server Manager.
- Add the "Active Directory Domain Services" role.

Promote DC-1 as a Domain Controller:
- In Server Manager, click on the notification flag and select "Promote this server to a domain controller."
- Choose "Add a new forest" and specify the domain name (e.g., mydomain.com).
- Follow the wizard to complete the promotion.
- After the promotion is complete, restart DC-1.
- Log back in as the user "mydomain.com\labuser."
<p>
<img src="https://i.imgur.com/UpwEfDv.png" height="80%" width="80%" alt="Domain Users in a VM"/>
</p>
<h3>Step 5: Create User Accounts in AD</h3>

- Open "Active Directory Users and Computers" (ADUC) on DC-1.
- Create an Organizational Unit (OU) named "_EMPLOYEES."
- Create another OU named "_ADMINS."
- Create a new employee user named "Jane Doe" with the username "jane_admin" and the same password.
- Add "jane_admin" to the "Domain Admins" Security Group.
- Log out of the Remote Desktop session and log back in as "mydomain.com\jane_admin."
<p>
<img src="https://i.imgur.com/UpwEfDv.png" height="80%" width="80%" alt="Domain Users in a VM"/>
</p>
<h3>Step 6: Join Client-1 to the Domain</h3>

- In the Azure Portal, set Client-1's DNS settings to point to DC-1's Private IP address.
- Restart Client-1 from the Azure Portal.
- Log in to Client-1 using Remote Desktop as the original local admin user (labuser).
- Join Client-1 to the domain (mydomain.com).
- The computer will restart automatically.
- Log in to DC-1 using Remote Desktop and verify that Client-1 appears in ADUC under the "Computers" container.

<h3>Step 7: Organize Clients in an OU (Optional)</h3>

- Create a new OU in ADUC named "_CLIENTS."
- Drag and drop Client-1 into the "_CLIENTS" OU for organizational purposes.

<h3>Step 8: Configure Remote Desktop Access on Client-1</h3>

- Log in to Client-1 as "mydomain.com\jane_admin."
- Open System Properties.
- Click "Remote Desktop."
- Allow "domain users" access to remote desktop.

<h3>Step 9: Create Additional Users and Log In</h3>

- Log in to DC-1 as "jane_admin."
- Open PowerShell ISE as an administrator.
- Use the created script file a Name Generator Users.ps1.
- Run the script to create additional user accounts.
- Verify the accounts in the appropriate OU in ADUC.
- Attempt to log in to Client-1 with one of the newly created accounts using the respective password mentioned in the script. (Password1)
<p>
<img src="https://i.imgur.com/JH4o8o6.png" height="50%" width="50%" alt="Domain Users in a VM"/>
</p>

<h3>Step 10 Is Testing, I Tested this AD in another project, See: AD Testing</h3>

<h2>My Closing Remarks</h2>

Setting up Active Directory in a Microsoft Azure Virtual Network with a domain controller and a domain-joined machine can provide you with a robust and centralized solution for managing your Windows-based network resources in the Cloud. This configuration allows for streamlined user management, enhanced security, and the ability to enforce group policies and access controls.

As you continue to work with this setup, consider implementing additional best practices and security measures to protect your network and data. Regularly update and patch your virtual machines, back up critical data, and monitor network traffic for any suspicious activities. Azure provides various security features and tools to help you maintain the integrity and availability of your Active Directory environment.

Furthermore, keep yourself updated with the latest developments in Azure and Active Directory to leverage new features and capabilities that can enhance the efficiency and security of your network.

Remember that Active Directory in Azure can scale to meet the needs of your organization, whether you have a small network or a large enterprise environment. Proper planning, configuration, and maintenance will ensure a smooth and secure operation of your Active Directory infrastructure in the Azure cloud.
