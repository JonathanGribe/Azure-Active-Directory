# Basic Windows Server 2022 Environment -  Setup Lab

<img width="826" height="156" alt="image" src="https://github.com/user-attachments/assets/c0bdc42c-1106-4ed0-b35b-4fe833aae8b8" />


# Table of Contents
1. [Purpose of the Lab](#purpose-of-the-lab)
2. [Tools and Technology](#tools-and-technology)
3. [Part I: Setup Server](#part-i-setup-server)
   - [Section 1: Create Azure Virtual Machine](#section-1-create-azure-virtual-machine-with-server-2025-datacenter-image)
   - [Section 2: Install AD DS Role](#section-2-install-ad-ds-role-and-promote-to-domain-controller)
   - [Section 3: Install DHCP Role](#section-3-install-dhcp-role)
4. [Part II: Setup Windows Client](#part-ii-setup-and-connect-windows-client)

# Purpose of the lab
The purpose of this lab is to build confidence in using Microsoft Windows Server 2025.  It will walk through the basics of creating a Virtual Machine in Microsoft Azure to host the server. It will also walkthrough installing
key roles and features on that server and connecting a client windows desktop machine to the server. 

# Tools and Technology
Microsoft Azure
Microsoft Windows Server 2025 Datacenter: Azure Edition - x64 Gen2


<!------------------------------SERVER SETUP------------------------------------->
<!----------------------------Setup Azure Virtual Machine------------------------>
# Part I: Set up the server

## Create an Azure virtual machine

This section guides you through creating a Windows Server 2025 virtual machine in Azure.

### Sign in to Azure Portal

1. Navigate to https://portal.azure.com and sign in with your Azure credentials.

### Create the virtual machine resource

1. Select **Create a resource**.

   ![Create a resource button in Azure Portal](https://github.com/user-attachments/assets/1cc3488f-46a8-4b20-961f-ad9e9d1261e1)

2. Under **Virtual Machine**, select **Create**.

   ![Virtual Machine create option](https://github.com/user-attachments/assets/4d8853f4-6553-4646-b724-0ba051e0908b)

### Configure basic settings

> [!NOTE]
> This lab uses only the essential configuration options. If you're familiar with Azure, you can adjust settings as needed.

1. On the **Basics** tab, configure the following settings:
   - **Resource group**: Select an existing resource group or create a new one
   - **Virtual machine name**: Enter a name for your VM
   - **Image**: Select **Windows Server 2025 Datacenter: Azure Edition – x64 Gen2**
   - **Username**: Create an administrator username
   - **Password**: Create a strong password
   - **Licensing**: Select the checkbox for **Would you like to use an existing Windows Server license?**

   ![Basics tab configuration](https://github.com/user-attachments/assets/f3e02b2b-9069-4538-8373-7478b3f031db)

### Configure network settings

1. Select the **Networking** tab.

2. Configure the following settings:
   - **Virtual network**: Select an existing virtual network or create a new one
   - **Subnet**: Select an existing subnet or create a new one
   - **Public IP**: Select an existing public IP or create a new one

   > [!IMPORTANT]
   > Note your virtual network and subnet names. You'll use the same network when creating the client machine later.

3. Select **Review + create**.

4. After validation passes, select **Create**.

   ![Network configuration](https://github.com/user-attachments/assets/1cf97e31-b270-4dfa-95e1-d1022c35a691)

The deployment begins. Wait for the deployment to complete before proceeding to the next section.

<!--------------------------Setup server------------------------------------------------------------------>
# Part I: Set up the server (continued)

## Install Active Directory Domain Services

This section guides you through installing AD DS, configuring the server network settings, and promoting the server to a domain controller.

### Connect to the server via Remote Desktop

Use Remote Desktop Protocol (RDP) to connect to your Windows Server virtual machine.

**On Windows:**

1. Open the **Start** menu and search for **Remote Desktop Connection**.
2. Select the application from the results.
3. In the **Computer** field, enter the VM's public IP address (found in the Azure Portal on the VM overview page).
4. Select **Connect**.
5. Enter your username and password.
6. If prompted with a certificate warning, select **Yes** to continue.

**On macOS:**

1. Open the **App Store** and install **Microsoft Remote Desktop**.
2. Launch the app and select **Add PC**.
3. In the **PC name** field, enter the VM's public IP address.
4. Add your user account credentials or select an existing account.
5. Double-click the PC entry to connect.
6. If prompted with a certificate warning, select **Continue**.

### Configure static IP address and DNS

Before installing AD DS, configure the server with a static IP address and temporary DNS settings.

1. Open **Settings** and navigate to **Network & Internet**.
2. Select your network adapter, then select **Edit** under **IP assignment**.
3. Select **Manual** and enable **IPv4**.
4. Configure the following settings:
   - **IP address**: Enter the current IP address assigned to the server
     > [!TIP]
     > To find the current IP address, open Command Prompt and run `ipconfig`, or view it in the Azure Portal on the VM's **Networking** page.
   - **Subnet mask**: Enter `255.255.255.0` (or match your Azure subnet configuration)
   - **Gateway**: Enter your subnet's default gateway (typically the first address in your subnet range)
   - **Preferred DNS**: Enter `8.8.8.8`
     > [!NOTE]
     > This is a temporary DNS setting. You'll change this to the server's own IP address after promoting it to a domain controller.

5. Select **Save**.

   ![Network configuration with static IP and DNS](https://github.com/user-attachments/assets/1384b4cb-56a5-4b70-a168-b6ad7a77b520)

### Install the AD DS role

1. Open **Server Manager**.
2. Select **Manage** > **Add Roles and Features**.

   ![Add Roles and Features menu](https://github.com/user-attachments/assets/c48b30e5-69f7-4353-89ac-376f5c062832)

3. On the **Before You Begin** page, select **Next**.
4. On the **Installation Type** page, select **Role-based or feature-based installation**, then select **Next**.
5. On the **Server Selection** page, select the local server from the server pool, then select **Next**.
6. On the **Server Roles** page, select the **Active Directory Domain Services** checkbox.

   ![Select Active Directory Domain Services role](https://github.com/user-attachments/assets/c4d740ee-c4b1-4f01-b736-6ce48235113f)

7. In the **Add Roles and Features Wizard** dialog, select **Add Features** to include required management tools.
8. Select **Next** through the remaining wizard pages, then select **Install**.
9. Wait for the installation to complete.

> [!NOTE]
> This installs the AD DS binaries but does not configure the domain controller. No restart is required at this stage.

### Promote the server to a domain controller

After installing AD DS, promote the server to a domain controller to create your Active Directory forest.

1. In **Server Manager**, select the **notification flag** (with a warning icon) in the top-right corner.

   ![Notification flag with post-deployment configuration](https://github.com/user-attachments/assets/2c43d52e-34c0-4cea-b2b9-f6952e1725b7)

2. Select **Promote this server to a domain controller**.
3. On the **Deployment Configuration** page:
   - Select **Add a new forest**
   - In the **Root domain name** field, enter your domain name (e.g., `contoso.local`)
   - Select **Next**

   ![Deployment configuration for new forest](https://github.com/user-attachments/assets/06d6fe8f-1c03-4195-b912-cca754c1781d)

4. On the **Domain Controller Options** page:
   - Set both **Forest functional level** and **Domain functional level** to **Windows Server 2016** or higher
   - Ensure **Domain Name System (DNS) server** is selected
   - Enter and confirm a **Directory Services Restore Mode (DSRM) password**
   - Select **Next**

   ![Domain Controller Options configuration](https://github.com/user-attachments/assets/18f8c4c2-a02d-4453-ab8a-6155e597b722)

5. On the **DNS Options** page, select **Next** (ignore any delegation warnings).
6. On the **Additional Options** page, verify the NetBIOS domain name, then select **Next**.
7. On the **Paths** page, accept the default locations for the database, log files, and SYSVOL folder, then select **Next**.
8. On the **Review Options** page, review your selections, then select **Next**.
9. On the **Prerequisites Check** page, wait for validation to complete, then select **Install**.

   ![Prerequisites check completion](https://github.com/user-attachments/assets/dc318749-c28c-4950-9390-8f3584cf6a16)

The server automatically restarts after the promotion completes. Wait for the restart to finish before reconnecting.

### Verify the installation

After the server restarts and you reconnect via RDP:

1. Open **Server Manager**.
2. Verify that the **Dashboard** shows three new services running:
   - **AD DS** (Active Directory Domain Services)
   - **DNS** (DNS Server)
   - **File and Storage Services**

   ![Server Manager showing active services](https://github.com/user-attachments/assets/50229aea-54bb-414d-926c-e00acc9c4ed5)

### Update DNS settings

After promoting the server to a domain controller, update the DNS settings to point to the server itself.

1. Open **Settings** and navigate to **Network & Internet**.
2. Select your network adapter, then select **Edit** under **DNS server assignment**.
3. Change the **Preferred DNS** from `127.0.0.1` to the server's actual IP address.

   > [!IMPORTANT]
   > Using the server's IP address instead of the loopback address (127.0.0.1) ensures proper DNS resolution for domain-joined clients.

   ![DNS set to loopback address](https://github.com/user-attachments/assets/3a6cd43f-8e08-48e6-8a44-a04f9a5ac08b)

4. Select **Save**.

   ![DNS updated to server IP address](https://github.com/user-attachments/assets/f215400e-f5af-4e90-9d68-7360c4bf7aec)

### Verification checklist

Confirm the following before proceeding:
- [ ] Server Manager shows AD DS, DNS, and File and Storage Services as running
- [ ] DNS is configured with the server's IP address (not 127.0.0.1)
- [ ] You can successfully ping your domain name: `ping yourdomain.local`



<!------------------------------------Install DHCP Role------------------------------------------------------>
# Section 3: Install DHCP Role
# Install the DHCP Server role on Windows Server 2025

1. Open Server Manager.
   
   - Select **Start**, and then select **Server Manager**.

2. Start the Add Roles and Features Wizard.
   
   - In Server Manager, select **Manage**, and then select **Add Roles and Features**.

3. On the **Before you begin** page, review the information, and then select **Next**.

4. On the **Select installation type** page, select **Role-based or feature-based installation**, and then select **Next**.

5. On the **Select destination server** page, select the server where you want to install DHCP, and then select **Next**.

6. On the **Select server roles** page, select the **DHCP Server** check box.

7. In the **Add Roles and Features Wizard** dialog box that appears, select **Add Features** to include the required management tools.

<img width="998" height="533" alt="image" src="https://github.com/user-attachments/assets/ee734959-dc07-48de-ae6e-fa3abd5f3f89" />


9. On the **Select server roles** page, select **Next**.

10. On the **Select features** page, accept the default selections, and then select **Next**.

11. On the **DHCP Server** page, review the information about the DHCP Server role, and then select **Next**.

12. On the **Confirm installation selections** page, review your selections.
    
    - Optional: Select the **Restart the destination server automatically if required** check box if you want the server to restart automatically after installation.

13. Select **Install** to begin the installation.
    <img width="785" height="559" alt="image" src="https://github.com/user-attachments/assets/58d5ee99-14e4-4368-a3b5-bb30fd575207" />


15. After the installation completes, on the **Installation progress** page, select **Close**.

## Complete DHCP post-installation configuration

After you install the DHCP Server role, you must complete the post-installation configuration.

1. In Server Manager, select the **Notifications** icon (flag with a warning symbol), and then select **Complete DHCP configuration**.
<img width="785" height="559" alt="image" src="https://github.com/user-attachments/assets/b9ef01e3-f872-471f-9ad9-df843167e8ac" />

2. In the **DHCP Post-Install configuration wizard**, on the **Description** page, select **Next**.


3. On the **Authorization** page, choose one of the following options:
   
   - To use the current user's credentials to authorize the DHCP server in Active Directory, select **Use the following user's credentials**, verify the credentials are correct, and then select **Commit**.
   - If you need to use different credentials, select **Use alternate credentials**, enter the appropriate credentials, and then select **Commit**.

4. On the **Summary** page, verify that the configuration completed successfully, and then select **Close**.

5. On the **Dashboard** page, you will see the 
<img width="863" height="900" alt="image" src="https://github.com/user-attachments/assets/08b53bc7-4358-43e9-8048-d8a1361cfbe0" />


## Verify the installation

1. In Server Manager, select **Tools**, and then select **DHCP** to open the DHCP management console.

2. In the DHCP console, expand the server name to verify that the server appears and is authorized.

4. Verify that both **IPv4** and **IPv6** nodes are present beneath the server name.

<img width="757" height="900" alt="image" src="https://github.com/user-attachments/assets/24bf5244-2dcd-44d8-aa60-867725b6aad6" />


---
<!-------------------------------------------------Configure DHCP scope---------------------------------------------------------->
## Next steps
# Configure DHCP Scope

After you install the DHCP Server role, you need to configure scopes, options, and additional features to enable automatic IP address assignment on your network.

## Create and configure a DHCP scope

A DHCP scope defines the range of IP addresses that the DHCP server can assign to client devices on a specific subnet.

### Create a new IPv4 scope

1. Open the DHCP management console.
   
   - In Server Manager, select **Tools**, and then select **DHCP**.

2. In the DHCP console, expand the server name, right-click **IPv4**, and then select **New Scope**.

3. In the **New Scope Wizard**, on the **Welcome** page, select **Next**.

4. On the **Scope Name** page, configure the following settings:
   
   - In the **Name** box, enter a descriptive name for the scope (for example, "Building A Network" or simply "IPv4 Scope").
   - Optional: In the **Description** box, enter additional details about the scope.
   - Select **Next**.

<img width="517" height="426" alt="image" src="https://github.com/user-attachments/assets/10aafb13-fcf7-44f9-8b39-a1eed9533caa" />


5. On the **IP Address Range** page, configure the address range:
   
   - In the **Start IP address** box, enter the first IP address in the range (for example, 192.168.1.10).
   - In the **End IP address** box, enter the last IP address in the range (for example, 192.168.1.200).
   - In the **Length** box, enter the subnet mask length, or use the **Subnet mask** box to enter the mask directly (for example, 255.255.255.0).
   - Select **Next**.
   <img width="517" height="426" alt="image" src="https://github.com/user-attachments/assets/753c71f3-35d6-4732-bd23-3d3ff7754340" />


6. On the **Add Exclusions and Delay** page, add any IP addresses that should not be assigned:
   
   - Optional: In the **Start IP address** and **End IP address** boxes, enter ranges to exclude (for example, 192.168.1.1 to 192.168.1.9 for network equipment).
  
   - Select **Add** to add the exclusion range.
   
   - Select **Next**.
    
    <img width="517" height="426" alt="image" src="https://github.com/user-attachments/assets/eef3c0e1-1427-4297-9d5d-da5244735ebf" />
    


7. On the **Lease Duration** page, configure how long clients can use an IP address:
   
   - Accept the default lease duration (8 days), or enter custom values for days, hours, and minutes.
   - Select **Next**.

8. On the **Configure DHCP Options** page, select **Yes, I want to configure these options now**, and then select **Next**.

9. On the **Router (Default Gateway)** page, add the default gateway:
   
   - In the **IP address** box, enter the gateway address (In this case, 172.17.0.1).
   - Select **Add**.
   - Select **Next**.
<img width="517" height="426" alt="image" src="https://github.com/user-attachments/assets/abce3d09-dc50-41f5-817d-cc1fc84f1456" />


10. On the **Domain Name and DNS Servers** page, configure DNS settings:
    
    - Optional: In the **Parent domain** box, enter your domain name.
    - In the **IP address** box, enter the DNS server address.
    - Select **Add**.
    - Repeat for additional DNS servers if needed.
    - Select **Next**.


11. On the **WINS Servers** page, select **Next** (unless you need to configure WINS servers for legacy applications).

12. On the **Activate Scope** page, select **Yes, I want to activate this scope now**, and then select **Next**.

13. On the **Completing the New Scope Wizard** page, review your settings, and then select **Finish**.

### Verify the scope configuration

1. In the DHCP console, expand **IPv4**, and then expand the scope you created.

2. Verify that the following items appear beneath the scope:
   
   - **Address Pool** - Shows the IP address range
   - **Address Leases** - Shows active leases (initially empty)
   - **Reservations** - For configuring static IP assignments
   - **Scope Options** - Shows configured options like gateway and DNS
<img width="591" height="332" alt="image" src="https://github.com/user-attachments/assets/151e2f11-0c65-40e1-a4f6-978c99fae3b1" />



<!---------------------------------------CLIENT SETUP------------------------------------------------------------------>
<!--------------------------------Create Client Virtual Machine-------------------------------------------------------->
## Section 1: Create a Windows client virtual machine

Create a Windows 11 virtual machine using the same process described in [Section 1: Create an Azure virtual machine](#section-1-create-an-azure-virtual-machine), with the following differences:

### Image selection

- **Image**: Select **Windows 11 Pro** (or your preferred Windows 11 edition)

  ![Windows 11 image selection](https://github.com/user-attachments/assets/d41e4966-ee91-47ed-b2f7-0843d90edbd3)

### Network configuration

> [!IMPORTANT]
> The client VM must be on the same virtual network as the server to communicate with the domain controller.

1. On the **Networking** tab, configure the following:
   - **Virtual network**: Select the same virtual network you created for the server
   - **Subnet**: Select the same subnet as the server
   - **Public IP**: Select **None** (not required for this lab)

   ![Network settings showing same VNet as server](https://github.com/user-attachments/assets/b51370ca-6a4e-43d4-9c14-4ff8d82aae4b)

2. Complete the remaining configuration and select **Create**.

### Record network information

After deployment completes:

1. Navigate to the VM's **Overview** page in the Azure Portal.
2. Note the **Private IP address**—you'll need this to connect from the server.

   ![VM overview showing private IP address](https://github.com/user-attachments/assets/c0265fbc-b7ff-4e46-81da-7e311fc66cce)





<!---------------------------------Setup Client network and connect to the servers domain------------------------------->


## Section 2: Configure the client and join the domain

This section guides you through connecting to the client VM, configuring DNS settings, and joining the client to the Active Directory domain.

### Connect to the client via Remote Desktop

Since the client VM doesn't have a public IP address, connect to it from the Windows Server using Remote Desktop.

1. On the **Windows Server**, open **Remote Desktop Connection**.
2. In the **Computer** field, enter the client's **private IP address** (noted in the previous section).
3. Select **Connect**.
4. Enter the client's username and password.
5. If prompted with a certificate warning, select **Yes** to continue.

   ![Remote Desktop Connection to client VM](https://github.com/user-attachments/assets/8195d005-8009-44b5-8349-689edec9229c)

### Configure DNS settings

Configure the client to use the domain controller as its DNS server.

1. On the **Windows 11 client**, open **Settings** and navigate to **Network & Internet**.
2. Select your network adapter, then select **Edit** under **DNS server assignment**.
3. Select **Manual** and enable **IPv4**.
4. In the **Preferred DNS** field, enter the Windows Server's IP address (the domain controller).
5. Select **Save**.

   ![DNS configuration pointing to domain controller](https://github.com/user-attachments/assets/f4bf1159-59a2-45ca-a5ba-ccbce7015038)

> [!NOTE]
> This client will use a static IP address configuration since Azure doesn't support DHCP relay to custom DHCP servers in all regions. If you need DHCP functionality, you must configure an Azure DHCP relay agent, which may not be available in your region.

### Join the client to the domain

1. On the **Windows 11 client**, open **Settings**.
2. Select **Accounts** in the left navigation pane.
3. Select **Access work or school**.
4. Next to **Add a work or school account**, select **Connect**.

   ![Access work or school settings](https://github.com/user-attachments/assets/ab26d012-7149-4ef6-9731-3203d5c4f015)

5. Select **Join this device to a local Active Directory domain**.

   ![Join local Active Directory domain option](https://github.com/user-attachments/assets/c70a2382-cf4b-48a4-8b45-ad0f79e32074)

6. In the **Domain name** field, enter your domain name (e.g., `contoso.local`), then select **Next**.
7. Enter the **username** and **password** of a domain administrator account (the credentials you created for the Windows Server).
8. On the **Add an account** page:
   - Under **User Account**, enter a name for the local user account
   - Under **Account Type**, select **Standard User**
   - Select **Next**
9. Select **Restart now** to complete the domain join process.

### Verify domain join

After the client restarts, verify it successfully joined the domain.

**On the Windows Server:**

1. Open **Server Manager**.
2. Select **Tools** > **Active Directory Users and Computers**.
3. Expand your domain name in the left pane.
4. Select the **Computers** container.
5. Verify that the client computer appears in the list.

   ![Client computer listed in Active Directory Users and Computers](https://github.com/user-attachments/assets/bdf8bfea-8644-499f-9a62-14e04730214c)

**On the Windows 11 client:**

1. Open **Settings** > **Accounts** > **Access work or school**.
2. Verify that your domain appears as **Connected to [domain name] domain**.

### Verification checklist

Confirm the following before proceeding:
- [ ] Client DNS is configured with the domain controller's IP address
- [ ] Client successfully joined the domain
- [ ] Client computer appears in Active Directory Users and Computers on the server
- [ ] You can sign in to the client with a domain account

























