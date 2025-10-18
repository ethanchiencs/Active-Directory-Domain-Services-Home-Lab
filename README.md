# Active-Directory-Domain-Services-Home-Lab
<hx> Active Directory Domain Services Home Lab  </hx>  
Project Completion: September 2025  
Training Source: Microsoft Learn - AZ-1008: Administer Active Directory Domain Services  
Environment: Hyper-V on Windows 11  
Domain: tailwindtraders.internal  
Project Overview:  
Completed Microsoft's official Active Directory Domain Services guided lab (AZ-1008) consisting of 4 comprehensive exercises. Deployed a production-ready multi-domain controller environment with organizational structure, user lifecycle management, password policies, security configurations, and administrative delegation following enterprise best practices.  

Lab Structure:  
Configure Domain Controller Operations  
Configure User Management Operations  
Manage Password Policies  
Configure Security Settings  

Primary Domain Controller: TAILWIND-DC1  
Secondary Domain Controller: TAILWIND-MBR1 (promoted member server)  
Domain Name: tailwindtraders.internal    
Forest/Domain Functional Level: Windows Server 2022  
Virtualization Platform: Hyper-V Manager  
Network Configuration: NAT Network (10.10.10.0/24)  
<img width="975" height="946" alt="Screenshot (2)" src="https://github.com/user-attachments/assets/84314a37-1fb3-4a35-8ff3-7872bfef74d7" />

Administrative Credentials:
Domain Admin: TAILWINDTRADERS\Administrator  
Password: Pa55w.rdPa55w.rd  
DSRM Password: Pa55w.rdPa55w.rd  

<b> Configure Domain Controller Operations  </b>  
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
<img width="963" height="1010" alt="Screenshot (8)" src="https://github.com/user-attachments/assets/0c98aeaa-ccb4-499d-a171-852e872c1b86" />

Task 2: Transfer Flexible Single Master Operations (FSMO) Roles  
Objective: Transfer RID Master role from TAILWIND-DC1 to TAILWIND-MBR1  
Steps Completed:  
Opened Active Directory Users and Computers on TAILWIND-MBR1  
Right-clicked root node → All Tasks → Operations Masters  
Navigated to RID tab  
Clicked Change to transfer RID Master role  
Confirmed transfer and verified successful completion    
<img width="963" height="1036" alt="Screenshot (9)" src="https://github.com/user-attachments/assets/fafedf81-c907-49e9-a6f6-82f22541a7eb" />

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
<img width="960" height="1031" alt="Screenshot (10)" src="https://github.com/user-attachments/assets/b2c1ce78-2cdb-4096-a3de-50874626b727" />

<b>Configure User Management Operations  </b>  
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

Task 2: Create Users and Configure Account Properties  
Objective: Create contractor user accounts with account expiration  
Steps Completed:  

Right-clicked Sydney OU → New → User  
Created SydneyContractor:  
Full name: SydneyContractor  
User logon name: SydneyContractor  
Password: Pa55w.rdPa55w.rd  
<img width="963" height="872" alt="Screenshot (12)" src="https://github.com/user-attachments/assets/364de8fd-5f84-40e9-a101-86cffb859037" />


Opened SydneyContractor properties → Account tab  
Set Account Expires to: End of January 1, 2030  
<img width="963" height="870" alt="Screenshot (13)" src="https://github.com/user-attachments/assets/f5bf9c4d-bb97-49e4-a2de-6a4000ea4c52" />

Right-clicked SydneyContractor → Copy  
Created MelbourneContractor (same password)  
Created BrisbaneContractor (same password)  
Moved MelbourneContractor to Melbourne OU (drag and drop)  
Moved BrisbaneContractor to Brisbane OU (drag and drop)  

Task 3: Create Security Group  
Objective: Create Sydney Administrators group for delegation  
Steps Completed:  

Right-clicked Sydney OU → New → Group  
Named group: "Sydney Administrators"  
Set Group scope: Universal  
Opened SydneyContractor properties → Member Of tab  
Added user to Sydney Administrators group  
Verified group membership  

Task 4: Configure Protected User  
Objective: Add SydneyContractor to Protected Users security group  
Steps Completed:  

Opened SydneyContractor properties → Member Of tab  
Clicked Add → Typed "Protected Users"  
Clicked Check Names to verify group  
Added user to Protected Users group for enhanced security  
<img width="963" height="874" alt="Screenshot (15)" src="https://github.com/user-attachments/assets/2610f46a-97ee-461c-bff4-c20bb39dc2c1" />

Task 5: Delegate Security Permissions  
Objective: Delegate password reset permissions to Sydney Administrators  
Steps Completed:  

Right-clicked Sydney OU → Delegate Control  
Clicked Add → Entered "Sydney Administrators"  
Used Check Names to verify security group 
Selected task: "Reset user passwords and force password change at next logon"  
Completed delegation wizard  
<img width="960" height="874" alt="Screenshot (16)" src="https://github.com/user-attachments/assets/347e4d4c-4c93-4a6f-b9da-717da4281000" />

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
<img width="963" height="873" alt="Screenshot (17)" src="https://github.com/user-attachments/assets/85aa2409-8ab8-4536-9d70-bb84bdd4b7bf" />

Task 7: Disable User Account  
Objective: Disable MelbourneContractor account  
Steps Completed:  

Navigated to Melbourne OU  
Right-clicked MelbourneContractor → Disable Account  
Verified account disabled status (account icon showed down arrow)  
<img width="960" height="873" alt="Screenshot (18)" src="https://github.com/user-attachments/assets/ef05ec0f-cd36-4914-8f3b-c0d41a319ac9" />

Task 8: Reset User Password  
Objective: Reset BrisbaneContractor password  
Steps Completed:  

Navigated to Brisbane OU  
Right-clicked BrisbaneContractor → Reset Password  
Entered new password: Pa66w.rdPa66w.rd (twice)  
Confirmed password reset completion  
<img width="963" height="875" alt="Screenshot (19)" src="https://github.com/user-attachments/assets/d94b1999-ec31-48a9-b0c3-e87265dbc9db" />

<b> Manage Password Policies  </b>  
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
<img width="963" height="874" alt="Screenshot (20)" src="https://github.com/user-attachments/assets/0d37f03d-ace1-4195-aa10-7ec5ed6d1e5c" />

Policy Configuration:  

Minimum password length: 14 characters  
Applies to: All domain users  
Enforcement: Domain-wide via Default Domain Policy  

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
<img width="966" height="877" alt="Screenshot (21)" src="https://github.com/user-attachments/assets/dae660c8-48c2-4f42-9078-465fb3d4978c" />


In "Directly Applies To" section:  
Clicked Add → Typed "Domain Admins"  
Used Check Names to verify  
Applied policy to Domain Admins group  
<img width="963" height="873" alt="Screenshot (22)" src="https://github.com/user-attachments/assets/13e2e660-5773-4ed3-9aa7-9d6ebff253d0" />

Policy Details:  

Fine-Grained Password Policy (FGPP) for privileged accounts  
16-character minimum (stricter than domain default)  
Precedence 1 ensures it overrides default policy  
Targets: Domain Admins security group  

Task 3: Enable Active Directory Recycle Bin  
Objective: Enable AD Recycle Bin for object recovery  
Steps Completed:  
 
Opened Active Directory Administrative Center  
Selected tailwindtraders (local) in left pane  
Clicked "Enable Recycle Bin" in right pane  
Acknowledged warning about irreversible change  
Acknowledged replication latency warning  
Verified Recycle Bin enabled status  
<img width="957" height="874" alt="Screenshot (23)" src="https://github.com/user-attachments/assets/d056cb6d-d3b8-4f01-b374-d8c166c34345" />

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
<img width="957" height="1031" alt="Screenshot (24)" src="https://github.com/user-attachments/assets/5819c20c-c63b-4e1b-a503-c3dbf3cbe447" />

Security Impact:  

Blocks NTLM authentication domain-wide  
Forces use of Kerberos (more secure protocol)  
Prevents pass-the-hash attacks  
Hardens authentication mechanisms  

Task 2: Audit User Account Management in Sydney OU  
Objective: Enable detailed auditing of account management activities  
Steps Completed:  

Opened Group Policy Management Console  
Navigated to Sydney OU  
Right-clicked → Create a GPO in this domain, and link it here  
Named GPO: "SydneyOUPolicy"  
<img width="963" height="1029" alt="Screenshot (25)" src="https://github.com/user-attachments/assets/97f1887d-ea23-4a7b-95a7-c02a7937fc3d" />

Right-clicked SydneyOUPolicy → Edit  
Navigated to: Computer Configuration\Policies\Windows Settings\Security Settings\Advanced Audit Policy Configuration\Audit Policies\Account Management  
Double-clicked "Audit User account management"  
Checked "Configure the following audit events"  
Selected both Success and Failure events  
Applied policy  
<img width="960" height="1028" alt="Screenshot (26)" src="https://github.com/user-attachments/assets/887460df-c884-4a00-935f-0184642301e4" />

Auditing Configuration:  

Tracks all user account changes in Sydney OU  
Logs both successful and failed operations  
Captures: account creation, deletion, modification, password resets  
Enables compliance and security monitoring  

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
<img width="957" height="1033" alt="Screenshot (27)" src="https://github.com/user-attachments/assets/cdb1c6f2-b64a-4434-a8c7-dbd71f577dbe" />

Security Purpose:  

Prevents Sydney Administrators from running as service accounts  
Reduces attack surface for delegated admin accounts  
Implements principle of least privilege  
Separates service accounts from user accounts  

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

