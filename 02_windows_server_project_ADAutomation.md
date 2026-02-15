# Windows Server 2025 - Active Directory and Powershell Automation

# Table of Contents
# Purpose of Lab
<!----------------Lab begins----------------------------------->
# Active Directory User Basics
## Active Directory primer

Active Directory Domain Services (AD DS) is Microsoft's directory service that stores information about network objects such as users, computers, and groups. It organizes these objects into a hierarchical structure of domains, forests, and organizational units (OUs), enabling centralized management and authentication across your network. Understanding the distinction between user objects (the actual directory entry) and user accounts (the security principal), as well as Security Identifiers (SIDs), is fundamental to working effectively with AD DS.

When creating and managing user accounts in Active Directory, you'll work primarily with Active Directory Users and Computers (ADUC). User accounts can be domain-based (authenticated by AD) or local (authenticated by individual machines), with domain users being identified by their User Principal Name (UPN) in the format username@domain.com. Each user account contains multiple attributes across different property tabs (General, Account, Profile, Organization), some required and others optional, along with critical account options like password policies, expiration settings, and account status flags that control how the account functions within your domain.

Organizational structure and bulk management capabilities become essential as your environment grows. OUs provide a framework for organizing users logically, applying Group Policies, and delegating administrative control. When managing multiple users simultaneously, bulk operations using CSV files and PowerShell scripts become necessary. Understanding CSV file structure, required fields, and data validation is critical for successful bulk user creation and modification—skills that directly translate to managing user provisioning at scale.

For organizations planning cloud migration, specific user attributes take on heightened importance. Email addresses, proxy addresses, and properly configured UPN suffixes are critical for Azure AD/Entra ID synchronization. Attributes like Employee ID, department, and title not only help organize your on-premises directory but also ensure smooth identity synchronization to cloud services. Additionally, understanding password policies, account lockout policies, user rights, and group membership basics forms the security foundation necessary before transitioning to hybrid or cloud-based identity management.

**Additional Learning Resources**

- [Active Directory Domain Services Overview - Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)
- [Create a User Account in Active Directory - Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#create-a-user-account)
- [Organizational Units in Active Directory - Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/creating-an-organizational-unit-design)
- [Azure AD Connect: Accounts and Permissions - Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/reference-connect-accounts-permissions)
- [Password Policies and Account Lockout - Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/component-updates/password-policy)
Accessing Active Directory Users and Computers

### Default Active Directory Containers and OUs
When you load Active Directory Users and Computers you will see several folders on the left-hand side under the domain controller **corp.myserver.com**

<img width="306" height="256" alt="image" src="https://github.com/user-attachments/assets/012d3746-f85c-416d-ac9f-cef07794db54" />

- **Builtin** - Contains default security groups created during Active Directory installation. These groups have predefined rights and permissions for domain administration tasks (e.g., Administrators, Backup Operators, Account Operators). These groups cannot be moved or deleted.

- **Computers** - The default container where computer accounts are created when computers join the domain without specifying a target OU. Best practice: Create custom OUs for computers and redirect new computer accounts to those OUs instead of using this default container.

- **Domain Controllers** - An Organizational Unit (OU) that contains all domain controller computer accounts in the domain. This is the only default OU created during AD DS installation. Group Policy Objects (GPOs) specific to domain controllers are typically linked here.

- **ForeignSecurityPrincipals** - Contains Security Identifiers (SIDs) for security principals from trusted external domains. When you add users or groups from another trusted domain to a group in your domain, a ForeignSecurityPrincipal object is created here as a placeholder.

- **Managed Service Accounts** - A container for Managed Service Accounts (MSAs) and Group Managed Service Accounts (gMSAs). These are special account types designed for services and scheduled tasks, providing automatic password management and simplified service principal name (SPN) management.

- **Users** - The default container for new user accounts and groups created without specifying a target OU. Contains several built-in groups like Domain Admins, Domain Users, and the default Administrator account. Best practice: Create custom OUs for users and organize accounts there instead of using this default container.

# Automating User creation with Powershell

## OU Structure
This is how we are going to organize all of our personel in our simple company, part of the corp.myserver.com domain controller
```
corp.myserver.com
│
└── Corp
    ├── Users
    │   ├── Executives
    │   ├── Managers
    │   ├── Staff
    │   └── Service Accounts
    ├── Computers
    │   ├── Workstations
    │   └── Servers
    └── Groups
        ├── Security
        └── Distribution

```

## Using Powershell to create the OU structure in Active Directory:
```powershell
New-ADOrganizationalUnit -Name "Corp" -Path "DC=corp,DC=myserver,DC=com"

New-ADOrganizationalUnit -Name "Users" -Path "OU=Corp,DC=corp,DC=myserver,DC=com"
New-ADOrganizationalUnit -Name "Computers" -Path "OU=Corp,DC=corp,DC=myserver,DC=com"
New-ADOrganizationalUnit -Name "Groups" -Path "OU=Corp,DC=corp,DC=myserver,DC=com"

New-ADOrganizationalUnit -Name "Executives" -Path "OU=Users,OU=Corp,DC=corp,DC=myserver,DC=com"
New-ADOrganizationalUnit -Name "Managers" -Path "OU=Users,OU=Corp,DC=corp,DC=myserver,DC=com"
New-ADOrganizationalUnit -Name "Staff" -Path "OU=Users,OU=Corp,DC=corp,DC=myserver,DC=com"
New-ADOrganizationalUnit -Name "Service Accounts" -Path "OU=Users,OU=Corp,DC=corp,DC=myserver,DC=com"

New-ADOrganizationalUnit -Name "Workstations" -Path "OU=Computers,OU=Corp,DC=corp,DC=myserver,DC=com"
New-ADOrganizationalUnit -Name "Servers" -Path "OU=Computers,OU=Corp,DC=corp,DC=myserver,DC=com"

New-ADOrganizationalUnit -Name "Security" -Path "OU=Groups,OU=Corp,DC=corp,DC=myserver,DC=com"
New-ADOrganizationalUnit -Name "Distribution" -Path "OU=Groups,OU=Corp,DC=corp,DC=myserver,DC=com"



```
For adding a new OU within another OU you will need to find the OU Path:
```powershell
-Path "OU=IT,DC=corp,DC=myserver,DC=com"
```

Finding the OU path for account creation (manual):
1. Select **View** in Active Directory Users and Computers
2. Select **Advanced Features**
3. Right-click the OU you would like to acquire a path for
4. Select **Properties**
5. Select the **Attribute Editor tab**
6. Copy the **distinguishedName**
7. Paste where needed

<img width="401" height="455" alt="image" src="https://github.com/user-attachments/assets/f82b8a13-6518-4d05-b0f3-d1c1453cfe43" />



Once the script has been run, then it will generate the new Organizational Units:

<img width="265" height="325" alt="image" src="https://github.com/user-attachments/assets/271c50f5-0258-4760-a357-d470bd733d58" />


Note: You can verify the object is an Organization Unit:
1. Select **View**
2. Select **Advanced Features**
3. Right-Click the OU you would like to check
4. Select **Properties**
5. Select the **Object Tab**
6. Verify next to **Object class** it reads **Organizational Unit**   

<img width="808" height="545" alt="image" src="https://github.com/user-attachments/assets/8c26e9d2-1a63-4fda-95ff-f2932bc1020d" />


## Creating New Users
Now that we have the OU structure in place we need to create users for the new OUs:
Two Methods:
1. Manual creation
2. Automatic creation via script
3. Reading in a .csv file

## Manually Creating User Accounts

Link to Microsoft website

### Creating users via powershell script
```powershell

#Manually adding a user and adding them to a corresponding OU:

Import-Module ActiveDirectory

# --- Collect user information first ---
$firstname = Read-Host "First name"
$lastname = Read-Host "Last name"
$username = "$firstname.$lastname"
$description = Read-Host "Enter a description" 
$password = ConvertTo-SecureString "myPassword123" -AsPlainText -Force

Write-Host ""
Write-Host "Select the OU to create the user in:"
Write-Host "1. Executives"
Write-Host "2. Managers"
Write-Host "3. Staff"
Write-Host "4. Service Accounts"
Write-Host "5. Admin Users"

$choice = Read-Host "Enter the number of your choice"

# --- Map selection to OU path ---
switch ($choice) {
    "1" { $ouPath = "OU=Executives,OU=Users,OU=Corp,DC=corp,DC=myserver,DC=com" }
    "2" { $ouPath = "OU=Managers,OU=Users,OU=Corp,DC=corp,DC=myserver,DC=com" }
    "3" { $ouPath = "OU=Staff,OU=Users,OU=Corp,DC=corp,DC=myserver,DC=com" }
    "4" { $ouPath = "OU=Service Accounts,OU=Users,OU=Corp,DC=corp,DC=myserver,DC=com" }
    "5" { $ouPath = "OU=Admin Users,OU=IT,DC=corp,DC=myserver,DC=com" }
    default {
        Write-Host "Invalid selection. Exiting."
        exit
    }
}

# --- Create the user ---
New-ADUser -Name "$firstname $lastname" `
-GivenName $firstname `
-Surname $lastname `
-SamAccountName $username `
-UserPrincipalName "$username@corp.myserver.com" `
-Description $description `
-AccountPassword $password `
-Enabled $True `
-Path $ouPath

#Force Password Change at next Login
Set-ADUser -Identity $username -ChangePasswordAtLogon $True 

Write-Host ""
Write-Host "User $firstname $lastname created successfully in:"
Write-Host $ouPath

```
## Modified Script (Importing a .csv file using powershell)

This will allow us to create a more realistic Active Directory environment:






Creation of the .csv file from excel:



   
## Reading a .CSV file

# Creating the Active Directory Environment

