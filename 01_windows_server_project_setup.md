
# Fundamental Windows Server 2022 Environment -  Setup Lab

<img width="826" height="156" alt="image" src="https://github.com/user-attachments/assets/c0bdc42c-1106-4ed0-b35b-4fe833aae8b8" />



# Table of Contents

# Purpose of the lab

# Tools and Technology



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

This walkthrough describes how to install the Dynamic Host Configuration Protocol (DHCP) Server role on Windows Server 2025. The DHCP Server role enables your server to automatically assign IP addresses and network configuration settings to client devices on your network.

## Prerequisites

Before you begin, verify that:

- You have administrative credentials for the server.
- The server has a static IP address configured.
- You have network connectivity.

## Install the DHCP Server role

1. Open Server Manager.
   
   - Select **Start**, and then select **Server Manager**.

2. Start the Add Roles and Features Wizard.
   
   - In Server Manager, select **Manage**, and then select **Add Roles and Features**.

3. On the **Before you begin** page, review the information, and then select **Next**.

4. On the **Select installation type** page, select **Role-based or feature-based installation**, and then select **Next**.

5. On the **Select destination server** page, select the server where you want to install DHCP, and then select **Next**.

6. On the **Select server roles** page, select the **DHCP Server** check box.

7. In the **Add Roles and Features Wizard** dialog box that appears, select **Add Features** to include the required management tools.

8. On the **Select server roles** page, select **Next**.

9. On the **Select features** page, accept the default selections, and then select **Next**.

10. On the **DHCP Server** page, review the information about the DHCP Server role, and then select **Next**.

11. On the **Confirm installation selections** page, review your selections.
    
    - Optional: Select the **Restart the destination server automatically if required** check box if you want the server to restart automatically after installation.

12. Select **Install** to begin the installation.

13. After the installation completes, on the **Installation progress** page, select **Close**.

## Complete DHCP post-installation configuration

After you install the DHCP Server role, you must complete the post-installation configuration.

1. In Server Manager, select the **Notifications** icon (flag with a warning symbol), and then select **Complete DHCP configuration**.

2. In the **DHCP Post-Install configuration wizard**, on the **Description** page, select **Next**.

3. On the **Authorization** page, choose one of the following options:
   
   - To use the current user's credentials to authorize the DHCP server in Active Directory, select **Use the following user's credentials**, verify the credentials are correct, and then select **Commit**.
   - If you need to use different credentials, select **Use alternate credentials**, enter the appropriate credentials, and then select **Commit**.

4. On the **Summary** page, verify that the configuration completed successfully, and then select **Close**.

## Verify the installation

1. In Server Manager, select **Tools**, and then select **DHCP** to open the DHCP management console.

2. In the DHCP console, expand the server name to verify that the server appears and is authorized.

3. Verify that both **IPv4** and **IPv6** nodes are present beneath the server name.

---

## Next steps

After installing the DHCP Server role, you can:

- Create and configure DHCP scopes to define IP address ranges for your network.
- Configure DHCP options such as default gateway, DNS servers, and lease duration.
- Set up DHCP failover for high availability.
- Configure DHCP policies for advanced address assignment scenarios.

<!---------------------------------------CLIENT SETUP------------------------------------------------------------------>
<!--------------------------------Create Client Virtual Machine-------------------------------------------------------->
# Part II: Setup and connect Windows Client
## Section 1: Create Windows Client VM

<!---------------------------------Setup Client network and connect to the servers domain------------------------------->

# Step 2: Setup Windows Client Network


## Configure to DHCP and DNS (IP of Windows Server - Domain controller)


## Validate from Server end that the computer is in Active Directory Users and Computers











