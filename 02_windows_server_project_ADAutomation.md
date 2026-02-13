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
## Basic Script

```powershell
Import-Module ActiveDirectory

$firstname = Read-Host "First name"
$lastname = Read-Host "Last name"
$password = ConvertTo-SecureString "myPassword" -AsPlainText -Force

New-ADUser -Name "$firstname $lastname" `
           -GivenName $firstname `
           -Surname $lastname `
           -AccountPassword $password `
           -Enabled $True

Write-Host "User created successfully!"

```
## Modified Script (Importing a .csv file using powershell)

This will allow us to create a more realistic Active Directory environment:

Creation of the .csv file from excel:

Finding the OU path for account creation (manual):
1. Select **View** in Active Directory Users and Computers
2. Select **Advanced Features**
3. Right-click the OU you would like to acquire a path for
4. Select **Properties**
5. Select the **Attribute Editor tab**
6. 
## Reading a .CSV file

# Creating the Active Directory Environment

