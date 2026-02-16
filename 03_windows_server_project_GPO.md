# Windows Server 2025: Active Directory - Implementing Group Policy

# Table of Contents
Part I: High Overview
1. Compliance Standards
2. Organizational Policy
3. Technical Controls
4. GPO creation

Part II: GPO Implementation
1. Different Implementation Method
2. Manual
3. Powershell

# PART I: Concepts

## Framework
The framework for this section is taken from governance structures suggested by industry frameworks such as NIST 800-53, ISO/IEC 27001, CIS Controls, and Microsofts Group Policy deployment guidance. There is a process by which we take the standards, create organizational policy & technical controls, which are finally implemented as Group Policy Objects (GPOs). 


## Compliance Standards
Organizational security policies are derived from compliance frameworks such as NIST SP 800‑53 and ISO/IEC 27001. For example, NIST AC‑11 requires automatic session locking, which organizations typically implement through technical controls such as Group Policy. Microsoft’s Group Policy documentation confirms that GPOs are the primary mechanism for enforcing security settings across Windows domains. This is in accordance to order of precedence where an 'enforced policy' can override local policies to establish consistent configuration across AD environments.
https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-policy/group-policy-processing


As per NIST 800-53 Control PL-2 Organizations are required to create an orgnization policy and document procedure describing how the policy is implemented within the organization:  
https://csf.tools/reference/nist-sp-800-53/r5/pl/pl-2/

This can be found at: https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final
## Organizational Policy
https://learn.microsoft.com/en-us/windows/security/operating-system-security/device-management/windows-security-configuration-framework/windows-security-baselines

```
 Organizational Security Policy
(Modeled after NIST SP 800 53 and ISO/IEC 27001 principles)
1. Purpose
The purpose of this policy is to establish security requirements for the protection of organizational information systems and data. This policy defines the minimum technical and administrative controls required to ensure confidentiality, integrity, and availability across all corporate computing environments.

2. Scope
This policy applies to:
•	All employees, contractors, and third party users
•	All organization owned or managed devices
•	All systems connected to the corporate network
•	All cloud and on premises environments

3. Policy Statements
3.1 Access Control
1.	User accounts must be uniquely assigned to individuals.
2.	Multi factor authentication (MFA) must be used for all remote access and administrative accounts.
3.	Access rights must follow the principle of least privilege and be reviewed quarterly.
4.	Shared accounts are prohibited except where technically unavoidable and explicitly approved.

Standards Basis:
•	NIST SP 800 53: AC 2, AC 3, IA 2
•	ISO/IEC 27001: A.5.17, A.8.2, A.9.2.3
________________________________________
3.2 Password Requirements
1.	Passwords must be at least 14 characters in length.
2.	Passwords must include a combination of uppercase, lowercase, numbers, and symbols.
3.	Passwords must be changed every 180 days unless MFA is enforced.
4.	Password reuse is prohibited for the last 24 password histories.

Standards Basis:
•	NIST SP 800 53: IA 5
•	ISO/IEC 27001: A.5.17
________________________________________
3.3 Session Management
1.	Workstations must automatically lock after 10 minutes of inactivity.
2.	Servers must automatically lock after 15 minutes of inactivity.
3.	Users must authenticate to unlock a session.

Standards Basis:
•	NIST SP 800 53: AC 11
•	ISO/IEC 27001: A.9.4.2
________________________________________
3.4 System Configuration and Hardening
1.	All systems must be configured according to approved security baselines.
2.	Unauthorized software installation is prohibited.
3.	USB storage devices are disabled by default unless approved by IT Security.
4.	Windows Firewall must remain enabled on all endpoints.

Standards Basis:
•	NIST SP 800 53: CM 6, CM 7
•	ISO/IEC 27001: A.8.9, A.12.1.2
________________________________________
3.5 Logging and Monitoring
1.	Security logs must be retained for a minimum of 90 days.
2.	Administrative actions must be logged and reviewed weekly.
3.	Systems must report security events to the centralized SIEM.

Standards Basis:
•	NIST SP 800 53: AU 2, AU 6
•	ISO/IEC 27001: A.8.15
________________________________________
3.6 Patch and Vulnerability Management
1.	Critical security patches must be applied within 14 days of release.
2.	Monthly patch cycles must be performed on all systems.
3.	Vulnerability scans must be conducted at least quarterly.

Standards Basis:
•	NIST SP 800 53: SI 2, RA 5
•	ISO/IEC 27001: A.8.8, A.5.28
________________________________________
3.7 Acceptable Use
1.	Corporate systems must be used for authorized business purposes only.
2.	Users must not attempt to bypass security controls.
3.	Personal devices may not connect to the corporate network without approval.

Standards Basis:
•	NIST SP 800 53: PL 4, AC 20
•	ISO/IEC 27001: A.5.10
________________________________________
4. Enforcement
Violations of this policy may result in disciplinary action, up to and including termination of employment, as well as potential legal consequences.
________________________________________
5. Review and Maintenance
This policy must be reviewed annually or whenever significant changes occur in regulatory requirements, technology, or business operations.
________________________________________

```




## Technical Controls
The implementation of the Organizations policy into 'technical controls'

This Technical controls document will explain how we will implement the organizational policy.

```
Technical Controls Standard
1. Purpose
This document defines the technical controls required to enforce the organization’s security policies across all systems, networks, and environments. These controls translate policy statements into actionable, measurable, and auditable technical requirements.
2. Scope
These controls apply to:
•	All employees, contractors, and third party users
•	All organization owned or managed devices
•	All systems connected to the corporate network
•	All cloud and on premises environments
________________________________________
3. Technical Controls
3.1 Access Control
3.1.1 Unique User Accounts
•	All systems must integrate with centralized identity management (e.g., Active Directory, Azure AD).
•	Account creation must require identity verification and managerial approval.
•	Service accounts must be uniquely identifiable and documented.
3.1.2 Multi Factor Authentication (MFA)
•	MFA must be enforced for: 
o	All remote access (VPN, cloud portals, remote desktop gateways)
o	All administrative accounts (local admin, domain admin, cloud admin)
•	MFA must use approved methods (e.g., authenticator app, hardware token, FIDO2 key).
3.1.3 Least Privilege & Quarterly Access Review
•	Role based access control (RBAC) must be implemented for all systems.
•	Privileged groups (e.g., Domain Admins, Global Admins) must be monitored and tightly restricted.
•	Quarterly access reviews must be performed and documented by system owners.
3.1.4 Shared Accounts
•	Shared accounts must be disabled unless explicitly approved by IT Security.
•	Approved shared accounts must: 
o	Have documented justification
o	Use password vaulting
o	Have full audit logging enabled
Standards Basis: NIST SP 800 53 AC 2, AC 3, IA 2; ISO/IEC 27001 A.5.17, A.8.2, A.9.2.3
________________________________________
3.2 Password Requirements
3.2.1 Password Length & Complexity
•	Systems must enforce a minimum password length of 14 characters.
•	Complexity must require uppercase, lowercase, numbers, and symbols unless using passphrases.
3.2.2 Password Expiration
•	Passwords must expire every 180 days unless MFA is enforced.
•	Accounts without MFA must follow stricter expiration policies.
3.2.3 Password Reuse
•	Systems must enforce a 24 password history to prevent reuse.
3.2.4 Technical Enforcement
•	Password policies must be enforced via: 
o	Group Policy (Windows)
o	Identity provider configuration (Azure AD, Okta, etc.)
o	Local OS security configuration for non domain systems
Standards Basis: NIST SP 800 53 IA 5; ISO/IEC 27001 A.5.17
________________________________________
3.3 Session Management
3.3.1 Automatic Locking
•	Workstations must lock after 10 minutes of inactivity.
•	Servers must lock after 15 minutes of inactivity.
3.3.2 Authentication on Unlock
•	Users must re authenticate using their assigned credentials.
•	Cached credentials must not bypass lock screen authentication.
3.3.3 Technical Enforcement
•	Windows systems: Group Policy (GPO)
•	Linux systems: /etc/profile, GNOME/KDE settings, or equivalent
•	Cloud consoles: session timeout configuration
Standards Basis: NIST SP 800 53 AC 11; ISO/IEC 27001 A.9.4.2
________________________________________
3.4 System Configuration & Hardening
3.4.1 Security Baselines
•	All systems must follow approved baselines (e.g., CIS Benchmarks, Microsoft Security Baselines).
•	Baseline compliance must be validated using automated tools (e.g., Intune, GPO, SCCM, Ansible).
3.4.2 Software Installation Restrictions
•	Only IT approved software may be installed.
•	Application allowlisting must be used where feasible (e.g., AppLocker, WDAC).
3.4.3 USB Storage Controls
•	USB storage must be disabled by default.
•	Exceptions require documented approval and device level monitoring.
3.4.4 Host Firewall Enforcement
•	Windows Firewall must be enabled on all endpoints.
•	Firewall rules must be centrally managed and monitored.
Standards Basis: NIST SP 800 53 CM 6, CM 7; ISO/IEC 27001 A.8.9, A.12.1.2
________________________________________
3.5 Logging & Monitoring
3.5.1 Log Retention
•	Security logs must be retained for at least 90 days in a tamper resistant system.
3.5.2 Administrative Action Logging
•	All privileged actions must be logged, including: 
o	Group membership changes
o	Privilege elevation
o	System configuration changes
•	Logs must be reviewed weekly by IT Security.
3.5.3 Centralized SIEM Integration
•	All systems must forward logs to the centralized SIEM.
•	Log forwarding failures must generate alerts.
Standards Basis: NIST SP 800 53 AU 2, AU 6; ISO/IEC 27001 A.8.15
________________________________________
3.6 Patch & Vulnerability Management
3.6.1 Critical Patch Timelines
•	Critical patches must be deployed within 14 days of vendor release.
•	Emergency patches may require accelerated deployment.
3.6.2 Monthly Patch Cycles
•	All systems must receive monthly OS and application updates.
•	Patch status must be tracked and reported.
3.6.3 Vulnerability Scanning
•	Vulnerability scans must be performed quarterly at minimum.
•	High risk findings must be remediated within defined SLAs.
Standards Basis: NIST SP 800 53 SI 2, RA 5; ISO/IEC 27001 A.8.8, A.5.28
________________________________________
4. Compliance & Exceptions
•	Compliance is monitored by IT Security through audits, automated tools, and log analysis.
•	Exceptions must be formally documented, risk assessed, and approved by the CISO or delegate.
________________________________________
5. Revision History
Version	Date	Description	Author
1.0	YYYY MM DD	Initial release	Security Team
________________________________________
If you'd like, I can also generate:
•	A matching Procedures document
•	A control to policy mapping matrix
•	A CIS/NIST/ISO crosswalk
•	A one page executive summary
Just tell me what direction you want to take this next.


```

## GPO creation
The actual process of creating the GPOs to be implemented at various levels

Creating GPOs 
Creating the individual GPOs for the lab environment
   
# PART II: GPO Implementation (based on the Organizational Policy/Technical Controls)

In this part we take our technical controls and implement them as group policy objects.
“GPO enforcement is performed prior to Azure migration to ensure consistent baseline configuration, reduce configuration drift, and provide a stable foundation for mapping controls into cloud‑based management (Intune/Azure AD). This approach aligns with Microsoft’s hybrid identity and configuration management guidance.”

