# Windows Server 2025 Lab Continued... Active Directory - Implementing Group Policy

# Table of Contents
Part I: Introduction and Technical Controls
Part II: Implementation of Technical Controls

# Part I: Introduction
Get into how Technical controls are formed. If you want or just list them below, lmao

## Technical Controls to Implement

```
 Technical Controls to Implement
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

# PART II: GPO Implementation (based on the Organizational Policy/Technical Controls)

In this part we take our technical controls and implement them as group policy objects.

## General Workflow for implementing Group Policy Objects:
1. In the Server Management Console, Select Tools, then Group Policy Management
2. In the left-pane expand the forest and domain
3. Right-Click the Group Policy Objects container, and select New
4. Link the GPO by Right-Clicking on the OU and select Link and Existing GPO
5. Edit the GPO

Implementation of these policies is in the file: 






