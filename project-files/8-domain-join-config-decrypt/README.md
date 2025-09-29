### 8. Domain Join Clients and Configure Decryption
##### Domain Join
1. Power on client virtual machines that were created earlier.
2. Log in to each client with administrator credentials.
3. Confirm **IP address** was configured via **DHCP**
	- Open **Command Prompt** and run `ipconfig /all`

		![](../../screenshots/adpalab-276.png)
4. Open System Properties > Rename and join lab domain > Restart

	![](../../screenshots/adpalab-277.png)

	![](../../screenshots/adpalab-278.png)
5. Log into either client as **labadmin2** > Move onto next part

	*Note: You can log into both, but only one PC is needed for the next part.*

##### Create Certificates for Decryption

We'll be doing this portion through the Client VM because it will be easier to generate and issue a certificate for decryption.
1. From Client VM, navigate to firewall management: 172.16.3.2

	![](../../screenshots/adpalab-279.png)
2. Navigate to: **Device** > **Certificate Management** > **Certificates** > **Device Cer**

	![](../../screenshots/adpalab-280.png)
3. Import **Root** and **Issuing** Certificates
	- *Note: You can export the certificates from the client's **Computer Certificates***
		- Open **Manage Computer Certificates**.
		- Navigate to: **Trusted Root Certificate Authorities** and **Intermediate Certification Authorities** > **Certificates**

			![](../../screenshots/adpalab-286.png)

			![](../../screenshots/adpalab-287.png)

			![](../../screenshots/adpalab-281.png)

			![](../../screenshots/adpalab-285.png)
		- Right-click the certificates > All Tasks > Export > Export the certificates as Base-64 format > Save

			![](../../screenshots/adpalab-282.png)
	- Select **Import** > **Name** Certificate > **Browse** and select exported certificate > OK

		![](../../screenshots/adpalab-283.png)
	- After importing, click on certificate > Check **Trusted Root CA** > OK

		![](../../screenshots/adpalab-284.png)
	- **Commit** after both certificates have been imported and trusted.

		![](../../screenshots/adpalab-288.png)
4. Generate certificate for decryption
	-  Click **Generate**

		![](../../screenshots/adpalab-289.png)
	- Fill out the following then **Generate**:
		- Certificate Name, Common Name, and Signed By

		![](../../screenshots/adpalab-290.png)
	- The certificate should now be pending > **Select** > **Export Certificate**

		![](../../screenshots/adpalab-291.png)
5. Issue the certificate
	- After exporting, open the certificate request in **Notepad** and copy the request.
	- In a web browser, navigate to **https:\//certs.lab.local/certsrv** and log in

		![](../../screenshots/adpalab-292.png)
	- Request a certificate > Submit a request > Select Subordinate CA and Paste Request > Submit

		![](../../screenshots/adpalab-293.png)

		![](../../screenshots/adpalab-294.png)

		![](../../screenshots/adpalab-295.png)
	- Download certificate as Base-64

		![](../../screenshots/adpalab-296.png)
6. Import Signed decryption certificate
	- Select **Import** and select the issued certificate > OK

		![](../../screenshots/adpalab-297.png)
	- The certificate should no longer be pending

		![](../../screenshots/adpalab-298.png)
	- Select decrypt certificate and check **Forward Trust Certificate** > **OK**

		![](../../screenshots/adpalab-299.png)
7. Generate certificate for **Forward Untrust**
	- Click **Generate**
	- Fill out the following then **Generate**:
		- Certificate Name, Common Name, Check CA

			![](../../screenshots/adpalab-300.png)
	- Select the certificate and check **Forward Untrust Certificate** > **OK**

		![](../../screenshots/adpalab-301.png)
8. Verify certificates

	You should now have four imported certificates.

	![](../../screenshots/adpalab-302.png)
9. **Commit**

##### Create Decryption Policies
1. Create a **Decryption Profile**

	We will be applying this profile to a **Decryption Policy**.
	- Navigate to **Objects** >**Decryption** > **Decryption Profile** > **Add**
	- Configure the following:
		Name; SSL Decryption - SSL Forward Proxy and SSL Protocol Settings; No Decryption

		![](../../screenshots/adpalab-303.png)

		![](../../screenshots/adpalab-304.png)

		![](../../screenshots/adpalab-305.png)
	- **OK**
2. Create a **URL Category** object.

	This object will be used to not decrypt specific traffic.
	- Navigate to **Objects** > **Custom Objects** > **URL Category** > **Add**
	- Configure the following:

		Name; Type: Category Match; Categories

		![](../../screenshots/adpalab-306.png)
3. Create a **Decryption Exclusion** policy.
	- Navigate to: **Policies** > **Decryption** > **Add**
	- Configure the following:
		- Name

			![](../../screenshots/adpalab-307.png)
		- Source

			![](../../screenshots/adpalab-308.png)
		- Destination

			![](../../screenshots/adpalab-309.png)
		- Service/URL Category

			![](../../screenshots/adpalab-310.png)
		- Options

			![](../../screenshots/adpalab-311.png)
	- **OK**
4. Create a **Decryption** policy.
	- Navigate to: **Policies** > **Decryption** > **Add**
	- Configure the following:
		- Name

			![](../../screenshots/adpalab-312.png)
		- Source

			![](../../screenshots/adpalab-313.png)
		- Destination

			![](../../screenshots/adpalab-314.png)
		- Service/URL Category

			![](../../screenshots/adpalab-315.png)
		- Options

			![](../../screenshots/adpalab-316.png)
	- **OK**
5. **Check policies** and **Commit**
	- Ensure that the policies are in the following order:

		![](../../screenshots/adpalab-317.png)

##### Verify Traffic Decryption
1. Verify decryption with browser certificate.
	- In your lab client, open a web browser and navigate to any website (I browsed to yahoo.com).
	- Open the site certificate

		![](../../screenshots/adpalab-318.png)

		![](../../screenshots/adpalab-319.png)

		![](../../screenshots/adpalab-320.png)

		![](../../screenshots/adpalab-321.png)

	The traffic has been **decrypted** and was issued the **Forward Trust** certificate.
2. Verify with **Traffic** logs.
	- Navigate to: **Monitor** > **Traffic**
	- Use the following filter: `( addr.src in 172.16.1.2 ) and ( port eq 443 ) and ( zone.dst eq 'External' )`

		We're searching for traffic originating from 172.16.1.2 (**PC-LAB-1**) that is going to the **External** zone through port **443**.
	- Open any log

		![](../../screenshots/adpalab-322.png)
	- Ensure the **Decrypted** is checked in the **Flags** section

		![](../../screenshots/adpalab-323.png)
3. Verify **Decryption Profile**

	Since the profile 
	- In your lab clients, open a web browser and navigate to badssl.com.
	- Test **expired** and **untrusted-root**

		![](../../screenshots/adpalab-324.png)
	- You should receive the following errors if your profile was configured correctly:
	
		![](../../screenshots/adpalab-325.png)
	
		![](../../screenshots/adpalab-326.png)

