### 3. Configure Policies on NGFW (via GUI)
1. **Create a NAT policy**
	- Navigate to: **Policies** > **NAT** > **Add**

		*Note: Without a NAT policy, all clients **cannot** reach the internet.*
	- Name

		![](../../screenshots/adpalab-94.png)
	- Add **source** and **destination**

		![](../../screenshots/adpalab-95.png)
	- Add **source address translation**

		![](../../screenshots/adpalab-96.png)
	- **OK**
2. **Create a security policy** for the **Management** zone
	The security policy that we will be creating is to allow the **Management** interface to reach Palo Alto Networks servers.
	- Navigate to: **Policies** > **Security** > **Add**
	- Name

		![](../../screenshots/adpalab-377.png)
	- Select **Source Zone** (*Optional*: Set source address to Management IP space)

		![](../../screenshots/adpalab-379.png)
	- Select **Destination Zone**

		![](../../screenshots/adpalab-376.png)
	- Leave **Applications** as **Any**

		![](../../screenshots/adpalab-375.png)
	- Leave **Service** and **URL Category** as **default**.

		![](../../screenshots/adpalab-378.png)
	- Set **Action** and **Profile** settings

		![](../../screenshots/adpalab-374.png)
	- **OK** and **Commit**
3. **Validate Policies**
	- If the firewall was able to fetch a **device certificate**, then the configured policies worked as intended.
		- To verify, go to **Tasks** on bottom right and look for the following task:

			![](../../screenshots/adpalab-67.png)
	- You can also attempt to retrieve licenses from license server:
		- Navigate to: **Device** > **Licenses** > **Retrieve License Keys from License Server** (No Licenses should be present prior to this)
		- After license retrieval, all your subscriptions should be present:

			![](../../screenshots/adpalab-90.png)
4. *Optional, but **Recommended***: **Configure Dynamic Updates**
	Now that we have our policies in place, the firewall should have no issues pulling down updates, but we need to tell the firewall to download and install updates.
	- Navigate to: **Device** > **Dynamic Updates**
		- Set a **schedule** for **Antivirus** updates.

			![](../../screenshots/adpalab-20.png)
		- Set a **schedule** for **Applications and Threats** updates.

			![](../../screenshots/adpalab-21.png)

			*Note: It's recommended to test new apps before pushing them to a live environment, but this is a lab so we're going to download and install.*
		- Set a schedule for **WildFire** updates.

			![](../../screenshots/adpalab-400.png)

**Configured Schedule**

![](../../screenshots/adpalab-387.png)
