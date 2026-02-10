# Windows Server 2025 - Active Directory and Powershell Automation

# Table of Contents
# Purpose of Lab
<!----------------Lab begins----------------------------------->
# Active Directory User Basics
## Accessing Active Directory Users and Computers

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
## Modified Script (To include topics like exceptions handling)
## Reading a .CSV file

# Creating the Active Directory Environment

