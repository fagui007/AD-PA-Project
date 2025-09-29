### 5. Deploy and Configure DNS and DHCP Server
##### Initial Configuration
1. Configure **static IP** and **DNS server** in Servers (172.16.2.0/24) subnet.

	*Note: Reference IP Address Plan - DNS-DHCP-LAB*
	- IP Address: 172.16.2.3
	- Subnet Mask: 255.255.255.0
	- Default Gateway: 172.16.2.1
	- DNS Server: 172.16.2.2
2. Ensure that you can reach the **Servers gateway** on the **Firewall**.

	*Note: We did not need to configure a security rule on the Palo Alto Firewall for this because intrazone (within the same network) sessions are allowed by default.*

	![](../../screenshots/adpalab-33.png)

	We can also try to ping the **Domain Controller**

	![](../../screenshots/adpalab-354.png)
3. Change **computer name** and **join LAB.LOCAL domain**, use **labadmin** credentials when prompted, then **Restart**.

	![](../../screenshots/adpalab-357.png)

	![](../../screenshots/adpalab-61.png)

	![](../../screenshots/adpalab-399.png)
4. **Log in** with either **Lab Admin** user account.

	*Note: Notice how we are now signing in through the domain.*

	![](../../screenshots/adpalab-11.png)

##### DNS and DHCP Installation
1.  Navigate to **Server Manager** > **Manage** > **Add Roles and Features** > Complete the Wizard
2. Select **Role-base or feature-based installation**

	![](../../screenshots/adpalab-84.png)
3. Select **DNS-DHCP-LAB** to install roles and features.

	![](../../screenshots/adpalab-381.png)
4. Select Server Roles > **DHCP Server** and **DNS Server** > Check **Include management tools**.

	![](../../screenshots/adpalab-05.png)

	![](../../screenshots/adpalab-03.png)

	![](../../screenshots/adpalab-04.png)
5. Leave **Features** as default > Next
6. DHCP Server > Next
7. DNS Server > Next
8. Install DNS and DHCP features.

	![](../../screenshots/adpalab-401.png)

##### DHCP Configuration
1. Once feature installation is complete, select **Complete DHCP Configuration**			!

	![](../../screenshots/adpalab-28.png)
2. Continue with configuration.

	![](../../screenshots/adpalab-39.png)
3. **Authorize** DHCP server with an administrator account > Commit

	![](../../screenshots/adpalab-38.png)
4. Your DHCP server is now configured!

	![](../../screenshots/adpalab-40.png)
5. Create **DHCP Scope** for **Client IPs** > Open **DHCP** app

	![](../../screenshots/adpalab-37.png)
6. Expand path > Right-Click **IPv4** > select **New Scope**

	![](../../screenshots/adpalab-41.png)
7. Start scope setup

	![](../../screenshots/adpalab-49.png)
8. Give your scope a name.

	![](../../screenshots/adpalab-47.png)
9. Configure the address range and subnet mask

	*Note: Remember to exclude the network address, broadcast address, and the default gateway.*

	![](../../screenshots/adpalab-48.png)
10. We're not configuring **Exclusions/Delay** > Next
11.  Leave **Duration** as default > Next
12. Configure Scope Options

	![](../../screenshots/adpalab-43.png)
13. Add default gateway for **Client network** (This should be the IP of the interface on the firewall).

	![](../../screenshots/adpalab-46.png)
14. Leave **Domain Name** and **DNS Servers** as default > Next

	![](../../screenshots/adpalab-44.png)
15. We're not configuring a **WINS server** > Next
16. **Activate** the Client scope > Finish

	![](../../screenshots/adpalab-42.png)
17. You should now have an active scope for **Lab Clients**

	![](../../screenshots/adpalab-45.png)

	*Note: We will need to setup a **DHCP Relay** on the **firewall** so the **Client network** can send and receive DHCP requests and replies. This is necessary because the clients that need IPs do not reside within the same network as the DHCP server.*

##### DNS Configuration
1. Configure **Forward Lookup Zones** and **DNS Forwarder** > Open **DNS** app

	*Note: We need a forward lookup zone so domain names can be translated into IP addresses.*
2. Expand path to **Forward Lookup Zones** > Right-click **Forward Lookup Zones**> Select **New Zone**

	![](../../screenshots/adpalab-56.png)
3. Continue with setup > Select **Primary Zone**

	![](../../screenshots/adpalab-59.png)
4. Name your new zone > Next

	![](../../screenshots/adpalab-58.png)
5. Create new file for zone > Next

	![](../../screenshots/adpalab-57.png)
6. **Do not allow dynamic updates** > Next

	![](../../screenshots/adpalab-60.png)
7. Complete setup
8. Click on your DNS Server > Right-click **Forwarders** > **Properties**

	![](../../screenshots/adpalab-53.png)
9. Set DNS servers for to resolve DNS queries (if left blank, root hints will be used).

	![](../../screenshots/adpalab-52.png)

	*Note: The Server FQDN is not resolving yet because we need to create a security policy on the firewall to allow outbound traffic from our servers.*
10. **OK**
