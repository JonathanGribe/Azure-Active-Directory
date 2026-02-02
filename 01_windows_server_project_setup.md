
# Fundamental Windows Server 2022 Environment -  Setup Lab

<img width="826" height="156" alt="image" src="https://github.com/user-attachments/assets/c0bdc42c-1106-4ed0-b35b-4fe833aae8b8" />



# Table of Contents

# Purpose of the lab

# Tools and Technology



<!------------------------------SERVER SETUP------------------------------------->

# Part I: Setup Server

<!----------------------------Setup Azure Virtual Machine------------------------>
## Step 1: Create Azure Virtual Machine with Server 2025 Datacenter image
### 1.  Sign in to the Azure Portal at https://portal.azure.com

### 2. Select **Create a Resource**
<img width="765" height="318" alt="image" src="https://github.com/user-attachments/assets/1cc3488f-46a8-4b20-961f-ad9e9d1261e1" />

### 3. Select Create under Virtual Machine
<img width="763" height="628" alt="image" src="https://github.com/user-attachments/assets/4d8853f4-6553-4646-b724-0ba051e0908b" />


### 4. Configure the Virtual Machine

#### Under the Basics tab
1. Select or create a new **Resource group**.
2. Enter a **Virtual machine name**.
3. Choose the **Image**:
   - *Windows Server 2025 Datacenter: Azure Edition – x64 Gen2*
4. Create a **Username** and **Password**.
5. Select the checkbox:
   - *Would you like to use an existing Windows Server license?*


<img width="1425" height="2059" alt="image" src="https://github.com/user-attachments/assets/f3e02b2b-9069-4538-8373-7478b3f031db" />

#### Under the Network tab
1. Select or create a new **Virtual Network** and **Subnet** if not provided.
   - IMPORTANT: Remember the Virtual network and subnet.  We will use it when we setup the client machine
  
2. Select or create a new Public IP if not provided
3. Select **Review + create**
4. Select **Create**
<img width="1229" height="1160" alt="image" src="https://github.com/user-attachments/assets/1cf97e31-b270-4dfa-95e1-d1022c35a691" />


# Step 2: Install AD DS role and promote to Domain Controller

<!-----------------Remoting into the Server via RDP ------------------------->

## 1. Remote into the Windows Server virtual machine via RDP

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

## 2. Configure server with static IP address and temporary DNS

<!--------------------------------Install AD DS server role-------------------------------------------->

## 3. Install AD DS role

<!-------------------------Promote AD DS role to Domain controller---------------------------------->

## 4. Promote to Domain Controller
## 5.  Set Server's DNS

<!------------------------------------Install DHCP Role------------------------------------------------------>
# Step 4: Install DHCP Role


<!---------------------------------------CLIENT SETUP------------------------------------------------------------------>
<!--------------------------------Create Client Virtual Machine-------------------------------------------------------->
# Part II: Setup and connect Windows Client
## Step 1: Create Windows Client VM

<!---------------------------------Setup Client network and connect to the servers domain------------------------------->

# Step 2: Setup Windows Client Network
## Configure to DHCP and DNS (IP of Windows Server - Domain controller)
## Validate from Server end that the computer is in Active Directory Users and Computers











