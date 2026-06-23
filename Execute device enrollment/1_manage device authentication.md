> `Azure, Microsoft Configuration Manager, Microsoft Intune, Microsoft Entra ID`

# Microsoft Entra ID — Device Authentication & Join
 
---
 
## Overview
 
Starting with **Windows 10** and continuing with **Windows 11**, organizations can deploy **Microsoft Entra joined devices** as an alternative (or complement) to traditional Active Directory Domain Services (AD DS) domain joining.
 
Microsoft Entra ID (formerly Azure Active Directory) allows organizations to create device objects in the cloud directory and manage those devices from the cloud enabling SSO, compliance enforcement, and modern device management through tools like Microsoft Intune.
 
This document covers four learning units:
 
| Unit | Topic |
|------|-------|
| 2 | Describe Microsoft Entra Join |
| 3 | Prerequisites, Limitations, and Benefits |
| 4 | Joining Devices to Microsoft Entra ID |
| 5 | Managing Devices Joined to Microsoft Entra ID |
 
---
 
## Unit 1 — Describe Microsoft Entra Join
 
### Comparison: AD DS Domain Join vs. Microsoft Entra Join
 
| Feature | AD DS Domain Join | Microsoft Entra Join |
|---|---|---|
| Typical OS Support | Windows Pro/Enterprise (not Home/RT) | Windows 10/11 (not Home editions), Windows Server 2019+ VMs in Azure |
| Primary Resource Access | On-premises apps and services | Cloud-based apps and services (SSO) |
| Management Tool | Group Policy | Microsoft Intune (MDM) |
| Account Type | Domain accounts (optionally synced) | Microsoft Entra ID accounts |
| Home Edition Support | ❌ | ❌ |
 
> **Note:** Devices running Windows 10 Home, Pro, Enterprise, Windows Server, macOS, iOS, or Android **can register** with Microsoft Entra ID (device registration, not full domain join). Registered devices can access cloud-based and Azure-based resources via SSO and can be managed through Intune. However, Group Policy management is **not available** for registered-only devices.
 
---
 
### Usage Scenarios for Microsoft Entra Join
 
#### Scenario 1: Businesses Primarily in the Cloud
 
Organizations that operate primarily in the cloud or are migrating to it can use Microsoft Entra join to:
 
- Sign users into Windows with their Microsoft Entra ID accounts
- Join machines to Microsoft Entra ID through the **First Run Experience (FRX)** or via **Settings**
- Enable SSO to cloud resources like **Microsoft 365** (in browser and Office apps)
#### Scenario 2: Educational Institutions
 
Educational institutions typically have two categories of users:
 
- **Faculty** — Longer-term staff; on-premises accounts are appropriate
- **Students** — Shorter-term members; accounts can be managed in Microsoft Entra ID, pushing directory scale to the cloud
Students can sign in to Windows with their Microsoft Entra accounts and access Microsoft 365 resources in Office applications.
 
#### Additional Scenarios Where Microsoft Entra Join Is Recommended
 
- **Most apps and resources are cloud-based** — Ensures ease of access and SSO for cloud services (e.g., Microsoft 365)
- **Separating temporary accounts** — Contractors and seasonal workers can be provisioned in Microsoft Entra ID without polluting on-premises AD; limited cloud services can be scoped per account
- **BYOD (Bring Your Own Device)** — Supports non-Windows devices (iPads, Android tablets) that cannot join an AD DS domain but can enroll in Intune and Microsoft Entra ID
- **Transitioning to cloud-based infrastructure** — Using Microsoft Entra ID with an MDM such as Intune
- **Remote branch offices with limited on-premises infrastructure** — Enables device-joining capabilities for workers without requiring on-premises infrastructure
> **Admin Preparation:** Before devices can join, you must prepare Microsoft Entra ID. In the **Azure Portal**, navigate to your Microsoft Entra instance → **Users and groups** → **Device settings** to configure joining permissions.
 
---
 
### Microsoft Entra Hybrid Join
 
If your organization has an **on-premises Active Directory environment** and you want those domain-joined devices to also register with Microsoft Entra ID, you configure **Microsoft Entra hybrid joined devices**.
 
#### Supported Operating Systems for Hybrid Join
 
| Category | Operating Systems |
|---|---|
| Current | Windows 11, Windows 10, Windows 8.1 (except Home editions) |
| Down-level | Windows Server 2008/R2, 2012/R2, 2016, 2019, 2022 |
 
> **Important:** For down-level devices (non-Windows 10), you must install **Microsoft Workplace Join for non-Windows 10 computers**, available from the Microsoft Download Center.
 
> **Limitation:** Microsoft Entra hybrid join **cannot** be used if your environment consists of a single forest that synchronizes identity data to **more than one Microsoft Entra tenant**.
 
#### Reasons to Use Microsoft Entra Hybrid Join
 
- You have **Win32 apps** deployed to devices that rely on Active Directory machine authentication
- You require **Group Policy** to manage some devices
- You want to continue using **imaging solutions** to configure devices for employees
> **Note:** Microsoft Entra hybrid join automatically registers on-premises domain-joined devices with Microsoft Entra ID. If you don't want all devices to register automatically, you should **control** which devices undergo Microsoft Entra hybrid join.
 
---
 
## Unit 2 — Prerequisites, Limitations, and Benefits
 
### Prerequisites
 
To join a Windows device to Microsoft Entra ID, the following conditions must be met:
 
1. **Device Registration Service must be configured** — The service must be enabled so that devices can be registered in your Microsoft Entra tenant.
2. **Device count must be within the configured maximum** — The number of registered devices must be fewer than the configured maximum for your tenant.
3. **Federated tenant requirement** — If your tenant is **federated**, your Identity Provider (IdP) **must support**:
   - **WS-Fed** (WS-Federation)
   - **WS-Trust username/password endpoint** — version **1.3** or **2005**
   
   > This protocol support is required both to join the device to Microsoft Entra ID **and** to log on to the device with a password.
#### Organization Type Suitability
 
Microsoft Entra join is designed for:
 
- **Cloud-first organizations** — Primarily use cloud services with a goal to reduce on-premises infrastructure
- **Cloud-only organizations** — No on-premises infrastructure at all
> There are **no restrictions** on the size or type of organization that can deploy Microsoft Entra join. It also works in **hybrid environments**, enabling access to both cloud and on-premises apps and resources.
 
---
 
### Benefits of Microsoft Entra Joined Devices
 
| Benefit | Description |
|---|---|
| **SSO to Azure-managed SaaS apps** | No additional authentication prompts when accessing work resources. SSO remains active even when disconnected from the domain network. |
| **Enterprise compliant roaming of user settings** | Settings roam across joined devices without requiring a personal Microsoft account (e.g., Hotmail). |
| **Windows Hello support** | Enables secure and convenient passwordless access to work resources. |
| **Compliance-based access control** | Restricts app access to only devices that meet organizational compliance policies. |
| **Seamless on-premises resource access** | When the device has line of sight to the on-premises domain controller, it can access on-premises resources seamlessly. |
 
---
 
### Scenarios Where Microsoft Entra Join Is the Right Choice
 
For organizations with existing on-premises Windows Server Active Directory, Microsoft Entra ID is a strong option in these situations:
 
- **Transitioning to cloud-based infrastructure** with Microsoft Entra ID and an MDM like Intune
- **Cannot use on-premises domain join** (e.g., mobile devices such as tablets and phones)
- **Users primarily need to access Microsoft 365** or other SaaS apps integrated with Microsoft Entra ID
- **Managing a group of users in Microsoft Entra ID** instead of Active Directory (seasonal workers, contractors, students)
- **Remote branch offices** with limited on-premises infrastructure where joining capabilities are needed for workers
---
 
## Unit 3 — Join Devices to Microsoft Entra ID (Step-by-Step)
 
Joining a device to Microsoft Entra ID is straightforward and can be done during initial Windows setup or at any point after installation.
 
### Method: Join During Windows Setup (Out-of-Box Experience / OOBE)
 
Follow these steps when setting up a new Windows device:
 
1. **Power on** the new device. Wait for the **"Getting Ready"** message, then follow the on-screen prompts.
2. **Customize your region and language**, then **accept the Microsoft Software License Terms**.
   <img width="524" height="271" alt="image" src="https://github.com/user-attachments/assets/6b939184-ff6c-4678-a329-c8b36ac1125e" />
4. **Select your network** to connect to the internet.
5. When prompted with *"Who owns this PC?"*, select **"This device belongs to my organization"**.
6. **Enter the credentials** (username and password) provided by your organization, then select **Sign in**.
7. Windows locates the matching tenant in Microsoft Entra ID.
   - If you are in a **federated domain**, you will be redirected to your on-premises **Secure Token Service (STS)** server (e.g., Active Directory Federation Services — AD FS).
   - If you are in a **non-federated domain**, enter your credentials directly on the **Microsoft Entra ID-hosted page**.
8. You will be prompted to complete a **Multifactor Authentication (MFA)** challenge.
9. Microsoft Entra ID checks whether **enrollment in mobile device management (MDM)** is required.
10. Windows **registers the device** in the organization's directory in Microsoft Entra ID and **enrolls it in MDM** (e.g., Intune), if applicable.
11. Upon completion:
    - **Managed users** — Windows takes you to the desktop via automatic sign-in.
    - **Federated users** — You are directed to the Windows sign-in screen to enter your credentials.
> **Tip:** You can also join a device to Microsoft Entra ID **after setup** by going to **Settings → Accounts → Access work or school → Connect** and selecting the option to join a Microsoft Entra ID domain.
 
---
 
## Unit 4 — Manage Devices Joined to Microsoft Entra ID
 
### Management Overview
 
| Management Method | Applicable To | Available for Entra Joined Devices? |
|---|---|---|
| Group Policy | AD DS-joined devices | ❌ Not available (except with Microsoft Entra Domain Services) |
| Microsoft Entra Domain Services (Group Policy) | Limited scenarios | ⚠️ Available but limited; cannot manage smartphones/tablets |
| Microsoft Intune (MDM) | Cloud-joined and registered devices | ✅ Fully supported |
 
> **Key Point:** Microsoft Entra ID does **not** provide a built-in management mechanism for devices that don't support Group Policy. Microsoft Entra Domain Services is **not enabled by default** and must be manually enabled and configured.
 
---
 
### Managing Devices via Microsoft Intune
 
To manage Microsoft Entra joined devices using Intune:
 
1. **Configure Intune as an application in Azure.** This allows each device that joins Microsoft Entra ID to automatically enroll in Intune.
2. **Requirements for automatic MDM enrollment:**
   - An **active Intune subscription** associated with the **same Microsoft Entra tenant** where the integration is configured
   - The user joining the device must have an **assigned Intune license**
3. **After device enrollment in Intune**, you can configure:
   - **Security policies** — Apply security baselines and compliance rules
   - **Configuration policies** — Define device settings and app configurations
> **Important Distinction:** Intune management does **not** follow the same logic as Group Policy management and does **not** have as many available configuration options. Intune management options primarily focus on **security** and the **apps** present on managed devices.
 
---
 
## Tools and Resources Used
 
| Tool / Service | Description |
|---|---|
| **Microsoft Entra ID** (formerly Azure Active Directory) | Cloud-based identity and access management service |
| **Microsoft Intune** | Cloud-based mobile device management (MDM) and mobile application management (MAM) solution |
| **Microsoft Entra Domain Services** | Managed domain services for legacy scenarios; supports Group Policy in limited capacity |
| **Active Directory Federation Services (AD FS)** | On-premises Secure Token Service (STS) for federated authentication |
| **Windows Hello** | Passwordless authentication method for Windows devices |
| **Microsoft 365** | Cloud productivity suite integrated with Microsoft Entra ID for SSO |
| **WS-Fed / WS-Trust** | Federation protocols required by federated tenants for device join and sign-in |
| **Microsoft Workplace Join** | Tool for down-level (non-Windows 10) devices to support hybrid join (available on Microsoft Download Center) |
| **Azure Portal** | Administration portal for configuring Microsoft Entra ID device settings |
 
### Official Source URLs
 
| Page | URL |
|---|---|
| Describe Microsoft Entra Join | https://learn.microsoft.com/en-us/training/modules/administer-device-authentication/2-describe-azure-active-directory-join |
| Prerequisites, Limitations, and Benefits | https://learn.microsoft.com/en-us/training/modules/administer-device-authentication/3-examine-azure-active-directory-join-prerequisites-limitations-benefits |
| Join Devices to Microsoft Entra ID | https://learn.microsoft.com/en-us/training/modules/administer-device-authentication/4-join-devices-azure-active-directory |
| Manage Devices Joined to Microsoft Entra ID | https://learn.microsoft.com/en-us/training/modules/administer-device-authentication/5-manage-devices-joined-azure-active-directory |
 
---
 
## Confirmed Scenarios, Detailed Solutions, and Applicable Sources
 
### Scenario 1: New Employee Device Setup in a Cloud-Only Organization
 
**Problem:** A new hire receives a fresh Windows 11 laptop. The organization uses Microsoft 365 and has no on-premises AD. IT needs the device joined to Microsoft Entra ID during first boot.
 
**Solution:**
1. During OOBE setup, select **"This device belongs to my organization"** when prompted about device ownership.
2. Enter the employee's Microsoft Entra credentials (work email and password).
3. Complete the MFA challenge when prompted.
4. Windows will automatically register the device in Microsoft Entra ID and enroll it in Intune (if configured).
5. The employee lands on the desktop via automatic sign-in.
**Admin prerequisite:** In the Azure Portal → Microsoft Entra ID → **Device Settings**, confirm that users are permitted to join devices. Set the maximum device limit appropriately.
 
**Applicable Sources:**
- https://learn.microsoft.com/en-us/entra/identity/devices/device-join-plan
- https://learn.microsoft.com/en-us/mem/intune/enrollment/windows-enrollment-methods
---
 
### Scenario 2: BYOD (Bring Your Own Device) Enrollment for Remote Contractors
 
**Problem:** Contractors use personal Windows 10 or macOS/iOS/Android devices and need access to corporate Microsoft 365 resources. Full domain join is not appropriate.
 
**Solution:**
- Instead of full Microsoft Entra join, use **Microsoft Entra device registration** (available for all supported platforms).
- Contractors navigate to **Settings → Accounts → Access work or school → Connect** and register their device.
- Once registered, SSO to Microsoft 365 and other Azure-integrated apps is available.
- Intune can apply conditional access and compliance policies to these registered devices.
> **Note:** Registered devices (not joined devices) give SSO to cloud resources but do not place the device under full organizational management.
 
**Applicable Sources:**
- https://learn.microsoft.com/en-us/entra/identity/devices/concept-device-registration
- https://learn.microsoft.com/en-us/mem/intune/user-help/enroll-device-android-company-portal
---
 
### Scenario 3: Hybrid Organization Needing Both Group Policy and Cloud Management
 
**Problem:** An enterprise has Windows 10/11 devices joined to an on-premises AD domain. They want to also benefit from Microsoft Entra ID features (Conditional Access, SSO to cloud apps) without abandoning Group Policy.
 
**Solution:** Configure **Microsoft Entra Hybrid Join**:
1. Ensure **Microsoft Entra Connect** is installed and configured for directory synchronization.
2. In Microsoft Entra Connect, enable the **hybrid join** configuration option.
3. Devices will automatically register with Microsoft Entra ID while remaining domain-joined.
4. Group Policy continues to work from on-premises AD.
5. Cloud-based Conditional Access and SSO now apply to these devices.
> **Warning:** This will NOT work if your single forest synchronizes to more than one Microsoft Entra tenant.
 
**Applicable Sources:**
- https://learn.microsoft.com/en-us/entra/identity/devices/hybrid-join-plan
- https://learn.microsoft.com/en-us/entra/identity/devices/how-to-hybrid-join
---
 
### Scenario 4: Educational Institution Managing Student Devices
 
**Problem:** A university needs to manage student-assigned Windows devices. Students are temporary members; IT doesn't want to create on-premises AD accounts for them. Faculty have traditional on-premises accounts.
 
**Solution:**
- Create student accounts in **Microsoft Entra ID only** (cloud-only accounts).
- During device setup, students join their devices to Microsoft Entra ID using their cloud credentials.
- Enroll devices in **Intune** to push security policies, app deployment, and compliance rules.
- Faculty devices can remain on traditional AD DS or be hybrid-joined.
**Applicable Sources:**
- https://learn.microsoft.com/en-us/education/windows/set-up-students-pcs-to-join-domain
- https://learn.microsoft.com/en-us/mem/intune/fundamentals/deployment-guide-enrollment-windows
---
 
### Scenario 5: Federated Domain — Device Join Failing with Authentication Error
 
**Problem:** Devices in a federated tenant fail to complete the Microsoft Entra join process, showing authentication or sign-in errors.
 
**Root Cause:** The Identity Provider (IdP) does not support **WS-Fed** and/or **WS-Trust username/password endpoint** (versions 1.3 or 2005).
 
**Solution:**
1. Verify your IdP's federation support. Check the IdP documentation for WS-Fed and WS-Trust protocol support.
2. If using **AD FS**, ensure the WS-Trust endpoint is enabled:
   - Open **AD FS Management Console**
   - Go to **Service → Endpoints**
   - Verify `/adfs/services/trust/13/usernamemixed` or `/adfs/services/trust/2005/usernamemixed` is **enabled**
3. If the IdP lacks this support, consider migrating to **Microsoft Entra ID managed authentication** (Password Hash Sync or Pass-Through Authentication) instead of federation.
**Applicable Sources:**
- https://learn.microsoft.com/en-us/entra/identity/devices/troubleshoot-hybrid-join-windows-current
- https://learn.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-user-certificate-authentication
---
 
### Scenario 6: Intune Not Enrolling Devices After Microsoft Entra Join
 
**Problem:** Devices join Microsoft Entra ID successfully but do not enroll in Intune automatically.
 
**Root Cause Options:**
- No active Intune subscription is linked to the Microsoft Entra tenant
- The joining user does not have an assigned Intune license
- MDM auto-enrollment scope is not configured
**Solution:**
1. In the **Azure Portal**, go to **Microsoft Entra ID → Mobility (MDM and MAM) → Microsoft Intune**.
2. Set the **MDM user scope** to **All** (or a specific group containing the users/devices in scope).
3. Confirm users have an **Intune license** assigned (via Microsoft 365 Admin Center or Entra ID Licenses blade).
4. Verify the Intune subscription is associated with the **same tenant** as the Microsoft Entra ID directory.
**Applicable Sources:**
- https://learn.microsoft.com/en-us/mem/intune/enrollment/windows-enroll
- https://learn.microsoft.com/en-us/mem/intune/fundamentals/licenses-assign
---
 
