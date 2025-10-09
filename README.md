# Active-Directory-Domain-Services-Home-Lab
Active Directory Domain Services Home Lab
Project Completion: August 2025
Training Source: Microsoft Learn - AZ-1008: Administer Active Directory Domain Services
Lab Type: Guided Microsoft Learning Lab (4 Exercises)
Environment: Hyper-V on Windows 11
Domain: tailwindtraders.internal
Project Overview
Completed Microsoft's official Active Directory Domain Services guided lab (AZ-1008) consisting of 4 comprehensive exercises. Deployed a production-ready multi-domain controller environment with organizational structure, user lifecycle management, password policies, security configurations, and administrative delegation following enterprise best practices.  
Lab Structure:  
Exercise 2: Configure Domain Controller Operations  
Exercise 3: Configure User Management Operations  
Exercise 4: Manage Password Policies  
Exercise 5: Configure Security Settings  

Lab Environment Setup
Infrastructure Components

Primary Domain Controller: TAILWIND-DC1
Secondary Domain Controller: TAILWIND-MBR1 (promoted member server)
Domain Name: tailwindtraders.internal
Forest/Domain Functional Level: Windows Server 2022
Virtualization Platform: Hyper-V Manager
Network Configuration: NAT Network (10.10.10.0/24)

Administrative Credentials

Domain Admin: TAILWINDTRADERS\Administrator
Password: Pa55w.rdPa55w.rd
DSRM Password: Pa55w.rdPa55w.rd


Lab 2: Configure Domain Controller Operations
Task 1: Install AD DS and Promote to Domain Controller
Objective: Promote member server TAILWIND-MBR1 to become additional domain controller
Steps Completed:

Signed in to TAILWIND-MBR1 as TAILWINDTRADERS\Administrator
Opened Server Manager → Manage → Add Roles and Features
Selected Role-based or feature-based installation
Added Active Directory Domain Services role with all required features
Clicked notification icon in Server Manager to start promotion wizard
Selected "Add a domain controller to an existing domain"
Re-authenticated as Administrator with domain credentials
Configured Directory Services Restore Mode (DSRM) password
Completed prerequisites check and installed AD DS
Server automatically restarted and became operational domain controller

Skills Demonstrated:

Domain controller deployment and configuration
Server role installation via Server Manager
Understanding of DSRM and disaster recovery
Multi-DC environment setup for redundancy

Task 2: Transfer Flexible Single Master Operations (FSMO) Roles
Objective: Transfer RID Master role from TAILWIND-DC1 to TAILWIND-MBR1
Steps Completed:

Opened Active Directory Users and Computers on TAILWIND-MBR1
Right-clicked root node → All Tasks → Operations Masters
Navigated to RID tab
Clicked Change to transfer RID Master role
Confirmed transfer and verified successful completion

Skills Demonstrated:

FSMO role management and understanding
Operations Master role transfer procedures
Load balancing across domain controllers

Task 3: Create Active Directory Site and Configure Subnet
Objective: Create Sydney site with associated 172.16.1.0/24 subnet
Steps Completed:

Signed in to TAILWIND-DC1 as tailwindtraders\administrator
Opened Active Directory Sites and Services from Tools menu
Right-clicked Sites → New Site → Named "Sydney"
Selected DEFAULTIPSITELINK as link name
Expanded Sites folder → Right-clicked Subnets → New Subnet
Entered prefix: 172.16.1.0/24
Associated subnet with Sydney site
Verified site and subnet configuration

Skills Demonstrated:

Active Directory Sites and Services configuration
Subnet management and IP addressing (CIDR notation)
Site topology planning for replication


Lab 3: Configure User Management Operations
Task 1: Create Organizational Units
Objective: Create three geographic OUs for organizational structure
Steps Completed:

Opened Active Directory Users and Computers on TAILWIND-DC1
Right-clicked tailwindtraders.internal domain
Selected New → Organizational Unit
Created three OUs:  
Sydney
Melbourne
Brisbane

Skills Demonstrated:

OU design and hierarchy planning
Organizational structure implementation

Task 2: Create Users and Configure Account Properties
Objective: Create contractor user accounts with account expiration
Steps Completed:

Right-clicked Sydney OU → New → User
Created SydneyContractor:

Full name: SydneyContractor
User logon name: SydneyContractor
Password: Pa55w.rdPa55w.rd


Opened SydneyContractor properties → Account tab
Set Account Expires to: End of January 1, 2030
Right-clicked SydneyContractor → Copy
Created MelbourneContractor (same password)
Created BrisbaneContractor (same password)
Moved MelbourneContractor to Melbourne OU (drag and drop)
Moved BrisbaneContractor to Brisbane OU (drag and drop)

Skills Demonstrated:

User account creation and lifecycle management
Account expiration configuration for temporary workers
Efficient user creation using copy function
OU-based organizational management

Task 3: Create Security Group
Objective: Create Sydney Administrators group for delegation
Steps Completed:

Right-clicked Sydney OU → New → Group
Named group: "Sydney Administrators"
Set Group scope: Universal
Opened SydneyContractor properties → Member Of tab
Added user to Sydney Administrators group
Verified group membership

Skills Demonstrated:

Security group creation and management
Group scope selection (Universal, Global, Domain Local)
Group membership administration

Task 4: Configure Protected User
Objective: Add SydneyContractor to Protected Users security group
Steps Completed:

Opened SydneyContractor properties → Member Of tab
Clicked Add → Typed "Protected Users"
Clicked Check Names to verify group
Added user to Protected Users group for enhanced security

Skills Demonstrated:

Advanced security features (Protected Users)
Understanding of credential protection mechanisms
Security group management for privileged accounts

Task 5: Delegate Security Permissions
Objective: Delegate password reset permissions to Sydney Administrators
Steps Completed:

Right-clicked Sydney OU → Delegate Control
Clicked Add → Entered "Sydney Administrators"
Used Check Names to verify security group
Selected task: "Reset user passwords and force password change at next logon"
Completed delegation wizard

Skills Demonstrated:

Administrative delegation using Delegation of Control Wizard
Least-privilege access control implementation
Role-based access control (RBAC) principles
Help desk task delegation

Task 6: Configure City Attribute and Search
Objective: Set custom attribute and use Find feature
Steps Completed:

Opened SydneyContractor properties → Address tab
Set City field to: "Sydney"
Right-clicked tailwindtraders.internal → Find
Navigated to Advanced tab
Selected Field → User → City
Set Condition: "Is (exactly)"
Set Value: "Sydney"
Clicked Find Now to search
Verified SydneyContractor appeared in search results

Skills Demonstrated:

User attribute management
Advanced Active Directory search capabilities
Custom field configuration for user properties

Task 7: Disable User Account
Objective: Disable MelbourneContractor account
Steps Completed:

Navigated to Melbourne OU
Right-clicked MelbourneContractor → Disable Account
Verified account disabled status (account icon showed down arrow)

Skills Demonstrated:

Account lifecycle management
User account disabling procedures
Offboarding processes

Task 8: Reset User Password
Objective: Reset BrisbaneContractor password
Steps Completed:

Navigated to Brisbane OU
Right-clicked BrisbaneContractor → Reset Password
Entered new password: Pa66w.rdPa66w.rd (twice)
Confirmed password reset completion

Skills Demonstrated:

Password reset procedures (Tier 1 help desk task)
Secure password handling
User support operations


Lab 4: Manage Password Policies
Task 1: Configure Domain Password Policy
Objective: Strengthen domain-wide password requirements
Steps Completed:

Opened Group Policy Management Console from Server Manager
Expanded tailwindtraders.internal forest → Domains → tailwindtraders.internal
Right-clicked Default Domain Policy → Edit
Navigated to: Computer Configuration\Policies\Windows Settings\Security Settings\Account Policies\Password Policy
Double-clicked "Minimum password length" policy
Changed minimum characters from default to 14 characters
Applied and closed Group Policy Management Editor

Policy Configuration:

Minimum password length: 14 characters
Applies to: All domain users
Enforcement: Domain-wide via Default Domain Policy

Skills Demonstrated:

Group Policy Management Console navigation
Domain password policy configuration
Security baseline implementation
Compliance with password complexity standards

Task 2: Configure Fine-Grained Password Policy
Objective: Create stricter password policy for Domain Admins
Steps Completed:

Opened Active Directory Administrative Center from Tools menu
Clicked tailwindtraders (local) under Overview
Opened System container
Opened Password Settings Container
Right-clicked → New → Password Settings
Configured policy:

Name: "Domain Admin Password Policy"
Precedence: 1 (highest priority)
Minimum password length: 16 characters


In "Directly Applies To" section:

Clicked Add → Typed "Domain Admins"
Used Check Names to verify
Applied policy to Domain Admins group



Policy Details:

Fine-Grained Password Policy (FGPP) for privileged accounts
16-character minimum (stricter than domain default)
Precedence 1 ensures it overrides default policy
Targets: Domain Admins security group

Skills Demonstrated:

Fine-Grained Password Policy (FGPP) implementation
Active Directory Administrative Center usage
Differentiated password requirements for privileged accounts
Password Settings Container management
Security hardening for administrative accounts

Task 3: Enable Active Directory Recycle Bin
Objective: Enable AD Recycle Bin for object recovery
Steps Completed:

Opened Active Directory Administrative Center
Selected tailwindtraders (local) in left pane
Clicked "Enable Recycle Bin" in right pane
Acknowledged warning about irreversible change
Acknowledged replication latency warning
Verified Recycle Bin enabled status

Skills Demonstrated:

Active Directory Recycle Bin configuration
Disaster recovery preparation
Understanding of AD replication
Forest-level feature enablement


Lab 5: Configure Security Settings
Task 1: Restrict NTLM Authentication
Objective: Disable legacy NTLM authentication for domain accounts
Steps Completed:

Opened Group Policy Management Console
Expanded tailwindtraders.internal → Group Policy Objects
Right-clicked Default Domain Controller Policy → Edit
Navigated to: Computer Configuration\Policies\Windows Settings\Security Settings\Local Policies\Security Options
Double-clicked "Network security: Restrict NTLM: NTLM authentication in this domain"
Checked "Define this policy setting"
Selected value: Deny all
Confirmed setting change
Applied policy

Security Impact:

Blocks NTLM authentication domain-wide
Forces use of Kerberos (more secure protocol)
Prevents pass-the-hash attacks
Hardens authentication mechanisms

Skills Demonstrated:

Legacy protocol restriction
Security hardening configuration
Group Policy security settings
Understanding of authentication protocols (NTLM vs Kerberos)

Task 2: Audit User Account Management in Sydney OU
Objective: Enable detailed auditing of account management activities
Steps Completed:

Opened Group Policy Management Console
Navigated to Sydney OU
Right-clicked → Create a GPO in this domain, and link it here
Named GPO: "SydneyOUPolicy"
Right-clicked SydneyOUPolicy → Edit
Navigated to: Computer Configuration\Policies\Windows Settings\Security Settings\Advanced Audit Policy Configuration\Audit Policies\Account Management
Double-clicked "Audit User account management"
Checked "Configure the following audit events"
Selected both Success and Failure events
Applied policy

Auditing Configuration:

Tracks all user account changes in Sydney OU
Logs both successful and failed operations
Captures: account creation, deletion, modification, password resets
Enables compliance and security monitoring

Skills Demonstrated:

Advanced Audit Policy Configuration
OU-specific Group Policy creation and linking
Security event monitoring setup
Compliance and forensic preparation

Task 3: Deny Log On As a Service
Objective: Restrict service account logon for Sydney Administrators
Steps Completed:

Opened Group Policy Management Console
Navigated to Sydney OU → SydneyOUPolicy
Right-clicked → Edit
Navigated to: Computer Configuration\Policies\Windows Settings\Security Settings\Local Policies\User Rights Assignment
Double-clicked "Deny Log on as a service"
Checked "Define this policy setting"
Clicked Add User or Group → Browse → Advanced → Find Now
Selected "Sydney Administrators" group
Clicked OK through all dialog boxes to apply

Security Purpose:

Prevents Sydney Administrators from running as service accounts
Reduces attack surface for delegated admin accounts
Implements principle of least privilege
Separates service accounts from user accounts

Skills Demonstrated:

User Rights Assignment configuration
Security principal restriction
Service account security best practices
Group-based security policy application


Complete Skills Summary
Active Directory Administration
✓ Domain controller deployment and promotion
✓ Multi-DC environment configuration
✓ FSMO role management and transfer
✓ Sites and Services configuration
✓ Subnet management with CIDR notation
✓ Organizational Unit structure design
User and Identity Management
✓ User account creation and lifecycle management
✓ Account property configuration (expiration, attributes)
✓ Password resets and forced password changes
✓ Account enabling/disabling
✓ Security group creation and management
✓ Group membership administration
✓ Protected Users configuration
Security and Compliance
✓ Group Policy Object creation and linking
✓ Domain password policy configuration (14-character minimum)
✓ Fine-Grained Password Policies (16-character for admins)
✓ Legacy protocol restriction (NTLM blocking)
✓ Advanced audit policy configuration
✓ User Rights Assignment management
✓ Active Directory Recycle Bin enablement
Administrative Delegation
✓ Delegation of Control Wizard
✓ Role-based access control (RBAC)
✓ Least-privilege principle implementation
✓ OU-level permission delegation
Tools and Interfaces
✓ Active Directory Users and Computers (ADUC)
✓ Active Directory Administrative Center (ADAC)
✓ Group Policy Management Console (GPMC)
✓ Active Directory Sites and Services
✓ Server Manager
✓ Hyper-V Manager
✓ PowerShell (network configuration)
Help Desk Operations (Tier 1/2)
✓ Password resets
✓ Account lockout resolution
✓ User account searches and queries
✓ Account status modifications
✓ Group membership changes
✓ Custom attribute management

Security Best Practices Implemented
Password Security:

14-character minimum for all domain users
16-character minimum for Domain Admins (FGPP)
Complexity requirements enabled
Password history enforcement

Authentication Hardening:

NTLM authentication blocked (Kerberos only)
Protected Users group for sensitive accounts
Service account logon restrictions

Audit and Compliance:

User account management auditing (success/failure)
Security event logging for forensics
Account activity tracking

Administrative Security:

Delegated permissions (least privilege)
Separated administrative duties
Role-based access control
Multiple domain controllers (redundancy)

Disaster Recovery:

Active Directory Recycle Bin enabled
Multiple domain controllers for failover
DSRM password configured


Lab Completion Evidence
Completed Labs:

✅ Lab 2: Configure Domain Controller Operations
✅ Lab 3: Configure User Management Operations
✅ Lab 4: Manage Password Policies
✅ Lab 5: Configure Security Settings

Training Source: Microsoft Learn
Course: AZ-1008 - Administer Active Directory Domain Services
Lab URLs:

https://microsoftlearning.github.io/AZ-1008-Administer-Active-Directory-Domain-Services/


Real-World Applications
This lab simulates enterprise Active Directory environments and prepares for:
Help Desk/Desktop Support Roles:

Daily password reset operations
Account lockout troubleshooting
User account management
Group membership modifications

Systems Administrator Roles:

Domain controller management
Group Policy implementation
Security baseline configuration
Audit policy setup

IT Security Roles:

Password policy enforcement
Authentication protocol hardening
Audit logging configuration
Access control implementation


Project Takeaways
Technical Competencies Gained:

Comprehensive Active Directory administration
Enterprise identity and access management
Group Policy-based security configuration
Multi-server environment management
Security hardening and compliance

Soft Skills Developed:

Following detailed technical documentation
Systematic troubleshooting approach
Attention to security details
Documentation of technical procedures

Career Readiness:

Ready for help desk positions requiring AD knowledge
Prepared for systems administrator interview questions
Demonstrates self-directed learning
Shows commitment to IT career development
