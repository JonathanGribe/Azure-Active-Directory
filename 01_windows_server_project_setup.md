
# Basic Windows Server 2022 Environment -  Setup Lab

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

## Next steps
# Configure DHCP Server on Windows Server 2025

After you install the DHCP Server role, you need to configure scopes, options, and additional features to enable automatic IP address assignment on your network.

## Create and configure a DHCP scope

A DHCP scope defines the range of IP addresses that the DHCP server can assign to client devices on a specific subnet.

### Create a new IPv4 scope

1. Open the DHCP management console.
   
   - In Server Manager, select **Tools**, and then select **DHCP**.

2. In the DHCP console, expand the server name, right-click **IPv4**, and then select **New Scope**.

3. In the **New Scope Wizard**, on the **Welcome** page, select **Next**.

4. On the **Scope Name** page, configure the following settings:
   
   - In the **Name** box, enter a descriptive name for the scope (for example, "Building A Network").
   - Optional: In the **Description** box, enter additional details about the scope.
   - Select **Next**.

5. On the **IP Address Range** page, configure the address range:
   
   - In the **Start IP address** box, enter the first IP address in the range (for example, 192.168.1.10).
   - In the **End IP address** box, enter the last IP address in the range (for example, 192.168.1.200).
   - In the **Length** box, enter the subnet mask length, or use the **Subnet mask** box to enter the mask directly (for example, 255.255.255.0).
   - Select **Next**.

6. On the **Add Exclusions and Delay** page, add any IP addresses that should not be assigned:
   
   - Optional: In the **Start IP address** and **End IP address** boxes, enter ranges to exclude (for example, 192.168.1.1 to 192.168.1.9 for network equipment).
   - Select **Add** to add the exclusion range.
   - Select **Next**.

7. On the **Lease Duration** page, configure how long clients can use an IP address:
   
   - Accept the default lease duration (8 days), or enter custom values for days, hours, and minutes.
   - Select **Next**.

8. On the **Configure DHCP Options** page, select **Yes, I want to configure these options now**, and then select **Next**.

9. On the **Router (Default Gateway)** page, add the default gateway:
   
   - In the **IP address** box, enter the gateway address (for example, 192.168.1.1).
   - Select **Add**.
   - Select **Next**.

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

---

## Configure DHCP options

DHCP options provide additional network configuration settings to client devices beyond IP addresses. You can configure options at the server level (applies to all scopes) or at the scope level (applies to a specific scope only).

### Configure scope-level options

1. In the DHCP console, expand **IPv4**, expand the scope, right-click **Scope Options**, and then select **Configure Options**.

2. In the **Scope Options** dialog box, select the options you want to configure:

   **To configure the default gateway (Router):**
   
   - Select the **003 Router** check box.
   - In the **IP address** box, enter the gateway address.
   - Select **Add**.

   **To configure DNS servers:**
   
   - Select the **006 DNS Servers** check box.
   - In the **IP address** box, enter the DNS server address.
   - Select **Add**.
   - Repeat for additional DNS servers.

   **To configure the DNS domain name:**
   
   - Select the **015 DNS Domain Name** check box.
   - In the **String value** box, enter your domain name (for example, contoso.com).

   **To configure the lease duration (Option 051):**
   
   - Select the **051 Lease** check box.
   - In the **Long** box, enter the lease duration in seconds.

3. After you configure all required options, select **OK**.

### Configure server-level options

Server-level options apply to all scopes on the DHCP server.

1. In the DHCP console, expand the server name, right-click **Server Options**, and then select **Configure Options**.

2. Configure the desired options using the same method as scope-level options.

3. Select **OK** to apply the settings.

**Note:** Scope-level options override server-level options. Configure common settings at the server level and scope-specific settings at the scope level.

---

## Set up DHCP failover for high availability

DHCP failover enables two DHCP servers to provide IP addresses for the same scope, ensuring service continuity if one server becomes unavailable. This feature supports load balance mode (both servers actively respond) and hot standby mode (one server is primary).

### Prerequisites

Before you configure DHCP failover, verify that:

- You have two DHCP servers installed and authorized in Active Directory.
- Both servers have network connectivity to each other.
- You have configured at least one scope on the primary DHCP server.
- The scope you want to replicate is not already part of a failover relationship.

### Configure DHCP failover

1. On the primary DHCP server, open the DHCP console.

2. Expand **IPv4**, right-click the scope you want to configure for failover, and then select **Configure Failover**.

3. In the **Configure Failover** wizard, on the **Introduction to DHCP Failover** page, verify the scope is selected, and then select **Next**.

4. On the **Specify the partner server to use for failover** page, configure the partner server:
   
   - Select **Add Server**.
   - In the **Add Server** dialog box, select the partner DHCP server, and then select **OK**.
   - Select **Next**.

5. On the **Create a new failover relationship** page, configure the failover settings:
   
   - In the **Relationship Name** box, enter a descriptive name (for example, "DHCP-Failover-01").
   - In the **Maximum Client Lead Time** box, accept the default (1 hour) or enter a custom value.
   - In the **Mode** dropdown list, select one of the following:
     - **Load Balance** - Both servers actively respond to DHCP requests. In the **Load Balance Percentage** boxes, configure the traffic distribution (default is 50/50).
     - **Hot Standby** - One server is active, the other is standby. In the **Role of Partner Server** dropdown list, select **Standby**. In the **Addresses reserved for standby server** box, enter the percentage of addresses reserved (default is 5%).
   - Optional: Select the **Enable Message Authentication** check box, and then enter a shared secret for secure communication between servers.
   - Select **Next**.

6. On the **Finish** page, review the configuration summary, and then select **Finish**.

7. On the **Configure Failover** results page, verify the operation completed successfully, and then select **Close**.

### Verify DHCP failover configuration

1. In the DHCP console on the primary server, expand **IPv4**, and then select the scope.

2. In the details pane, verify that the **Failover** column shows the failover relationship name.

3. Right-click the scope, and then select **Properties**.

4. Select the **Failover** tab to view failover relationship details.

5. On the partner DHCP server, open the DHCP console and verify that the replicated scope appears under **IPv4**.

**Note:** After you configure failover, scope changes made on either server automatically replicate to the partner server.

---

## Configure DHCP policies for advanced address assignment

DHCP policies enable you to assign IP addresses and options based on specific criteria such as device type, vendor class, user class, or MAC address. This allows for advanced scenarios like assigning different IP ranges to printers versus computers, or providing specific configurations to mobile devices.

### Create a scope-level policy

1. In the DHCP console, expand **IPv4**, expand the scope, right-click **Policies**, and then select **New Policy**.

2. In the **DHCP Policy Configuration Wizard**, on the **Policy Name** page, configure the following:
   
   - In the **Policy Name** box, enter a descriptive name (for example, "Printers Policy").
   - Optional: In the **Description** box, enter additional details.
   - Select **Next**.

3. On the **Configure Conditions for the policy** page, add conditions that identify which clients receive this policy:
   
   - Select **Add**.
   - In the **Add/Edit Condition** dialog box, configure the following:
     - In the **Criteria** dropdown list, select a condition type (for example, **Vendor Class**, **MAC Address**, **User Class**, or **Client Identifier**).
     - In the **Operator** dropdown list, select **Equals** or **Not Equals**.
     - In the **Value** box, enter the matching value (for example, for printers, you might use a vendor class like "HP Printer").
     - Select **Add**, and then select **OK**.
   - Select **Next**.

4. On the **Configure settings for the policy** page, configure IP address range settings:
   
   - Optional: Select **Yes** if you want to configure a specific IP address range for this policy.
   - If you selected **Yes**, enter the **Start IP address** and **End IP address** for the policy.
   - Select **Next**.

5. On the **Configure options for the policy** page, configure policy-specific DHCP options:
   
   - Select **Yes** if you want to configure specific DHCP options for devices matching this policy.
   - If you selected **Yes**, configure options such as default gateway, DNS servers, or other settings specific to this device type.
   - Select **Next**.

6. On the **Summary** page, review your policy configuration, and then select **Finish**.

### Create a server-level policy

Server-level policies apply across all scopes on the DHCP server.

1. In the DHCP console, expand the server name, right-click **Policies**, and then select **New Policy**.

2. Follow the same steps as creating a scope-level policy (steps 2-6 above).

### Configure policy processing order

If multiple policies apply to a client, DHCP processes them in order of priority.

1. In the DHCP console, navigate to **Policies** (either under a scope or under the server name).

2. In the details pane, verify the **Processing Order** column.

3. To change the processing order, right-click a policy, and then select **Move Up** or **Move Down**.

### Example policy scenarios

**Assign specific IP ranges to different device types:**

- Create a policy for printers using Vendor Class condition, assign them IPs from 192.168.1.150-192.168.1.200.
- Create a policy for computers using MAC Address prefix condition, assign them IPs from 192.168.1.50-192.168.1.149.

**Provide different DNS servers based on user class:**

- Create a policy for "Guest" user class with public DNS servers.
- Create a policy for "Employee" user class with internal DNS servers.

**Configure shorter lease times for mobile devices:**

- Create a policy based on Vendor Class for mobile devices.
- Configure a shorter lease duration (for example, 4 hours instead of 8 days).

---

## Next steps

After configuring DHCP scopes, options, failover, and policies, you can:

- Monitor DHCP server performance using the DHCP console or Performance Monitor.
- Review DHCP audit logs located in %SystemRoot%\System32\DHCP for troubleshooting.
- Configure DHCP reservations for devices that require consistent IP addresses.
- Set up DHCP relay agents for multi-subnet environments.
- Implement DHCP Name Protection to prevent name squatting in DNS.





<!---------------------------------------CLIENT SETUP------------------------------------------------------------------>
<!--------------------------------Create Client Virtual Machine-------------------------------------------------------->
# Part II: Setup and connect Windows Client
## Section 1: Create Windows Client VM

<!---------------------------------Setup Client network and connect to the servers domain------------------------------->

# Step 2: Setup Windows Client Network


## Configure to DHCP and DNS (IP of Windows Server - Domain controller)


## Validate from Server end that the computer is in Active Directory Users and Computers











