Hybrid Azure AD Join with On-Premises AD

This project demonstrates how to manually configure a HYBRID AZURE AD Join environment using WINDOWS SERVER 2019 and AZURE AD Connect. The goal is to sync an on-premises Active Directory domain with Azure AD and enable automatic registration of domain-joined devices.

---

## Project Environment

- Server OS: Windows Server 2019  
- RAM: 32 GB  
- CPU: Intel 4-core processor  
- Storage: 32 TB HDD  
- Client OS: Windows 10  
- Azure Account: Active and configured

---

## Objective

To perform a manual Hybrid Azure AD Join setup using:
- Active Directory Domain Services (AD DS)
- Azure AD Connect sync
- Group Policy for auto-registration

---

## Step-by-Step Process

### Step 1: Install Active Directory Domain Services (AD DS)

1. Open **Server Manager**.
2. Click on **Add Roles and Features**.
3. Select the **Active Directory Domain Services** checkbox.
4. Accept all default settings and click **Install**.
5. After installation, click the notification flag in Server Manager and select **Promote this server to a domain controller**.
6. Use all default settings and install. The server will reboot automatically.

---

### ✅ Step 2: Create a New Forest and Domain

1. Choose **Add a new forest** option.
2. Enter **maxhome.com** as the root domain name.
3. Set a password and complete the domain creation process.

---

### ✅ Step 3: Create Domain Users

1. Go to **Tools > Active Directory Users and Computers**.
2. Right-click on **Users** and select **New > User**.
3. Create two users:
   - `maxhomeuser1`
   - `maxhomeuser2`

---

### ✅ Step 4: Install Azure AD Connect

1. Search for **"Azure AD Connect"** in your browser.
2. Go to **Microsoft Entra Connect** and download the provisioning agent.
3. On the Azure portal:
   - Create a new user `maxportal`.
   - Assign **Global Administrator** permissions to it.
4. Download and install **Azure AD Connect** on your server.
5. During setup, enter the credentials for `maxportal`.
6. Complete the setup and sync your local AD with Azure AD.
7. Verify that `maxhomeuser1` and `maxhomeuser2` are now visible in the **Azure AD portal**.

---

### Step 5: Enable Auto-Registration via Group Policy

1. Open **Group Policy Management**.
2. Expand the domain `maxhome.com`.
3. Right-click on **Group Policy Objects > New**.
4. Name the policy: `hybridjoindomainregistration`.
5. Right-click and **Edit** it.
6. Navigate to:
Computer Configuration >Policies >Administrative Templates >Windows Components >
Device Registration
7. Double-click **Register domain-joined computers as devices**, set it to **Enabled**, and click **OK**.

---

### Step 6: Link the GPO to the Domain

1. In **Group Policy Management**, right-click on the domain `maxhome.com`.
2. Select **Link an existing GPO**.
3. Choose `hybridjoindomainregistration` and click **OK**.

---

### Step 7: Testing the Hybrid Join

1. Use a Windows 10 client machine connected to a different network.
2. Log in using the synced Azure AD user:maxhomeuser1@maxiemiliyangmail.onmicrosoft.com
3. Open **Command Prompt as Administrator**.
4. Run the following command:
dsregcmd /status
5.Got the output:
DomainJoined : YES
AzureADJoined : YES
This confirms that Hybrid Azure AD Join is successfully configured.

Conclusion
This setup bridges the on-premises Active Directory environment with Azure Active Directory, allowing to manage hybrid identity and enable seamless access to cloud resources using a single set of credentials.
