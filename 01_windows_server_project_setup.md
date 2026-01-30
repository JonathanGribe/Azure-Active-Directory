
# Fundamental Windows Server 2022 Environment -  Setup Lab

<img width="792" height="173" alt="image" src="https://github.com/user-attachments/assets/478c6b5a-4414-4ee0-8f0f-3c61d2e5a039" />

# Table of Contents

## Part I: Server Setup
## Part II: Client Setup and link

# Purpose of the Lab

# Tools and Technology
## Microsoft Azure

# Part I: Setup Server
# Step 1: Create Azure Virtual Machine with Server 2022 Datacenter image
### 1. Navigate to the Azure portal: portal.azure.com
### 2. Select **Create a Resource**
<img width="1149" height="527" alt="image" src="https://github.com/user-attachments/assets/3001d223-9d16-42dd-b0b9-56abe337f37a" />

### 3. Select Create under Virtual Machine
<img width="1024" height="948" alt="image" src="https://github.com/user-attachments/assets/35da79cf-55a1-4b02-89e0-1dbce54d6ab7" />

### 4. Configure the Virtual Machine
#### Basics
<img width="1425" height="2059" alt="image" src="https://github.com/user-attachments/assets/f3e02b2b-9069-4538-8373-7478b3f031db" />

#### Network
<img width="1229" height="1160" alt="image" src="https://github.com/user-attachments/assets/1cf97e31-b270-4dfa-95e1-d1022c35a691" />





# Step 2: Install AD DS role and escalate to Domain Controller
## 1. Remote into VM via RDP 
### Test connection with Powershell
## 2. Configure server with static IP address and temporary DNS 

## 3. Install AD DS role

## 4. Promote to Domain Controller
### Set Server's DNS

# Step 5: Install DHCP Role

#Part II: Setup and connect Windows Client
#Step 1: Create Windows Client VM

#Step 2: Setup Windows Client Network
## Configure to DHCP and DNS (IP of Windows Server - Domain controller)
## Validate from Server end that the computer is in Active Directory Users and Computers











