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

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<h3>Step 1: Setup two virtual machine in Azure</h3>

- Create two virtual machines DC-1 & Client-1
You can create resource group in the sametime while creating virtul machine DC-1
- The first virtual machine we create is the Domain Controller
	- Name: DC-1 and Image is Windows Server 2022 
	-- The second virtual machine will be the Client
		- Name: Client-1
		- Image: Windows 10 Pro
		- Use the same resource group and vNet as DC-1
 Again just like DC-1 take note of the virtual network (vNet) that is automatically created
 Make sure both virtual machines have matching locations.
  
       
<p>
<img src="https://i.imgur.com/rJuIlEb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Set DC-1's Virtual Network Interface Card (VNIC) private IP address to be static
- Go to DC-1's network settings
- Select Networking

<p>
<img src="https://i.imgur.com/IMtE0q3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
- Select IP Configurations > ipconfig1	

<p>
<img src="https://i.imgur.com/8vxGh4n.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>


- Change the assignment from dynamic to static

<p>
<img src="https://i.imgur.com/vleCSRa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Double check in the IP configurations that DC-1 Private IP addreess is set to Static


<p>
<img src="https://i.imgur.com/hna2HnS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Step 2: Ensure Connectivity Between the Client and Domain Controller</h3>

- Login to Client-1 using Microsoft Remote Desktop
- Search for Command Prompt and open it
- Ping DC-1's private IP Address (10.0.0.4)
- Type "ping -t 10.0.0.4 into the command-line interface
- The ping request continually  times out due to the firewall settings
- To fix this, we need to enable ICMPv4 on DC-1's local Windows firewall



<p>
<img src="https://i.imgur.com/UmmwhJ2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

 - Login in to DC-1 remote desktop connection

<p>
<img src="https://i.imgur.com/KxU3pBO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Start > Windows Administrative Tools > Windows Defender Firewall with Advanced Security > Inbound Rules
- You may need to scroll to right to find protocols section
- Find "Core Networking Diagnostics" and "ICMPv4" and enable these two inbound rules


<p>
<img src="https://i.imgur.com/KJ7x4lL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Log back into Client-1 and visually inspect the command line and you should notice that Client-1 will automatically begin pinging DC-1 

<p>
<img src="https://i.imgur.com/X019WXQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Step 3: Install Active Directory</h3>

- Log back into DC-1
- Open Server Manager
- Select "Add Roles and Features" > Follow the prompts
- At Server Roles, check "Active Directory Domain Services."
	

<p>
<img src="https://i.imgur.com/VYUcgHJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Select Add Features & select Next
- Complete the installation

<p>
<img src="https://i.imgur.com/FIbc0MM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>


- At the top right of the Server Manager Dashboard, click on the flag
- Select "Promote This Server to a Domain Controller."
	
<p>
<img src="https://i.imgur.com/p0cd3At.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Select "Add a New Forest."
- Root domain name: mydomain.com and select next after

<p>
<img src="https://i.imgur.com/4DY6pgY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Create a password & Select Next and follow the prompts
- Select Install to complete the installation

<p>
<img src="https://i.imgur.com/CAxowFh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- DC-1 should automatically restart
- Proceed to log back into DC-1 as user: mydomain.com\labuser	

<p>
<img src="https://i.imgur.com/4GdWkSY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Step 4: Create an Admin and Normal User Account in Active Directory v1.15.8</h3>
     
- On DC-1, there’s two options to open Active Directory Users and Computers (ADUC)
- Option 1 on your screen go to the bottom left corner next to start menu there’s a search bar type (Active Directory Users and Computers)
- Second option is to open Server Manager
 (Click tools) at the top-right of the screen.
- Select Active Directory Users and Computers
           
<p>
<img src="https://i.imgur.com/mCKSxxR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Right-click mydomain.com > New > Select Oranizational Unit (OU)
- Create two OUs Oranizational Unit 
	
<p>
<img src="https://i.imgur.com/88yXQXw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Name the first "_EMPLOYEES" 
	
<p>
<img src="https://i.imgur.com/rKUrPAH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Name the second "_ADMINS"	

<p>
<img src="https://i.imgur.com/GsYHRzb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Right-click mydomain.com and click Refresh to sort the new organizational units to the top
- Go to the _ADMINS OU
- Right-click the name of the OU > New > User
	- First/Last name: Tayab Ahmed
	- User login name: tayab_admin
	- Select Next
	- Create a password ********
	- Uncheck all boxes
	- Select Next and then select Finish

<p>
<img src="https://i.imgur.com/iOAcHHO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Go to the _ADMINS OU
- Right-click Tayab Ahmed & select Properties
- Click the tab named "Member of" > select Add
- Type in the names of your domain administrators
- Select "Check Names" > OK > Apply

<p>
<img src="https://i.imgur.com/EgaLI2I.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Log out of DC-1 as "labuser" and log back in as “mydomain.com\tayab_admin”

<p>
<img src="https://i.imgur.com/RnowLm0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Step 5: Join Client-1 to your domain (mydomain.com)
</h3>

- Log back into Client-1 using Microsoft Remote Desktop as the original local admin (labuser)
- Right-click the Start menu and select System
- On right-hand side of the screen, select Rename This PC (Advanced) > Change

<p>
<img src="https://i.imgur.com/arBWiRk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Under "Member of" select Domain
- Type "mydomain.com" and select OK
- Notice a tab will pop up on the screen saying DC-1 for domain.com could not be contatced this is becuase Client-1 private IP address is not set to DC-1 in the DNS Server	

<p>
<img src="https://i.imgur.com/8CChdyH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
	
- On Client-1 open the commoand line type in "Ipconfig/all" 
- Scorll down and look for the DNS server then observe the ip address numbers
	
<p>
<img src="https://i.imgur.com/WRRulhg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Go back to the Azure portal
- Navigate to the Client-1 Virtual Machine
- On the left-hand side of the screen select Networking
- Select the link next to the NIC > select DNS Server > Custom
- Type in DC-1's private IP address
- Click Save
- After it is done updating, select Restart and select Yes	

<p>
<img src="https://i.imgur.com/2lz8dC9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- On Client-1 azure portal click on the restart button shown below 	

<p>
<img src="https://i.imgur.com/lsAQwLZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- On Client-1 open the commoand line type in "Ipconfig/all" 
- Scorll down and look for the DNS server then observe the ip address numbers again, mines say "10.0.0.4"	

<p>
<img src="https://i.imgur.com/17jQZ4P.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
	
- Log back into Client-1 using Microsoft Remote Desktop as the original local admin (labuser)
- Right-click the Start menu and select System
- On right-hand side of the screen, select Rename This PC (Advanced) > Change
- Under "Member of" select Domain
- Type "mydomain.com" and select OK
- Username: mydomain.com\tayab_admin "save username and password on your notepad so you will not forget"
- Type in password and press OK
- Restart the computer 	
	
<p>
<img src="https://i.imgur.com/fy64xoz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- A tab should open automatically saying "Welcome to the domain.com domain,"
	
<p>
<img src="https://i.imgur.com/cK16S3e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Log back into Client-1
- Use mydomain.com\tayab_admin
	
<p>
<img src="https://i.imgur.com/bC27j6v.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Right-click the Start menu and select System	

<p>
<img src="https://i.imgur.com/EpA2uq5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- On the right-hand side of the screen, select Remote Desktop
- Under User Accounts, click "Select Users That Can Remotely Access This PC > select Add
- Type in the name of your domain users
- Select "Check Names" click OK and OK again

<p>
<img src="https://i.imgur.com/bAhJDum.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 7: Create as many additional users as you would like and attempt to log into Client-1 with one of the users' profiles
- Log back into DC-1 as tayab_admin
- Go to Active Directory Users and Computers "mydomain.com" > _EMPLOYEES this is where we observe users being created once we use Powershell script to run.
	
<p>
<img src="https://i.imgur.com/rthoWGF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Search for "PowerShell Ise"
- Right-click on PowerShell Ise and open it as an administrator.
	
<p>
<img src="https://i.imgur.com/mzclcTK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- At the top-left of the screen select New Script and paste the contents of the following script into it
- You can find the powershell script [here](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)

<p>
<img src="https://i.imgur.com/sJ5pirD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/HRnDObP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/zFxxCdO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/EcnBI7d.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/6c9wRHV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/EZgbg3o.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/YGvIDlf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/1U5s2VS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/asCXluH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/fOl2DQY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
