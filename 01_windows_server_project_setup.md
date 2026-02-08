
# Basic Windows Server 2022 Environment -  Setup Lab

<img width="826" height="156" alt="image" src="https://github.com/user-attachments/assets/c0bdc42c-1106-4ed0-b35b-4fe833aae8b8" />



# Table of Contents

# Purpose of the lab
The purpose of this lab is to build confidence in using Microsoft Windows Server 2025.  It will walk through the basics of creating a Virtual Machine in Microsoft Azure to host the server. It will also walkthrough installing
key roles and features on that server and connecting a client windows desktop machine to the server. 

# Tools and Technology
Microsoft Azure
Microsoft Windows Server 2025 Datacenter x64


<!------------------------------SERVER SETUP------------------------------------->

# Part I: Setup Server

<!----------------------------Setup Azure Virtual Machine------------------------>
## Section 1: Create Azure Virtual Machine with Server 2025 Datacenter image
### Step 1:  Sign in to the Azure Portal at https://portal.azure.com

### Step 2: Select **Create a Resource**
<img width="765" height="318" alt="image" src="https://github.com/user-attachments/assets/1cc3488f-46a8-4b20-961f-ad9e9d1261e1" />

### Step 3: Select Create under Virtual Machine
<img width="763" height="628" alt="image" src="https://github.com/user-attachments/assets/4d8853f4-6553-4646-b724-0ba051e0908b" />


### Step 4: Configure the Virtual Machine
- We are creating the Virtual Machine with only the necessary components that are important to this lab. If you are familiar with Azure you may adjust the virtual machine settings the way you would like.
  
**Step 5: Under the Basics tab**
1. Select or create a new **Resource group**.
2. Enter a **Virtual machine name**.
3. Choose the **Image**:
   - *Windows Server 2025 Datacenter: Azure Edition – x64 Gen2*
4. Create a **Username** and **Password**.
5. Select the checkbox:
   - *Would you like to use an existing Windows Server license?*


<img width="1425" height="2059" alt="image" src="https://github.com/user-attachments/assets/f3e02b2b-9069-4538-8373-7478b3f031db" />

#### Step 6: Under the Network tab
1. Select or create a new **Virtual Network** and **Subnet** if not provided.
   - IMPORTANT: Remember the Virtual network and subnet.  We will use it when we setup the client machine
  
2. Select or create a new Public IP if not provided
3. Select **Review + create**
4. Select **Create**
<img width="1229" height="1160" alt="image" src="https://github.com/user-attachments/assets/1cf97e31-b270-4dfa-95e1-d1022c35a691" />

<!-------------------------------------------------------------------------------------------->
# Section 2: Install AD DS role and promote to Domain Controller

<!-----------------Remoting into the Server via RDP ------------------------->

## Step 1: Remote into the Windows Server virtual machine via RDP

- **Windows 11**
  1. Open the Start menu and type **Remote Desktop Connection**.
  2. Select the application from the results.
  3. Enter the VM’s public IP address.
  4. Select **Connect**.
  5. Enter your username and password.
  6. Accept the certificate prompt if shown.

- **macOS**
  1. Open the App Store and install **Microsoft Remote Desktop**.
  2. Launch the app and select **Add PC**.
  3. Enter the VM’s public IP address.
  4. Add or select a user account.
  5. Double‑click the PC entry to connect.
  6. Accept the certificate prompt if shown.
 
<!---------------Configure the server with static IP and temporary DNS-------------------------->

## Step 2: Configure server with static IP address and temporary DNS
1. Open up Network Settings and navigate to Iv4 Setting
2. Create a manual IPv4 address and default gateway for the server
   - To acquire the servers IP address:
   - Open commandline and type IP config. Place that into the box IP address. Or navigate to the server resource homepage in Microsoft Azure
4. Create a temporary DNS 8.8.8.8
<img width="1581" height="788" alt="image" src="https://github.com/user-attachments/assets/1384b4cb-56a5-4b70-a168-b6ad7a77b520" />


<!--------------------------------Install AD DS server role-------------------------------------------->

## Step 3: Install AD DS role
1. 	Open Server Manager
2. 	Select Manage → Add Roles and Features.
<img width="428" height="228" alt="image" src="https://github.com/user-attachments/assets/c48b30e5-69f7-4353-89ac-376f5c062832" />


3. 	Choose Role-based or feature-based installation.
4. 	Select the local server from the server pool.


5. 	In the Server Roles list, check Active Directory Domain Services.
6. 	Accept any required features when prompted.

<img width="1031" height="603" alt="image" src="https://github.com/user-attachments/assets/c4d740ee-c4b1-4f01-b736-6ce48235113f" />

7. 	Continue through the wizard and select Install.
8. 	Wait for the installation to complete (no reboot required yet).
This installs the binaries but does not configure the domain controller
<!-------------------------Promote AD DS role to Domain controller---------------------------------->

## Step 4: Promote to Domain Controller

<img width="590" height="458" alt="image" src="https://github.com/user-attachments/assets/2c43d52e-34c0-4cea-b2b9-f6952e1725b7" />


<img width="761" height="560" alt="image" src="https://github.com/user-attachments/assets/06d6fe8f-1c03-4195-b912-cca754c1781d" />

<img width="442" height="70" alt="image" src="https://github.com/user-attachments/assets/18f8c4c2-a02d-4453-ab8a-6155e597b722" />
<img width="762" height="562" alt="image" src="https://github.com/user-attachments/assets/dc318749-c28c-4950-9390-8f3584cf6a16" />


When the promotion is complete the server will reset
You will see three new active services 
1. AD DS, DNS and File and Storage Services
<img width="1527" height="746" alt="image" src="https://github.com/user-attachments/assets/50229aea-54bb-414d-926c-e00acc9c4ed5" />


## Step 5:  Set Server's DNS
After the promotion of the domain controller you will navigate back to Ipv4 settings and change the DNS to the IP address of the server. This will allow your domain controller to function with DNS

After the reset you will see the server set to 127.0.0.1.  We need to change it to the IP address of the server:
<img width="373" height="116" alt="image" src="https://github.com/user-attachments/assets/3a6cd43f-8e08-48e6-8a44-a04f9a5ac08b" />


<img width="379" height="200" alt="image" src="https://github.com/user-attachments/assets/f215400e-f5af-4e90-9d68-7360c4bf7aec" />



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
# Part II: Setup and connect Windows Client
## Section 1: Create Windows Client VM
Create the Virtual Machine in the same way that we have created the same manner as before [reference above]
- except we will use the windows 11 base image
<img width="749" height="36" alt="image" src="https://github.com/user-attachments/assets/d41e4966-ee91-47ed-b2f7-0843d90edbd3" />

- For network, remember select the same vnet as the server

<img width="776" height="230" alt="image" src="https://github.com/user-attachments/assets/b51370ca-6a4e-43d4-9c14-4ff8d82aae4b" />


- Take note of network information on the Azure home screen for the client
<img width="379" height="194" alt="image" src="https://github.com/user-attachments/assets/c0265fbc-b7ff-4e46-81da-7e311fc66cce" />





<!---------------------------------Setup Client network and connect to the servers domain------------------------------->

# Step 2: Setup Windows Client Network
## Log into the client machine through the Windows Server 2025 machine
- Use the clients private IP address which can be found through on the homepage of the virtual machine
- Log into to the client machine via RDP through the server since the client does not have a public IP address

<img width="452" height="554" alt="image" src="https://github.com/user-attachments/assets/8195d005-8009-44b5-8349-689edec9229c" />


## Configure to DNS and DHCP(IP of Windows Server - Domain controller)
Configure client DNS
-Go into network settings either via the control panel or Settings and Netwowrk
-Select Use the following DNS server addresses
-Type the name of the Preferred DNS server as the IP of the domain on Server 2025 VM
<img width="1026" height="618" alt="image" src="https://github.com/user-attachments/assets/f4bf1159-59a2-45ca-a5ba-ccbce7015038" />

## Connect to Windows Server DHCP
Note: Azure will by default not allow a connection to a self created server. This be possible by setting up a DHCP rely via Azure.
This may not be possible depending on your region. 

## Connect to Servers domain controller
Step 1: Select the **Windows icon**
Step 2: Select **Accounts** on the left hand side
Step 3: Select **Access work or school**
Step 4: Select the **Connect** button beside **Add a work or schoole account**
<img width="699" height="178" alt="image" src="https://github.com/user-attachments/assets/ab26d012-7149-4ef6-9731-3203d5c4f015" />

Step 5: Select the option **Join this device to a local Active Directory domain**

<img width="654" height="634" alt="image" src="https://github.com/user-attachments/assets/c70a2382-cf4b-48a4-8b45-ad0f79e32074" />

Step 6: Type in the Domain name you want to connect to

Step 7: Type the username and password of the Server

Step 8: Create an account name under **User Account** and select Standard User under **Account Type**

## Validate from Server end that the computer is in Active Directory Users and Computers
<img width="789" height="900" alt="image" src="https://github.com/user-attachments/assets/bdf8bfea-8644-499f-9a62-14e04730214c" />















