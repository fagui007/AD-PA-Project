### 4. Deploy and Configure Domain Controller

*Note: Ensure that Windows Server 2025 is fully installed on VM, including VMWare Tools.*
##### Initial Configuration
1. Configure **static IP** and **DNS server** in Servers (172.16.2.0/24) subnet.

	*Note: Reference IP Address Plan*
	
	![](../../screenshots/adpalab-32.png)
2. Ensure that you can reach the **Servers gateway** on the **Firewall**.
	
	*Note: We did not need to configure a rule for this because intrazone sessions are allowed by default.*
	
	![](../../screenshots/adpalab-33.png)
3. Change **computer name** then **Restart**.
	
	![](../../screenshots/adpalab-31.png)

##### Active Directory Domain Services Configuration

1. Navigate to: **Server Manager** > **Manage** > **Add Roles and Features** > Complete the Wizard
	
	![](../../screenshots/adpalab-35.png)

2.  Select **Role-based or feature-based installation**.
	
	![](../../screenshots/adpalab-359.png)
3. Select **DC-LAB** to install roles and features.
	
	![](../../screenshots/adpalab-380.png)
4. Select Server Roles > **Active Directory Domain Services** > Check **Include management tools**.
	
	![](../../screenshots/adpalab-34.png)
	
	![](../../screenshots/adpalab-08.png)
5. Leave **Features** as default > Next
6. On AD DS page > Next
7. **Install**
8. Promote the server to **Domain Controller**.

	![](../../screenshots/adpalab-356.png)
9. **Add a new forest** and create a **Root domain name** > Next

	![](../../screenshots/adpalab-100.png)
10. Create **Domain Services Restore Mode (DSRM) password** > Next
	
	![](../../screenshots/adpalab-62.png)
11. Leave **DNS Options** as default > Next
12. Leave **Additional Options** as default > Next
13. Leave **Paths** as default > Next
14. **Review** your selections > Next
15. After **Prerequisites Check** is complete > Install > Server will restart once installation is complete.
	
	![](../../screenshots/adpalab-83.png)
16. Log back in after restart.

##### Users and Groups Creation
1. Create new **Organization Unit (OU)** for **Lab Users**.
	- Open **Active Directory Users and Computers**.
	
	![](../../screenshots/adpalab-02.png)
	- Right-click your domain > New > Organizational Unit > Name > OK

		![](../../screenshots/adpalab-104.png)

		![](../../screenshots/adpalab-89.png)
2. Create new **Users**.
	- Right-Click **Lab Users** > **New** > **User**
	
	![](../../screenshots/adpalab-102.png)

	![](../../screenshots/adpalab-106.png)

	![](../../screenshots/adpalab-105.png)
	- **Users** to create.
		- labadmin1
		- labadmin2
		- labuser1
		- labuser2
		- paloaltoservice

    		*Note: We're creating this account ahead of time for two reasons: we will be using this account to configure **Administrators** for the firewall and we will also be using it to setup **User-ID**.*
3. Create new **OU** for **Lab Groups**.
	- Right-click your domain > **New** > **Organizational Unit** > **Name** > **OK**

		![](../../screenshots/adpalab-104.png)
	
		![](../../screenshots/adpalab-103.png)
4. Create new **Groups** within **Lab Groups** OU.
	- Right-click your group OU > **New** > **Group** > **Group Name** > **OK**

		![](../../screenshots/adpalab-101.png)
	
		![](../../screenshots/adpalab-82.png)
	- Create the following **Groups**:
		- Firewall Admin
		- Server Admin
		- Restricted User
		- Non-restricted User
5. Add **Users** to **Groups**
	- Navigate to: **Lab Users** OU > Right-Click a user > **Properties** > **Member Of**

   		![](../../screenshots/adpalab-388.png)
	- Enter Object (Group) Name > **Check Names** > Group Name should auto-populate

   		![](../../screenshots/adpalab-10.png)
	- Add **Groups** to following **Users**:
		- labadmin1 - Domain Admins, Server Admin

			![](../../screenshots/adpalab-09.png)
		- labadmin2 - Domain Admins, Firewall Admin

			![](../../screenshots/adpalab-15.png)
		- labuser1 - Non-restricted User

			![](../../screenshots/adpalab-389.png)
		- labuser2 - Restricted User

			![](../../screenshots/adpalab-393.png)
		- paloaltoservice - Administrators, Event Log Readers, Server Operators

			![](../../screenshots/adpalab-243.png)

##### Group Policy Configuration
1. Open **Group Policy Management Console**

	![](../../screenshots/adpalab-80.png)
2. Create a **Group Policy Object (GPO)**.
	- Expand Forest > Domains

		![](../../screenshots/adpalab-81.png)
	- Right-Click LAB.LOCAL (or your domain) > Select **Create a GPO in this domain, and Link it here...**
	- Name your new GPO

   		![](../../screenshots/adpalab-108.png)
3. Edit your new **GPO**
	- Right-Click the GPO > **Edit**
	- Navigate to: **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Windows Defender Firewall with Advanced Security**

   		![](../../screenshots/adpalab-71.png)
	
	- On Right-hand window > Select **Windows Defender Firewall Properties** > **Enable** Domain, Private, and Public Firewall

   		![](../../screenshots/adpalab-72.png)

	- In **Policy Path**, Right-click **Inbound Rules** > **New Rule**

		![](../../screenshots/adpalab-70.png)

	- Configure the following rule to allow inbound connections from network (172.16.0.0/16) > Finish.

		![](../../screenshots/adpalab-74.png)

		![](../../screenshots/adpalab-78.png)

   		![](../../screenshots/adpalab-76.png)

   		![](../../screenshots/adpalab-79.png)

   		![](../../screenshots/adpalab-73.png)

   		![](../../screenshots/adpalab-77.png)

   		![](../../screenshots/adpalab-75.png)

##### DNS Configuration
1. Open **DNS**.	

	![](../../screenshots/adpalab-51.png)
2. Expand Path > Right-Click **Forwarders** > **Properties** > **Edit**

	![](../../screenshots/adpalab-54.png)
3. Add **DNS-DHCP-LAB** IP from IP Address Plan.

	*Note: The server FQDN will not resolve until we deploy the server and join it to the domain.*

	![](../../screenshots/adpalab-55.png)

