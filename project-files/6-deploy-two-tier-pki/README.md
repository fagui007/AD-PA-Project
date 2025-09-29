### 6. Deploy Two-Tier PKI

*Note: We'll be configuring ROOT-LAB first, then we'll configure ISSUING-LAB*
##### ROOT-LAB
1. Configure **static IP** and **DNS server** for **ROOT-LAB** in Servers (172.16.2.0/24) subnet.

	*Note: Reference IP Address Plan - ROOT-LAB*
	- IP Address: 172.16.2.4
	- Subnet Mask: 255.255.255.0
	- Default Gateway: 172.16.2.1
	- DNS Server: 172.16.2.2
2. Ensure that you can reach the **Servers gateway** on the **Firewall**.

	![](../../screenshots/adpalab-33.png)
3. Change **computer name** then **Restart**.

	*Note: For security purposes, we will not be joining this server to our domain. It will be a standalone server.*

	![](../../screenshots/adpalab-360.png)
4. Install **Active Directory Certificate Services** (ADCS).
	- Navigate to: **Server Manager** > **Manage** > **Add Roles and Features** > Complete the Wizard
	- Select **Role-base or feature-based installation**

		![](../../screenshots/adpalab-363.png)
	- Select **ROOT-LAB** to install roles and features.

		![](../../screenshots/adpalab-368.png)
	- Select Server Roles > **Active Directory Certificate Services** > Check **Include management tools**.

		![](../../screenshots/adpalab-366.png)

		![](../../screenshots/adpalab-364.png)
	- Leave **Features** as default > Next
	- AD CS > Next
	- Select **Role Services** > Next

		![](../../screenshots/adpalab-367.png)
	- Install **AD CS**

		![](../../screenshots/adpalab-362.png)
5. Configure **AD CS** on **ROOT-LAB**
	- Start configuration.

		![](../../screenshots/adpalab-29.png)
	- **Credentials** > Leave as default > Next 

		![](../../screenshots/adpalab-361.png)
	- Select **Role Services** > Next

		![](../../screenshots/adpalab-369.png)
	- Select **Setup Type** > Standalone CA > Next

		![](../../screenshots/adpalab-112.png)
	- Select **CA Type** > **Root CA** > Next

		![](../../screenshots/adpalab-113.png)
	- **Create a new private key** > Next

		![](../../screenshots/adpalab-114.png)
	- Specify **cryptographic options** > Next

		![](../../screenshots/adpalab-115.png)
	- Specify **CA Name** (left as default)> Next

		![](../../screenshots/adpalab-116.png)
	- Specify **Validity Period** > Next

		![](../../screenshots/adpalab-117.png)
	- Leave **Certificate Database** as default > Next

		![](../../screenshots/adpalab-118.png)
	- Finish Configuration > Close

		![](../../screenshots/adpalab-119.png)

		![](../../screenshots/adpalab-120.png)

		![](../../screenshots/adpalab-121.png)
6. Continue configuration on **Certification Authority**.
	- Open **Certification Authority** app.

		![](../../screenshots/adpalab-122.png)
	- Right-Click your server name > Properties

		![](../../screenshots/adpalab-123.png)
	- In Properties, navigate to **Extensions** tab.

		![](../../screenshots/adpalab-124.png)
	- Add a new **CRL Distribution Point (CDL)**
		- Click **Add** > The location that we will be adding is: `http://certs.lab.local/CertEnroll/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`

			*Note: We created a CNAME in DNS on DC-LAB that points certs.lab.local to ISSUING-LAB*

			![](../../screenshots/adpalab-125.png)
		- Ensure both boxes are checked so CDP can be reached on ISSUING-LAB

			![](../../screenshots/adpalab-126.png)

			***Ensure that the http address is added. If not, it will result in errors later in the lab.***
	- Add a new **Authority Information Access (AIA)**
		- Dropdown **Select extension** > Select AIA

			![](../../screenshots/adpalab-127.png)
		- Click **Add** > The location that we will be adding is: `http://certs.lab.local/CertEnroll/<ServerDNSName>_<CaName><CertificateName>.crt`

			![](../../screenshots/adpalab-128.png)
		- Select the following options > **OK**

			![](../../screenshots/adpalab-129.png)

			***Ensure that the http address is added. If not, it will result in errors later in the lab.***
	- Restart **Certification Authority services**
		- After clicking OK, you will be prompted to restart the AD CS service > Yes.

			![](../../screenshots/adpalab-130.png)
7. Change **Certificate Validity** and **CRL Validity** expiration periods.
	- Open **Command Prompt** as an **administrator**

		![](../../screenshots/adpalab-163.png)
	- Change the **CRL Validity** period.

		`certutil -setreg CA\CRLPeriodUnits 20`

		`certutil -setreg CA\CRLPeriod "Years"`

		![](../../screenshots/adpalab-164.png)
	- Change the **Certificate Validity** period.

		`certutil -setreg CA\ValidityPeriodUnits 10`

		`certutil -setreg CA\ValidityPeriod "Years"`

		![](../../screenshots/adpalab-166.png)
	- Restart **certificate services**
		`net stop certsvc`
		`net start certsvc`

		![](../../screenshots/adpalab-165.png)

##### ISSUING-LAB
1. Configure **static IP** and **DNS server** for **ISSUING-LAB** in Servers (172.16.2.0/24) subnet.

	*Note: Reference IP Address Plan - ISSUING-LAB*
	- IP Address: 172.16.2.5
	- Subnet Mask: 255.255.255.0
	- Default Gateway: 172.16.2.1
	- DNS Server: 172.16.2.2
2. Ensure that you can reach the **Servers gateway** on the **Firewall**.

	![](../../screenshots/adpalab-33.png)
3. Change **computer name** and **join LAB.LOCAL domain**, use **labadmin** credentials when prompted, then **Restart**.

	![](../../screenshots/adpalab-131.png)

	![](../../screenshots/adpalab-132.png)

	![](../../screenshots/adpalab-133.png)
	- Once restarted, log in with **labadmin** account.
4. Add a **CNAME** on **DNS** that points to **ISSUING-LAB**
	- Log into **DC-LAB** as a **labadmin**.
	- Open **DNS** app.
	- Expand path to **LAB.LOCAL**

		![](../../screenshots/adpalab-178.png)
	- Right-Click **LAB.LOCAL** > Select **New Alias (CNAME)**

		![](../../screenshots/adpalab-179.png)
	- Set **Alias name** to **certs** and **FQDN** to **ISSUING-LAB.LAB.LOCAL** > OK

		![](../../screenshots/adpalab-180.png)
	- Verify **CNAME** by pinging **certs.lab.local**

		![](../../screenshots/adpalab-181.png)
5. Install **Active Directory Certificate Services** (ADCS).
	 Navigate to Server Manager > Manage > Add Roles and Features > Complete the Wizard
	- Select **Role-base or feature-based installation**

		![](../../screenshots/adpalab-134.png)
	- Select **ISSUING-LAB** to install roles and features.

		![](../../screenshots/adpalab-135.png)
	- Select Server Roles > **Active Directory Certificate Services** > Check **Include management tools**.

		![](../../screenshots/adpalab-136.png)

		![](../../screenshots/adpalab-364.png)
	- Leave **Features** as default > Next
	- AD CS > Next
	- Select **Role Services** > Check **Include Management tools**> Next

		![](../../screenshots/adpalab-138.png)

		![](../../screenshots/adpalab-137.png)
	- Web Server Role (IIS) > Next
	- Role Services, leave as default > Next
	- Install AD CS

		![](../../screenshots/adpalab-139.png)
6. Configure **AD CS** on **ISSUING-LAB**
	- Start configuration.
	
		![](../../screenshots/adpalab-140.png)
	- **Credentials** > Leave as default > Next 

		![](../../screenshots/adpalab-141.png)
	- Select **Role Services** > Next

		![](../../screenshots/adpalab-142.png)
	- Select **Setup Type** > Enterprise CA > Next

		![](../../screenshots/adpalab-143.png)
	- Select **CA Type** > **Subordinate CA** > Next

		![](../../screenshots/adpalab-144.png)
	- **Create a new private key** > Next

		![](../../screenshots/adpalab-145.png)
	- Specify **cryptographic options** > Next

		![](../../screenshots/adpalab-146.png)
	- Specify **CA Name** (left as default)> Next

		![](../../screenshots/adpalab-147.png)
	- Create **Certificate Request** > Next

		*Take note of **where** the certificate request is being saved and under what **name**.*

		![](../../screenshots/adpalab-148.png)
	- Leave **Certificate Database** as default > Next

		![](../../screenshots/adpalab-149.png)
	- Finish Configuration > Close

		![](../../screenshots/adpalab-150.png)

		![](../../screenshots/adpalab-151.png)
7. Obtain an **Issuing CA certificate** by submitting **certificate request** to **Root CA**, which is **ROOT-LAB**.
	- Log into **ROOT-LAB** with administrator account.
	- Open **File Explorer** and navigate to the **Local Disk (C:)**

		![](../../screenshots/adpalab-152.png)
	- Create a new folder > Right-click, New, Folder > Name: **Issuing CA**

		![](../../screenshots/adpalab-153.png)
	- Open a new **File Explorer** window and navigate to **`\\ISSUING-LAB\C$`**

		![](../../screenshots/adpalab-154.png)
	- Copy the **certificate request** and paste it into the newly created **Issuing CA** folder on the **Local Disk (C:)** for **ROOT-LAB**

		![](../../screenshots/adpalab-155.png)

		*Note: Ensure that you do not choose **Paste Shortcut***

		![](../../screenshots/adpalab-156.png)

		![](../../screenshots/adpalab-157.png)
	- Open **Certification Authority** app
	- Right-click your server > **Submit New Request**

		![](../../screenshots/adpalab-158.png)
	- Navigate to `C:\Issuing CA` > Select **certificate request** from **ISSUING-LAB**

		![](../../screenshots/adpalab-159.png)
	- Navigate to **Pending Requests**

		![](../../screenshots/adpalab-160.png)
	- Right-click the request > **All Tasks** > **Issue**

		![](../../screenshots/adpalab-161.png)

		The certificate should be issued now under **Issued Certificates**.
	- Navigate to **Issued Certificates** > Right-click certificate > **Open**

		![](../../screenshots/adpalab-162.png)

		![](../../screenshots/adpalab-167.png)

		*Note: The validity period for the certificate is 10 years because we configured that earlier. Default is typically one year.*
	- Navigate to **Details** tab > **Copy to File**

		![](../../screenshots/adpalab-168.png)
	- Export the certificate to **Issuing CA** folder.

		![](../../screenshots/adpalab-169.png)

		![](../../screenshots/adpalab-170.png)

		![](../../screenshots/adpalab-171.png)

		![](../../screenshots/adpalab-172.png)
8. Copy **Issuing CA** folder to **`\\ISSUING-LAB\C$`**

	Before copying the folder over, we need to copy the CRL from **ROOT-LAB**.
	- On **ROOT-LAB** navigate to **`C:\Windows\System32\CertSrv\CertEnroll`**

		![](../../screenshots/adpalab-173.png)
	- **Copy** the two files > **Paste** into **Issuing CA** folder in **ROOT-LAB**

		![](../../screenshots/adpalab-174.png)
	- **Copy Issuing CA** folder and **Paste** into **`\\ISSUING-LAB\C$`**

		![](../../screenshots/adpalab-175.png)
	- We can now **Power Off** and **secure** **ROOT-LAB** server. All certificates will be issued by **ISSUING-LAB**.
9. On **ISSUING-LAB**, finish **Certification Authority** configuration.
	- Copy **ROOT-LAB** files from **Issuing CA** folder to **`C:\Windows\System32\CertSrv\CertEnroll`** on **ISSUING-LAB**

		![](../../screenshots/adpalab-176.png)

		![](../../screenshots/adpalab-177.png)

		***ENSURE THAT THIS STEP IS COMPLETED BEFORE MOVING ON.***

		*If not completed, you will receive certificate revocation errors.*

		*If you have completed this step, but are still receiving this error then it's possible that either the AIA or CDL, or both, did not contain the correct http address pointing to ISSUING-LAB. If you're pointing to a CNAME, ensure that it's created on DNS.*
	- Open **Certification Authority** app
	- Right-click **CA server** > **All Tasks** > **Install CA Certificate**

		![](../../screenshots/adpalab-182.png)
	- Select **Issuing Certificate** that we placed in the **Issuing CA** folder.

		![](../../screenshots/adpalab-183.png)
	- Right-click CA Server > **Start Service**

		![](../../screenshots/adpalab-184.png)
	- Confirm that the server is up (Green checkmark)

		![](../../screenshots/adpalab-185.png)
10. Edit **Certificate Templates**
	- Expand path to **Certificate Templates**.

		![](../../screenshots/adpalab-186.png)
	- Delete all default templates in the folder.

		*Note: We will be creating/duplicating three templates for the lab.*

		![](../../screenshots/adpalab-193.png)
	- Right-click **Certificate Templates** > Manage

		![](../../screenshots/adpalab-192.png)
	- In the **Certificate Templates Consoles**, **Duplicate** and **Edit** the following templates:
		- Workstation Authentication
			- Right-click template > **Duplicate Template**

				![](../../screenshots/adpalab-194.png)
			- Navigate to **General** tab > **Rename** template duplicate

				![](../../screenshots/adpalab-195.png)
			- Navigate to **Security** tab > Select **Domain Computers** > Select **Allow** for **Autoenroll**

				![](../../screenshots/adpalab-196.png)
			- Click **OK** > Verify **Lab Workstation Authentication** template exists.

				![](../../screenshots/adpalab-197.png)
		- Web Server
			- Right-click template > **Duplicate Template**

				![](../../screenshots/adpalab-198.png)
			- Navigate to **General** tab > **Rename** template duplicate

				![](../../screenshots/adpalab-199.png)
			- Navigate to **Security** tab > Add **ISSUING-LAB**  > Select **Allow** for **Enroll**

				![](../../screenshots/adpalab-207.png)
				- Add **Computers Object Type**

				![](../../screenshots/adpalab-208.png)

				![](../../screenshots/adpalab-209.png)
	
				![](../../screenshots/adpalab-210.png)
			- Click **OK** > Verify **Lab Web Server** template exists.

				![](../../screenshots/adpalab-200.png)
		- Subordinate CA
			- Right-click template > **Duplicate Template**

				![](../../screenshots/adpalab-201.png)
			- Navigate to **General** tab > **Rename** template duplicate

				![](../../screenshots/adpalab-202.png)
			- Click **OK** > Verify **Lab Subordinate Certification Authority - Firewall** template exists.

				*Note: We will be using this template later in the lab to setup SSL Decryption on the firewall.*

				![](../../screenshots/adpalab-203.png)
		- Close out of **Certificate Templates Console**
	- Right-click **Certificate Templates** > **New** > **Certificate Template to Issue**

		![](../../screenshots/adpalab-204.png)
	- Find the duplicated certificate templates > Select > OK

		![](../../screenshots/adpalab-205.png)
	- Verify the certificate templates have been added.

		![](../../screenshots/adpalab-206.png)
11. Issue Web Server certificate for **`certs.lab.local`**
	- On **ISSUING-LAB**, search for **Manage computer certificates** and open > Yes.

		![](../../screenshots/adpalab-187.png)\

		![](../../screenshots/adpalab-188.png)
	- Expand path: Personal > Certificates

		![](../../screenshots/adpalab-189.png)
	- Right-click **Personal** > **All Tasks** > **Request New Certificate**

		![](../../screenshots/adpalab-190.png)
	- Select **Certificate Enrollment Policy** > Next

		![](../../screenshots/adpalab-191.png)
	- Select **Lab Web Server** > Next

		![](../../screenshots/adpalab-211.png)

		*Note: If Lab Web Server doesn't appear, ensure that ISSUING-LAB is allowed to enroll certificates in the Certificate Template Console settings for the Lab Web Server template.*
	- Expand details > Select **Properties**

		![](../../screenshots/adpalab-212.png)
	- Add **Common Name** and **DNS** > OK

		![](../../screenshots/adpalab-213.png)
	- **Enroll** > Finish

		![](../../screenshots/adpalab-214.png)

		![](../../screenshots/adpalab-215.png)
12. Configure **Internet Information Services (IIS) Manger** for **`certs.lab.local`**
	- Open IIS Manager

		![](../../screenshots/adpalab-216.png)
	- Expand **Connections** path to **Default Web Site**

		![](../../screenshots/adpalab-217.png)
	- Right-click **Default Web Site** > Select **Edit Bindings**

		![](../../screenshots/adpalab-218.png)
	- In **Site Bindings** > **Add**

		![](../../screenshots/adpalab-219.png)
	- Change **Type** to **https** and select **certs.lab.local** for **SSL certificate** > OK

		![](../../screenshots/adpalab-220.png)
	- Verify that **`https://certs.lab.local`** shows a secure connection.

		![](../../screenshots/adpalab-221.png)

		*Note: We will be using this site to issue a certificate for the firewall.*

##### DC-LAB
1.  Verify client certificate and **ROOT-LAB-CA** certificate do not exist (I tested this on **DNS-DHCP-LAB** server).
	- Open computer certificates > **Manage computer certificates**

		![](../../screenshots/adpalab-230.png)
	- Open **Personal** folder > Verify that there are no objects.

		![](../../screenshots/adpalab-231.png)
	- Open **Trusted Root Certification Authorities** > **Certificates** folder > Verify that **ROOT-LAB-CA** certificate does not exist.

		![](../../screenshots/adpalab-232.png)
2. Configure **GPO** to push root certificate as a **trusted root CA**.
	- Log in to **DC-LAB** as a **labadmin**.
	- Open **Group Policy Management** app

		![](../../screenshots/adpalab-222.png)
	- Expand path to lab domain.

		![](../../screenshots/adpalab-223.png)
	- Right-Click LAB.LOCAL (or your domain) > Select **Create a GPO in this domain, and Link it here...**

		![](../../screenshots/adpalab-235.png)
	- Name your new GPO > **OK**

		![](../../screenshots/adpalab-236.png)
	- Right-click your newly created GPO > **Edit**

		![](../../screenshots/adpalab-237.png)
	- Expand path to **Trusted Root Certification Authorities**

		![](../../screenshots/adpalab-238.png)
	- Right-click the folder > **Import**

		![](../../screenshots/adpalab-239.png)
	- Open path to Root CA certificate > Open

		![](../../screenshots/adpalab-240.png)
	- Complete import.
3. Verify **Root CA** certificate is present.
	- Log in to **DNS-DHCP-LAB** > Open **Command Prompt** > run **gpupdate /force**

		![](../../screenshots/adpalab-234.png)
	- Open **computer certificates** >**Trusted Root CA**s folder > Verify **ROOT-LAB-CA** is present.

		![](../../screenshots/adpalab-241.png)
4. Configure **GPO auto-enrollment** to push certificates to clients.
	- Log in to **DC-LAB** as a **labadmin**.
	- Open **Group Policy Management** app
	- Expand path to lab domain.

		![](../../screenshots/adpalab-223.png)
	- Right-Click LAB.LOCAL (or your domain) > Select **Create a GPO in this domain, and Link it here...**

		![](../../screenshots/adpalab-224.png)
	- Name your new GPO > OK

		![](../../screenshots/adpalab-225.png)
	- Right-click your newly created GPO > Edit

		![](../../screenshots/adpalab-403.png)
	- Expand path to **Public Key Policies**
 
		![](../../screenshots/adpalab-227.png)
	- Select the **Auto-Enrollment** object type > Right-click > Open Properties

		![](../../screenshots/adpalab-228.png)
	- Enable the policy.

		![](../../screenshots/adpalab-229.png)
	- Check the two options to allow certificate updates > OK

		![](../../screenshots/adpalab-233.png)
5. Verify client certificate was issued.
	 - Log in to **DNS-DHCP-LAB** > Open **Command Prompt** > run **gpupdate /force**

		![](../../screenshots/adpalab-234.png)
	- Open **computer certificates** >**Personal**s folder > Verify client certificate is present.

		![](../../screenshots/adpalab-242.png)


**Completed PKI Setup**

![](../../screenshots/adpalab-355.png)

