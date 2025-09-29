### 0. Pre-lab Setup
##### Host PC Ethernet Configuration
1. *Optional, but recommended:* **Disable IPv4 and IPv6** on Network Adapters that will be used for Bridge Adapters.
	- On Host PC, navigate to: **Control Panel** > **Network and Internet** > **Network Connections** > Right-Click and Select **Properties** for adapter that will be a **Bridged Adapter**.
	
		![](../../screenshots/adpalab-50.png)
2. **Set Static IP for Ethernet Adapter** that will connect to eth1/8 on NGFW.
	- On Host PC, navigate to: **Control Panel** > **Network and Internet** > **Network Connections** > Right-Click and Select **Properties** for adapter that will be used for **Firewall Configuration**.
	- Select **IPv4** > **Properties** > Use the following:
		- IP: 10.1.1.2
		- Subnet Mask: 255.255.255.0
		- Default Gateway: 10.1.1.1
		- DNS can be left blank.
		
		![](../../screenshots/adpalab-64.png)
##### Workstation Configuration
1. **Create Custom Network Adaptor** for Bridged Adapters on VMWare Workstation
	- Select VMWare Workstation, navigate to: **Edit** > **Virtual Network Editor** > **Change Settings** > **Add Network** > Select any virtual network > **Bridged** > Choose Adapter that was previously configured
	
		![](../../screenshots/adpalab-398.png)
	- Ensure that you ceate two virtual networks for **Client VMs** and **Server VMs**.
	
		![](../../screenshots/adpalab-97.png)
2. **Create VMs** and install Windows Server 2025/Windows 11 Enterprise for all servers and clients.
	- File > New Virtual Machine *or* Crtl+N
	
		![](../../screenshots/adpalab-397.png)
	- Select "Install OS Later"
	
		![](../../screenshots/adpalab-110.png)

   - Choose the OS Version: Windows 11 or Server 2025
	
		![](../../screenshots/adpalab-111.png)

   - Name your VM and select storage location.

	   	![](../../screenshots/adpalab-93.png)
	- Select disk capacity (I left as default).

	   	![](../../screenshots/adpalab-383.png)
	- Click on **Customize Hardware** > **New CD/DVD** > **Use ISO Image File** > Select OS ISO

		![](../../screenshots/adpalab-88.png)
	- Change **Network Adapter** then **Close** and **Finish**

		![](../../screenshots/adpalab-99.png)
	
	**End Result**

	![](../../screenshots/adpalab-30.png)
	- Ensure that network adapters are assigned to correct VMs:
		- **Clients** → VMnet1
		- **Servers** → VMnet2
