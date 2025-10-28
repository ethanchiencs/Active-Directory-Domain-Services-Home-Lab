# Active Directory Domain Services Home Lab

## Project Overview

**Completion Date:** September 2025  
**Training Source:** Microsoft Learn - AZ-1008: Administer Active Directory Domain Services  
**Environment:** Hyper-V on Windows 11  
**Domain:** tailwindtraders.internal

This project implements a comprehensive Active Directory Domain Services environment based on Microsoft's official AZ-1008 guided lab. The lab consists of four core exercises covering domain controller operations, user management, password policies, and security configurations, all following enterprise best practices.

---

## Lab Environment

### Infrastructure Specifications

| Component | Details |
|-----------|---------|
| **Primary Domain Controller** | TAILWIND-DC1 |
| **Secondary Domain Controller** | TAILWIND-MBR1 (promoted member server) |
| **Domain Name** | tailwindtraders.internal |
| **Forest/Domain Functional Level** | Windows Server 2022 |
| **Virtualization Platform** | Hyper-V Manager |
| **Network Configuration** | NAT Network (10.10.10.0/24) |
<img width="969" height="719" alt="Screenshot (3)" src="https://github.com/user-attachments/assets/2eea5ebe-320c-49be-94c4-7ea38f8e19a2" />
<img width="975" height="946" alt="Screenshot (2)" src="https://github.com/user-attachments/assets/03c46ee3-f605-43d3-8267-c786112d843c" />

### Administrative Credentials

- **Domain Admin:** `TAILWINDTRADERS\Administrator`
- **Password:** `Pa55w.rdPa55w.rd`
- **DSRM Password:** `Pa55w.rdPa55w.rd`

---

## Exercise 1: Configure Domain Controller Operations

### Task 1: Install AD DS and Promote to Domain Controller

**Objective:** Promote member server TAILWIND-MBR1 to become an additional domain controller

**Steps Completed:**

1. Signed in to TAILWIND-MBR1 as `TAILWINDTRADERS\Administrator`
2. Opened Server Manager → Manage → Add Roles and Features
3. Selected role-based or feature-based installation
4. Added Active Directory Domain Services role with all required features
5. Initiated promotion wizard from Server Manager notification
6. Selected "Add a domain controller to an existing domain"
7. Re-authenticated with domain Administrator credentials
8. Configured Directory Services Restore Mode (DSRM) password
9. Completed prerequisites check and installed AD DS
10. Server automatically restarted as operational domain controller
<img width="966" height="877" alt="Screenshot (7)" src="https://github.com/user-attachments/assets/a8a7c698-9b34-48b2-b496-aaaca1fd3294" />
<img width="963" height="1010" alt="Screenshot (8)" src="https://github.com/user-attachments/assets/1252565e-0ddd-403f-a8d9-1e4f7b1bd2e8" />

### Task 2: Transfer Flexible Single Master Operations (FSMO) Roles

**Objective:** Transfer RID Master role from TAILWIND-DC1 to TAILWIND-MBR1

**Steps Completed:**

1. Opened Active Directory Users and Computers on TAILWIND-MBR1
2. Right-clicked root node → All Tasks → Operations Masters
3. Navigated to RID tab
4. Clicked Change to transfer RID Master role
5. Confirmed transfer and verified successful completion

**Result:** RID Master role successfully transferred to TAILWIND-MBR1
<img width="963" height="1036" alt="Screenshot (9)" src="https://github.com/user-attachments/assets/460a8a8f-e411-4762-aeaa-88c0b4e73d67" />

### Task 3: Create Active Directory Site and Configure Subnet

**Objective:** Create Sydney site with associated 172.16.1.0/24 subnet

**Steps Completed:**

1. Signed in to TAILWIND-DC1 as `tailwindtraders\administrator`
2. Opened Active Directory Sites and Services from Tools menu
3. Right-clicked Sites → New Site → Named "Sydney"
4. Selected DEFAULTIPSITELINK as link name
5. Expanded Sites → Right-clicked Subnets → New Subnet
6. Entered prefix: `172.16.1.0/24`
7. Associated subnet with Sydney site
8. Verified site and subnet configuration
<img width="960" height="1031" alt="Screenshot (10)" src="https://github.com/user-attachments/assets/f5c6406e-6a70-4069-9721-8e9408829604" />

---

## Exercise 2: Configure User Management Operations

### Task 1: Create Organizational Units

**Objective:** Create geographic OUs for organizational structure

**OUs Created:**
- Sydney
- Melbourne
- Brisbane

**Steps Completed:**

1. Opened Active Directory Users and Computers on TAILWIND-DC1
2. Right-clicked tailwindtraders.internal domain
3. Selected New → Organizational Unit
4. Created three OUs for geographic organization
<img width="963" height="873" alt="Screenshot (11)" src="https://github.com/user-attachments/assets/5d65de73-8008-49d7-85a6-0e8f6c0c36cc" />

### Task 2: Create Users and Configure Account Properties

**Objective:** Create contractor user accounts with account expiration settings

**Steps Completed:**

1. Right-clicked Sydney OU → New → User
2. Created `SydneyContractor`:
   - Full name: SydneyContractor
   - User logon name: SydneyContractor
   - Password: `Pa55w.rdPa55w.rd`
3. Opened SydneyContractor properties → Account tab
4. Set Account Expires to: End of June 1, 2030
5. Right-clicked SydneyContractor → Copy
6. Created `MelbourneContractor` (same password)
7. Created `BrisbaneContractor` (same password)
8. Moved MelbourneContractor to Melbourne OU (drag and drop)
9. Moved BrisbaneContractor to Brisbane OU (drag and drop)
<img width="963" height="872" alt="Screenshot (12)" src="https://github.com/user-attachments/assets/77819c54-a359-46d8-998e-7f099c710670" />
<img width="963" height="870" alt="Screenshot (13)" src="https://github.com/user-attachments/assets/2252638a-b86b-41f1-b6fa-ced02b7cd430" />

### Task 3: Create Security Group

**Objective:** Create Sydney Administrators group for delegation

**Steps Completed:**

1. Right-clicked Sydney OU → New → Group
2. Named group: "Sydney Administrators"
3. Set Group scope: Universal
4. Opened SydneyContractor properties → Member Of tab
5. Added user to Sydney Administrators group
6. Verified group membership
<img width="966" height="870" alt="Screenshot (14)" src="https://github.com/user-attachments/assets/6090def2-1146-4d47-98a6-c74ccf0789d9" />

### Task 4: Configure Protected User

**Objective:** Add SydneyContractor to Protected Users security group for enhanced security

**Steps Completed:**

1. Opened SydneyContractor properties → Member Of tab
2. Clicked Add → Typed "Protected Users"
3. Clicked Check Names to verify group
4. Added user to Protected Users group
<img width="963" height="874" alt="Screenshot (15)" src="https://github.com/user-attachments/assets/731b37af-e9d3-419f-8a83-b844eb5137fe" />

**Security Impact:** Enhanced protection against credential theft attacks

### Task 5: Delegate Security Permissions

**Objective:** Delegate password reset permissions to Sydney Administrators

**Steps Completed:**

1. Right-clicked Sydney OU → Delegate Control
2. Clicked Add → Entered "Sydney Administrators"
3. Used Check Names to verify security group
4. Selected task: "Reset user passwords and force password change at next logon"
5. Completed delegation wizard
<img width="960" height="874" alt="Screenshot (16)" src="https://github.com/user-attachments/assets/354b00a4-78e9-4ec3-a3fe-7b12881dc3a3" />

**Result:** Sydney Administrators can now reset passwords within their OU

### Task 6: Configure City Attribute and Search

**Objective:** Set custom attribute and demonstrate Find functionality

**Steps Completed:**

1. Opened SydneyContractor properties → Address tab
2. Set City field to: "Sydney"
3. Right-clicked tailwindtraders.internal → Find
4. Navigated to Advanced tab
5. Selected Field → User → City
6. Set Condition: "Is (exactly)"
7. Set Value: "Sydney"
8. Clicked Find Now to search
9. Verified SydneyContractor appeared in search results
<img width="963" height="873" alt="Screenshot (17)" src="https://github.com/user-attachments/assets/8706ce43-a02d-40b6-ad00-7bba798d25b9" />

### Task 7: Disable User Account

**Objective:** Disable MelbourneContractor account

**Steps Completed:**

1. Navigated to Melbourne OU
2. Right-clicked MelbourneContractor → Disable Account
3. Verified account disabled status (down arrow icon)
<img width="960" height="873" alt="Screenshot (18)" src="https://github.com/user-attachments/assets/f923686b-a25d-4fcb-aa04-00df97e68f10" />

### Task 8: Reset User Password

**Objective:** Reset BrisbaneContractor password

**Steps Completed:**

1. Navigated to Brisbane OU
2. Right-clicked BrisbaneContractor → Reset Password
3. Entered new password: `Pa66w.rdPa66w.rd` (confirmed twice)
4. Verified password reset completion
<img width="963" height="875" alt="Screenshot (19)" src="https://github.com/user-attachments/assets/6458f877-e8c9-4ed2-97c9-091a252268d0" />

---

## Exercise 3: Manage Password Policies

### Task 1: Configure Domain Password Policy

**Objective:** Strengthen domain-wide password requirements

**Steps Completed:**

1. Opened Group Policy Management Console from Server Manager
2. Expanded tailwindtraders.internal forest → Domains → tailwindtraders.internal
3. Right-clicked Default Domain Policy → Edit
4. Navigated to: Computer Configuration\Policies\Windows Settings\Security Settings\Account Policies\Password Policy
5. Double-clicked "Minimum password length" policy
6. Changed minimum characters to **14 characters**
7. Applied and closed Group Policy Management Editor
<img width="963" height="874" alt="Screenshot (20)" src="https://github.com/user-attachments/assets/3680f033-0839-45fa-9e90-d3e6ac36cd52" />

**Policy Configuration:**
- **Minimum password length:** 14 characters
- **Applies to:** All domain users
- **Enforcement:** Domain-wide via Default Domain Policy

### Task 2: Configure Fine-Grained Password Policy

**Objective:** Create stricter password policy for Domain Admins

**Steps Completed:**

1. Opened Active Directory Administrative Center from Tools menu
2. Clicked tailwindtraders (local) under Overview
3. Opened System container → Password Settings Container
4. Right-clicked → New → Password Settings
5. Configured policy:
   - **Name:** "Domain Admin Password Policy"
   - **Precedence:** 1 (highest priority)
   - **Minimum password length:** 16 characters
6. In "Directly Applies To" section:
   - Clicked Add → Typed "Domain Admins"
   - Used Check Names to verify
   - Applied policy to Domain Admins group
<img width="966" height="877" alt="Screenshot (21)" src="https://github.com/user-attachments/assets/4ed83db1-5936-41ce-b9a6-2ac3d2ef00ab" />
<img width="963" height="873" alt="Screenshot (22)" src="https://github.com/user-attachments/assets/12ffa3ac-f5db-4cd6-a4d6-d2011f6e4165" />

**Policy Details:**
- Fine-Grained Password Policy (FGPP) for privileged accounts
- 16-character minimum (stricter than domain default)
- Precedence 1 ensures it overrides default policy
- **Targets:** Domain Admins security group

### Task 3: Enable Active Directory Recycle Bin

**Objective:** Enable AD Recycle Bin for object recovery

**Steps Completed:**

1. Opened Active Directory Administrative Center
2. Selected tailwindtraders (local) in left pane
3. Clicked "Enable Recycle Bin" in right pane
4. Acknowledged warning about irreversible change
5. Acknowledged replication latency warning
6. Verified Recycle Bin enabled status
<img width="957" height="874" alt="Screenshot (23)" src="https://github.com/user-attachments/assets/768a9870-7cb6-4ec4-bb3d-b68d240ca6fe" />

**Impact:** Enables recovery of accidentally deleted AD objects without authoritative restore

---

## Exercise 4: Configure Security Settings

### Task 1: Restrict NTLM Authentication

**Objective:** Disable legacy NTLM authentication for domain accounts

**Steps Completed:**

1. Opened Group Policy Management Console
2. Expanded tailwindtraders.internal → Group Policy Objects
3. Right-clicked Default Domain Controller Policy → Edit
4. Navigated to: Computer Configuration\Policies\Windows Settings\Security Settings\Local Policies\Security Options
5. Double-clicked "Network security: Restrict NTLM: NTLM authentication in this domain"
6. Checked "Define this policy setting"
7. Selected value: **Deny all**
8. Applied policy
<img width="957" height="1031" alt="Screenshot (24)" src="https://github.com/user-attachments/assets/7d170bd2-de35-4af3-b65f-b6275dc31c80" />

**Security Impact:**
- Blocks NTLM authentication domain-wide
- Forces use of Kerberos (more secure protocol)
- Prevents pass-the-hash attacks
- Hardens authentication mechanisms

### Task 2: Audit User Account Management in Sydney OU

**Objective:** Enable detailed auditing of account management activities

**Steps Completed:**

1. Opened Group Policy Management Console
2. Navigated to Sydney OU
3. Right-clicked → Create a GPO in this domain, and link it here
4. Named GPO: "SydneyOUPolicy"
5. Right-clicked SydneyOUPolicy → Edit
6. Navigated to: Computer Configuration\Policies\Windows Settings\Security Settings\Advanced Audit Policy Configuration\Audit Policies\Account Management
7. Double-clicked "Audit User account management"
8. Checked "Configure the following audit events"
9. Selected both **Success** and **Failure** events
10. Applied policy
<img width="963" height="1029" alt="Screenshot (25)" src="https://github.com/user-attachments/assets/7a1cfe99-db5e-4508-b7e1-7442cba140da" />
<img width="960" height="1028" alt="Screenshot (26)" src="https://github.com/user-attachments/assets/82d805d3-afcc-4d08-bd7c-63b11004da12" />

**Auditing Configuration:**
- Tracks all user account changes in Sydney OU
- Logs both successful and failed operations
- Captures: account creation, deletion, modification, password resets
- Enables compliance and security monitoring

### Task 3: Deny Log On As a Service

**Objective:** Restrict service account logon for Sydney Administrators

**Steps Completed:**

1. Opened Group Policy Management Console
2. Navigated to Sydney OU → SydneyOUPolicy
3. Right-clicked → Edit
4. Navigated to: Computer Configuration\Policies\Windows Settings\Security Settings\Local Policies\User Rights Assignment
5. Double-clicked "Deny Log on as a service"
6. Checked "Define this policy setting"
7. Clicked Add User or Group → Browse → Advanced → Find Now
8. Selected "Sydney Administrators" group
9. Applied configuration
<img width="957" height="1033" alt="Screenshot (27)" src="https://github.com/user-attachments/assets/de9f7b62-6f45-436a-9986-45ad8265186c" />

**Security Purpose:**
- Prevents Sydney Administrators from running as service accounts
- Reduces attack surface for delegated admin accounts
- Implements principle of least privilege
- Separates service accounts from user accounts

---

## Skills Demonstrated

### Active Directory Administration
- Domain controller deployment and promotion
- Multi-DC environment configuration
- FSMO role management and transfer
- Sites and Services configuration
- Subnet management with CIDR notation
- Organizational Unit structure design

### User and Identity Management
- User account creation and lifecycle management
- Account property configuration (expiration, attributes)
- Password resets and forced password changes
- Account enabling/disabling
- Security group creation and management
- Group membership administration
- Protected Users configuration

### Security and Compliance
- Group Policy Object creation and linking
- Domain password policy configuration (14-character minimum)
- Fine-Grained Password Policies (16-character for admins)
- Legacy protocol restriction (NTLM blocking)
- Advanced audit policy configuration
- User Rights Assignment management
- Active Directory Recycle Bin enablement

### Administrative Delegation
- Delegation of Control Wizard
- Role-based access control (RBAC)
- Least-privilege principle implementation
- OU-level permission delegation

### Tools and Interfaces
- Active Directory Users and Computers (ADUC)
- Active Directory Administrative Center (ADAC)
- Group Policy Management Console (GPMC)
- Active Directory Sites and Services
- Server Manager
- Hyper-V Manager
- PowerShell (network configuration)

### Help Desk Operations
- Password resets
- Account lockout resolution
- User account searches and queries
- Account status modifications
- Group membership changes
- Custom attribute management

---

## Security Best Practices Implemented

### Password Security
- 14-character minimum for all domain users
- 16-character minimum for Domain Admins (FGPP)
- Complexity requirements enabled
- Password history enforcement

### Authentication Hardening
- NTLM authentication blocked (Kerberos only)
- Protected Users group for sensitive accounts
- Service account logon restrictions

### Audit and Compliance
- User account management auditing (success/failure)
- Security event logging for forensics
- Account activity tracking

### Administrative Security
- Delegated permissions (least privilege)
- Separated administrative duties
- Role-based access control
- Multiple domain controllers (redundancy)

### Disaster Recovery
- Active Directory Recycle Bin enabled
- Multiple domain controllers for failover
- DSRM password configured

---

## Conclusion

This Active Directory Domain Services home lab successfully demonstrates enterprise-level identity and access management capabilities. The implementation follows Microsoft's official training curriculum and incorporates industry best practices for security, compliance, and administrative delegation. The multi-domain controller environment provides hands-on experience with real-world AD operations that are directly applicable to IT support, systems administration, and cybersecurity roles.
