
Manual Steps for Hybrid Azure AD Join Setup
===========================================

This document describes the step-by-step manual configuration process for setting up Hybrid Azure AD Join between an on-premises Active Directory domain and Azure Active Directory.

-------------------------------------------

On-Prem Environment Setup
-------------------------

1. Windows Server 2019 Installation
- Installed Windows Server 2019 (Specs: 32 GB RAM, 4-core processor, 32 TB HDD)

2. Active Directory Domain Services (AD DS)
- Promoted server to a Domain Controller
- Created new domain: maxhome.com

3. Domain Users Creation
- Created two users:
  - maxhomeuser1
  - maxhomeuser2

-------------------------------------------

Azure Environment Setup
-----------------------

4. Azure AD Global Admin Setup
- Azure subscription already active
- Created global administrator user: maxportal

5. Azure AD Connect Installation
- Downloaded and installed Azure AD Connect on the on-prem server
- Selected Hybrid Azure AD Join configuration
- Used maxportal (global admin) to connect Azure AD
- Chose domain maxhome.com for synchronization

-------------------------------------------

Group Policy Configuration
--------------------------

6. Enable Auto-Registration of Devices
- Opened Group Policy Management Console
- Created and linked a GPO to the OU where domain users/computers reside
- Enabled the following setting:
  Computer Configuration → Administrative Templates → Windows Components → Device Registration
  → Register domain joined computers as devices: Enabled

-------------------------------------------

Verification
------------

7. Forced Group Policy Update
Command:
gpupdate /force

8. Device Join Status Check
Command:
dsregcmd /status

Expected Output:
- AzureADJoined: YES
- DomainJoined: YES
- DeviceId & TenantId present

-------------------------------------------

Screenshots
-----------
Screenshots verifying successful configuration are available in the /screenshots folder.

Notes
-----
- This setup was done manually without using any automation tools.
- Project was tested and verified using test domain and Azure tenant.
