<p align="center">
<img src="https://i.imgur.com/gKZkTaP.png" alt="VM AD logo"/>
</p>

<h1>Configuring Active Directory inside the Virtual Machine</h1>
This tutorial shows my deeper dive into the Configuring Active Directory inside the Virtual Machine<br />


<h2>Repersentation of Active Directory inside the Virtual Machine</h2>


<p>
  
<p>
<img src="https://i.imgur.com/ziGugGh.png" height="80%" width="80%" alt="AD VM"/>
</p>


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)
- PowerShell

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)
- Windows Server 2022 Datacenter

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)
- PowerShell 

<h2>Virtual Machine Deployment</h2>

<h3>Step 1: Create Azure Resources</h3>

- Log in to the Azure Portal.

- Create a new Resource Group (e.g., "MyResourceGroup").

- Create a new Virtual Network (VNet) in the same region as the Resource Group.

- Make note of the Resource Group and VNet names as you'll need them later.

<h3>Step 2: Configure DC-1 (Domain Controller)</h3>

- Create a Windows Server 2022 virtual machine named "DC-1" in the Resource Group and VNet created in Step 1.

- Set the NIC (Network Interface Card) Private IP address of DC-1 to a static IP.

- Ensure that DC-1 is in the same VNet as the Client-1.

<h3>Step 3: Ensure Connectivity</h3>

- Use Azure Network Watcher to confirm that both DC-1 and Client-1 are in the same VNet.

- Log in to Client-1 using Remote Desktop.

- Open Command Prompt and ping DC-1's private IP address using the command: ping -t <DC-1's IP address>.

- Log in to DC-1, open Windows Firewall settings, and enable ICMPv4 for pinging.

<p>
<img src="https://i.imgur.com/70SuOtT.png" height="80%" width="80%" alt="Domain Users in a VM"/>
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

<h3>Step 5: Create User Accounts in AD</h3>

- Open "Active Directory Users and Computers" (ADUC) on DC-1.
- Create an Organizational Unit (OU) named "_EMPLOYEES."
- Create another OU named "_ADMINS."
- Create a new employee user named "Jane Doe" with the username "jane_admin" and the same password.
- Add "jane_admin" to the "Domain Admins" Security Group.
- Log out of the Remote Desktop session and log back in as "mydomain.com\jane_admin."

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
- Use created script file a Name Generator Users.ps1.
- Run the script to create additional user accounts.
- Verify the accounts in the appropriate OU in ADUC.
- Attempt to log in to Client-1 with one of the newly created accounts using the respective password mentioned in the script. (Password1)


<h2>Testing Active Directory Inside VM</h2>

<h3>Testing the Active Directory (AD) setup involves various key steps and considerations:</h3>


<h4>User Authentication:</h4>
- Ensure users can log in with their domain accounts successfully.

<h4>Group Membership:</h4>
- Verify group-based access and privileges.

<h4>File and Print Sharing:</h4>
- Test sharing resources and permissions.

<h4>Group Policy:</h4>
- Apply and test policies for user and computer settings.

<h4>DNS Resolution:</h4>
- Confirm correct DNS name resolution.

<h4>AD Management:</h4>
- Use AD tools to manage users, groups, and OUs.

<h4>OU Structure:</h4>
- Ensure proper organization and Group Policy application.

<h4>Remote Desktop Access:</h4>
- Validate non-admin users' access via Remote Desktop.

<h4>Security Testing:</h4>
- Check password policies and security measures.

<h4>Backup and Restore:</h4>
- Test backup and recovery procedures.

<h4>Monitoring and Alerts:</h4>
- Set up monitoring for real-time issue detection.

Comprehensive testing helps ensure the reliability, security, and functionality of your AD environment.


<h2>My Closing Remarks</h2>

I have successfully set up an Active Directory environment on Azure, establishing a foundation for centralized user management and network authentication. I configured essential components such as domain controllers, user accounts, and organizational units. Then I tested user authentication, group memberships, and various aspects of the Active Directory setup to ensure its functionality and security.

Remember that maintaining and continuously monitoring an Active Directory infrastructure is essential to keep it secure and efficient. Regularly updating security policies, performing backups, and conducting routine testing are crucial steps to ensure the stability and reliability of any network.
