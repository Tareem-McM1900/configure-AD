<p align="center">
<img src="https://i.imgur.com/gKZkTaP.png" alt="VM AD logo"/>
</p>

<h1>Configuring Active Directory inside the Virtual Machine</h1>
This tutorial deeper dive into the Configuring Active Directory inside the Virtual Machine<br />


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

<h2>Virtual Machine Deployment</h2>




<p>
<img src="https://i.imgur.com/70SuOtT.png" height="80%" width="80%" alt="Domain Users in a VM"/>
</p>
<p>
Active Directory (AD) is a directory service developed by Microsoft for managing and organizing resources in a network. In a Microsoft Azure Virtual Network environment, you can set up Active Directory using virtual machines (VMs). Here's how I set up AD with one virtual machine as the domain controller (DC-1) and a second virtual machine named Client-1, while configuring Client-1's DNS server to point to DC-1:
  
1.	Create Virtual Machines:
   
   	First, create two virtual machines within your Azure Virtual Network. You can do this through the Azure Portal or by using Azure PowerShell or Azure CLI.

2.	Install Windows Server:
   
   	On both DC-1 and Client-1, install the Windows Server operating system. Ensure that you have an appropriate Windows Server license for each VM.

3.  Configure Static IP Addresses:

    Assign static IP addresses to both DC-1 and Client-1. You can do this within the Azure Portal or by configuring the network settings in each VM.

4.  Promote DC-1 to Domain Controller:

    On DC-1, open the Server Manager, and click on "Add roles and features."
    
    Follow the wizard to install the Active Directory Domain Services (AD DS) role.
    
    After the role installation is complete, promote DC-1 to a domain controller by running the "Promote this server to a domain controller" wizard.
    
    Create a new Active Directory forest or join an existing one, depending on your requirements.

5.	Configure DNS on DC-1:

    After promoting DC-1 to a domain controller, configure the DNS server role on it. AD DS typically integrates with DNS.

  	Make sure that the DNS service on DC-1 is running and properly configured to support Active Directory.

7.	Join Client-1 to the Domain:

    On Client-1, go to the network settings and set the DNS server IP address to be the static IP address of DC-1.

  	To join Client-1 to the Active Directory domain managed by DC-1, you can:

  	 > Right-click on "This PC" (or "My Computer"), select "Properties."

  	 > Click on "Change settings" under the "Computer name, domain, and workgroup settings" section.

  	 > Click "Change" to join a domain, enter the domain name, and follow the prompts to join.

<h2>Testing Active Directory</h2>




<h2>My Closing Remarks</h2>
