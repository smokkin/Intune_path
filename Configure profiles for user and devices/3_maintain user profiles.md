# Maintain User Profiles — Windows 10 / Modern Desktop

> IT administrators, Desktop Administrators, Modern Desktop Administrators
 
---
 
## Module Overview
 
This module covers how Windows stores user-specific settings, files, and customizations as **user profiles**, and teaches IT administrators how to manage, minimize, and synchronize those profiles across devices both on-premises and in cloud-based Azure / Microsoft Entra (Azure AD) environments.
 
Topics span the entire lifecycle: understanding what a profile is and how it works, the different profile types available in Windows, techniques to keep profiles small, redirecting known folders to network locations using Group Policy, and synchronizing user settings to the cloud via **Enterprise State Roaming (ESR)**.
 
---
 
## Learning Objectives
 
After completing this module, you will be able to:
 
- Explain the various user profile types that exist in Windows.
- Describe how a user profile works.
- Configure user profiles to conserve space.
- Explain how to deploy and configure Folder Redirection.
- Explain Enterprise State Roaming.
- Configure Enterprise State Roaming for Azure AD (Microsoft Entra ID) devices.
---
 
## Prerequisites
 
- Strong technical skills installing, maintaining, and troubleshooting Windows 10 or later.
- Strong understanding of computer networking, client security, and application concepts.
- Experience using Active Directory Domain Services (AD DS).
---
 
## Section 1 — Examining User Profiles
 
### What Is a User Profile?
 
A **user profile** is a collection of settings, files, and customizations that define an individual's personalized computing environment. The system creates a user profile the first time a user logs on to a computer. At subsequent logons, the system loads the user's profile, and other system components configure the user's environment according to the information stored in that profile.
 
Key behaviors:
- Each user on a shared computer receives their own customized desktop upon logon.
- Settings in one user profile are unique to that user and cannot be accessed by other users.
- Changes to one user's profile do not affect other users.
### Profile Components
 
A user profile consists of two primary elements:
 
| Component | Description |
|---|---|
| **Registry Hive (`NTuser.dat`)** | Loaded at logon and mapped to `HKEY_CURRENT_USER`. Stores registry-based preferences and configurations. |
| **Profile Folders** | A set of folders stored in the file system under the user's profile directory. Contains sub-folders and per-user data such as documents and configuration files. |
 
Windows Explorer uses profile folders extensively for items such as:
- Desktop
- Start Menu
- Documents folder
> **Note:** `NTuser.dat` is held open (locked) for writing while the user is logged on. When the user logs off, the system unloads `HKEY_CURRENT_USER` into `NTuser.dat` and updates it. Pending registry changes are staged in `.regtrans-ms` files and committed at logoff or shutdown.
 
### Profile Location
 
| Windows Version | Default Profile Path |
|---|---|
| Windows Vista and later | `C:\Users\<username>` |
| Windows XP / Windows Server 2003 | `C:\Documents and Settings\<username>` |
| Windows NT 4.0 | `%SystemRoot%\Profiles\<username>` |
 
> **Note:** On Windows Vista and later, a symbolic link named `Documents and Settings` is provided to redirect legacy access attempts to `C:\Users`. Backup software must be aware of this to avoid doubling backup size.
 
Registry path for profile list:
 
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList
```
 
### Profile Logon / Logoff Lifecycle
 
**Local Profile — Logon:**
1. Windows checks `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList` for the cached profile path.
2. `NTuser.dat` is mapped to `HKEY_CURRENT_USER`.
3. The `%userprofile%` environment variable is updated.
**Local Profile — Logoff:**
- Profile is saved to the local hard disk.
**Roaming Profile — Logon:**
1. Path to the roaming profile is retrieved from the user object on the Domain Controller.
2. Windows checks if a profile exists in the roaming path; if not, a folder is created.
3. Windows checks `ProfileList` for a cached copy.
4. `NTuser.dat` is mapped to `HKEY_CURRENT_USER`.
5. `%userprofile%` is updated.
**Roaming Profile — Logoff:**
- Local profile is copied to the path configured by the administrator.
- If a profile already exists on the server, the local profile is **merged** with the server copy.
---
 
## Section 2 — Exploring User Profile Types
 
### Local User Profile
 
- Created the first time a user logs on to a specific computer.
- Stored on the **local hard disk** of that computer.
- Changes are specific to that user **and that computer**.
- Does not follow the user to other machines.
### Roaming User Profile
 
- A copy of the local profile that is **copied to and stored on a network server share**.
- Downloaded to any computer the user logs on to in the domain.
- Changes are synchronized with the server copy when the user logs off.
- Advantages:
  - Users do not need to recreate a profile on each computer.
  - When a computer is replaced, the user's profile information is maintained on the network independently of any individual machine.
**Setting a Roaming Profile (example path):**
 
```
\\SERVER\USER_PROFILES\<username>
```
 
Set the path in **Active Directory Users and Computers** > User Properties > **Profile tab** > **Profile Path** field.
 
> **Warning:** Start menu layouts do **not** roam on Windows 10/Windows Server 2016 and later when users are using more than one PC, Remote Desktop Session Host, or VDI server. Use a specified Start layout or User Profile Disks as a workaround.
 
> **Warning:** When moving to a new Windows version that uses a different profile version (e.g., upgrading to Windows 10 version 1607), users receive a new, empty roaming profile. Minimize impact by using Folder Redirection to redirect common folders.
 
### Mandatory User Profile
 
- A **roaming user profile that has been pre-configured** by an administrator.
- Changes made during a session are **not saved** when the user logs off.
- Only system administrators can make changes.
- Useful for standardized environments such as kiosks and educational settings.
**How to create a mandatory profile:**
1. Rename `NTuser.dat` to `NTuser.man` in the profile folder on the server.
2. The `.man` extension makes the profile **read-only**.
3. The folder name must use the correct OS extension (e.g., `.v6` for Windows 10 version 1607 or later).
**Profile folder extension reference:**
 
| Operating System | Required Extension |
|---|---|
| Windows 10 version 1607 and later | `.v6` |
| Windows 10 version 1511 | `.v5` |
| Windows 8.1 / Windows Server 2012 R2 | `.v4` |
| Windows 8 / Windows Server 2012 | `.v3` |
| Windows Vista / Windows 7 / Windows Server 2008/R2 | `.v2` |
 
**Assigning the profile to a user:**
1. Open **Active Directory Users and Computers** (`dsa.msc`).
2. Navigate to the user account > Properties > **Profile tab**.
3. In the **Profile path** field, enter the path **without** the extension.  
   Example: `\\server\share\profile` (not `\\server\share\profile.v6`).
> **Note:** When a user is configured with a mandatory profile, Windows starts as if it were the user's first sign-in each time they sign in.
 
> **Important:** Unlike earlier versions of Windows, you **cannot** apply a Start and taskbar layout using a mandatory profile on Windows 10 and later.
 
**Fallback behavior:** If the server storing the mandatory profile is unavailable, users can sign in with a locally cached copy, if one exists. Otherwise, they are signed in with a temporary profile.
 
### Temporary User Profile
 
- Issued each time an **error condition** prevents the user's profile from loading.
- Deleted at the end of each session.
- All changes (desktop settings, files) are **lost** when the user logs off.
- Available on Windows 2000 and later.
### Profile Versioning
 
To use roaming profiles across multiple Windows versions, each version maintains a separate profile (indicated by the folder extension above). This means:
- Profiles **double in number** (and storage consumption) when supporting two OS versions.
- Changes made in one OS version **do not roam** to another OS version.
- Inform users of this limitation when managing mixed environments.
---
 
## Section 3 — Minimizing User Profile Size
 
Large user profiles slow down logon/logoff times and consume excessive storage. The following strategies help minimize profile size:
 
| Strategy | Description |
|---|---|
| **Folder Redirection** | Redirect known folders (Documents, Desktop, Pictures, etc.) off the profile to a network share. Profile no longer carries large data files. |
| **Exclude folders from roaming** | Configure Group Policy to exclude specific folders (e.g., `AppData\Local\Temp`) from being copied during roaming profile sync. |
| **Use Offline Files** | Files remain on a network share but are cached locally, keeping them out of the roaming profile. |
| **Enterprise State Roaming** | For Azure AD–joined devices: sync only settings and app data to the cloud rather than the entire profile. |
| **UE-V (User Experience Virtualization)** | On-premises roaming solution for application and Windows settings (alternative to ESR). |
| **OneDrive / Known Folder Move** | Redirect known folders to OneDrive for Business for cloud-based storage. |
| **Profile size quotas** | Enforce disk quotas on profile shares via Group Policy or file server settings. |
 
> **Best practice:** Deploy Folder Redirection **before** enabling Roaming User Profiles to minimize the size of roaming profiles from the start.
 
---
 
## Section 4 — Deploying and Configuring Folder Redirection
 
### What Is Folder Redirection?
 
**Folder Redirection** enables users and administrators to redirect the path of a known folder to a new location manually or through Group Policy. The new location can be:
- A folder on the local computer, **or**
- A directory on a network file share.
Users interact with files in the redirected folder **as if it still existed on the local drive**. For example, the Documents folder can be redirected to a network location, making files available from any domain-joined computer on the network.
 
### Benefits
 
- Reduces logon/logoff time by keeping large data out of roaming profiles.
- Centralizes user data for server-side backup.
- Users can access their files while offline (via **Offline Files** caching).
- Files remain accessible during network or server outages (offline cache).
- Enables the same files to be available across different OS versions (unlike profile-versioned roaming data).
- Transparent to the user — no visible changes in Windows Explorer.
### Prerequisites for Deployment
 
**Administrative:**
- Must be signed in as a member of **Domain Administrators**, **Enterprise Administrators**, or **Group Policy Creator Owners** security group.
**Infrastructure:**
- An Active Directory Domain Services (AD DS) domain with client computers joined to the domain.
- A computer with **Group Policy Management Console (GPMC)** installed.
- A **file server** to host redirected folders.
  - If using DFS Namespaces, DFS folders (links) must have a **single target** to prevent conflicting edits.
**Client:**
- Windows 10, Windows 11, Windows Server 2016 or later.
- x64-based or x86-based processor (**ARM processors are not supported**).
- Joined to the AD DS domain.
**Not supported:**
- Folder Redirection is not supported on PCs powered by ARM processors.
### Deployment Steps
 
**Step 1: Create a Security Group**
 
Create an AD security group containing all users for whom you want to enable Folder Redirection.
 
```powershell
New-ADGroup FolderRedirectionUsers -Path 'OU=Groups,DC=contoso,DC=com' `
    -GroupScope Global -PassThru -Verbose
Add-ADGroupMember -Identity FolderRedirectionUsers -Members user1, user2, user3
```
 
**Step 2: Create a Shared Folder on the File Server**
 
Create and share the root folder. Grant the following permissions:
 
| Principal | NTFS Permission | Share Permission |
|---|---|---|
| CREATOR OWNER | Full Control (subfolders/files only) | — |
| Domain Admins | Full Control | Full Control |
| Everyone | — | Read |
| Users | List Folder / Read Data, Create Folders / Append Data (this folder only) | — |
 
Example UNC path: `\\fileserver\RedirectedFolders`
 
**Step 3: Create a Group Policy Object (GPO)**
 
1. Open **Group Policy Management** (`gpmc.msc`).
2. Right-click the target OU > **Create a GPO in this domain, and Link it here**.
3. Give the GPO a meaningful name (e.g., `Folder Redirection Policy`).
**Step 4: Configure Folder Redirection Settings**
 
1. Right-click the GPO > **Edit**.
2. Navigate to:  
   `User Configuration > Policies > Windows Settings > Folder Redirection`
3. Right-click the folder you want to redirect (e.g., **Documents**) > **Properties**.
4. On the **Target** tab, configure:
   - **Setting:** `Basic – Redirect everyone's folder to the same location`
   - **Target folder location:** `Create a folder for each user under the root path`
   - **Root Path:** `\\fileserver\RedirectedFolders`
5. On the **Settings** tab, configure additional options as needed (see below).
6. Click **OK**.
7. Repeat for each folder you want to redirect.
**Step 5: Enable the GPO**
 
1. In Group Policy Management, right-click the GPO.
2. Select **Link Enabled**.
> **Important:** If you plan to implement **primary computer support**, do so before enabling the GPO to prevent data from being copied to non-primary computers.
 
**Step 6: Apply the Policy**
 
Force a Group Policy refresh on client computers:
 
```cmd
gpupdate /force
```
 
Or wait for the next automatic Group Policy refresh interval.
 
### Folder Redirection Types
 
| Type | Description |
|---|---|
| **Basic** | Redirects everyone's folder to the same root path location. A subfolder is created per user: `\\server\share\<username>\<FolderName>`. |
| **Advanced** | Redirects different users' folders to different locations based on **security group membership**. |
 
### Configurable Settings
 
In the **Settings** tab of a redirected folder's Properties:
 
| Setting | Default | Description |
|---|---|---|
| **Grant the user exclusive rights** | Enabled (recommended) | Administrator and other users cannot access the folder. |
| **Move the contents of \<FolderName\> to the new location** | Optional | Moves all existing data to the network share. May take significant time for large data sets or slow connections. |
| **Apply redirection policy to earlier Windows operating systems** | Optional | Applies the policy to down-level clients. |
| **Redirect the folder back to the local user profile location when the policy is removed** | Configurable | Determines behavior when the GPO is disabled. Enables offline access via Offline Files. |
 
> **Note:** Moving all data can take considerable time depending on connection speed and data volume. You may also notice delays when pinning/unpinning files in remote locations.
 
### Primary Computer Support
 
Primary computers allow you to **limit Folder Redirection and Roaming User Profiles to specific designated computers** per user.
 
- Reduces the risk of private data downloading to shared or untrusted computers (e.g., shared conference room PCs).
- Requires the AD DS schema to support the `msDS-Primary-Computer` attribute.
- Enable via Group Policy:
  - `Redirect folders on primary computers only`
  - `Download roaming profiles on primary computers only`
**Behavior on non-primary computers:**
- Loads the user's cached local profile (if one exists).
- Does not apply Folder Redirection or download roaming profile data.
### Testing Folder Redirection
 
1. Sign in to a domain-joined primary computer using an account configured for Folder Redirection.
2. If needed, run `gpupdate /force` from an elevated command prompt.
3. Open **File Explorer**.
4. Right-click a redirected folder (e.g., Documents) > **Properties**.
5. Select the **Location** tab.
6. Confirm the path displays the **network file share**, not a local path.
### Troubleshooting Folder Redirection
 
**Common Error Events (Application Log):**
 
| Event ID | Source | Description |
|---|---|---|
| `1000` | Userenv | Group Policy client-side extension Folder Redirection returned a failure status. |
| `101` | Folder Redirection | Failed to perform redirection of a folder; new directories could not be created. |
 
**Troubleshooting steps:**
1. Open **Event Viewer** > **Windows Logs** > **Application**.
2. Filter by source **Folder Redirection** or **Userenv**.
3. Check that the target share exists and permissions are correct.
4. Verify the user account has access to the share.
5. Run `gpresult /h report.html` to review applied policy settings.
6. Add file server/domain to the trusted **Local Intranet zone** via Group Policy:
   `Computer Configuration > Administrative Templates > Windows Components > Internet Explorer > Internet Control Panel > Security Page > Site to Zone Assignment List`
   Use format: `\\servername` or `file://servername`.
> **Tip:** Log out and back in after modifying Group Policy settings to trigger the updated Folder Redirection policy.
 
---
 
## Section 5 — Syncing User State with Enterprise State Roaming
 
### What Is Enterprise State Roaming?
 
**Enterprise State Roaming (ESR)** provides users with a unified experience across Windows devices by synchronizing Windows settings and Universal Windows App data to Microsoft's cloud (Azure / Microsoft Entra ID). It reduces the time needed to configure a new device.
 
ESR operates similarly to the consumer settings sync introduced in Windows 8, but is designed for enterprise environments and stores data securely in Azure rather than in consumer Microsoft accounts.
 
Key characteristics:
- Introduced with Windows 10.
- Cloud-based — all synced data is stored in the Microsoft cloud.
- Partitioned by geography: **North America**, **EMEA**, and **APAC**. Data does not replicate across regions.
- Data is hosted in Azure regions that best align with the country/region set in the Microsoft Entra instance.
### What Settings Are Synced?
 
| Category | Examples |
|---|---|
| **Theme settings** | Desktop background, taskbar position, color scheme |
| **Internet Explorer / Edge Legacy settings** | Browsing history, favorites |
| **Passwords** | Internet passwords, Wi-Fi profiles |
| **Language preferences** | Dictionary settings |
| **Ease of Access** | On-screen keyboard, Magnifier settings |
| **Other Windows settings** | Mouse settings, other OS preferences |
| **Universal Windows App data** | App settings written to the roaming folder by UWP app developers |
 
> **Note:** ESR applies to **Microsoft Edge Legacy** (HTML-based, launched with Windows 10 in 2015). It does **not** apply to the newer **Microsoft Edge Chromium-based browser** (released January 2020). See Microsoft Edge Sync documentation for the Chromium-based browser.
 
> **Note:** Win32 (desktop) applications are **not** supported by ESR app data sync. Only Universal Windows Apps (UWP) that explicitly write to the roaming folder benefit from app data sync.
 
### Licensing Requirements
 
Enterprise State Roaming is available to any organization with:
- **Microsoft Entra ID P1 or P2**, or
- **Enterprise Mobility + Security (EMS)** license.
Once ESR is enabled, the organization is granted a **free, limited-use license** to use Azure Rights Management protection in Azure Information Protection.
 
### Data Security
 
- All user data is **encrypted in transit and at rest** in the cloud.
- Sensitive data (such as passwords) is encrypted client-side with keys derived from the Microsoft Entra tenant.
- Prior to November 2022, all data was secured using **Azure Rights Management**. Starting November 2022, Microsoft no longer uses Azure RMS for all data encryption, though sensitive data remains encrypted client-side.
- Before roaming settings leave the computer, they are encrypted using RMS built into Windows 10 — no separate Azure RMS subscription is required.
### Data Retention
 
| Condition | Behavior |
|---|---|
| **Data not accessed for ≥1 year** | Treated as stale; may be deleted. Retention period is subject to change but is never less than 90 days. |
| **User deletes their Azure AD account** | Account roaming data marked for deletion; deleted within 90–180 days. |
| **Admin manually requests deletion** | Admin files a ticket with Azure Support. |
| **Admin turns off ESR for entire directory** | All users stop syncing; all settings data becomes stale and may be deleted after the retention period. |
| **User turns off sync on all devices** | All settings data for that user becomes stale; may be deleted after retention period. |
 
> **Warning:** Deleted data is **not recoverable**. However, settings data is only deleted from Azure — not from the end-user's device. If a device later reconnects to the ESR service, settings will be re-synced and stored in Azure again.
 
---
 
## Section 6 — Configuring Enterprise State Roaming in Azure
 
### Requirements
 
Before enabling ESR, ensure all of the following are met:
 
- Windows 10 version 1511 (OS Build 10586) or later, with the **latest updates** installed.
- Devices must be **Azure AD–joined** or **Hybrid Azure AD–joined**.
- ESR must be **enabled for the tenant** in the Microsoft Entra admin center.
- Users must be assigned an **Azure AD Premium** or **Enterprise Mobility + Security (EMS)** license.
- Devices must be **restarted** after ESR is enabled.
- Users must sign in using an **Azure AD identity**.
**For Hybrid Azure AD–joined devices (on-premises AD):**
- The IT admin must configure **Microsoft Entra hybrid joined devices** (using Azure AD Connect).
- Configure Group Policy to force client devices to automatically sync user data with Azure.
### Enabling ESR in the Microsoft Entra Admin Center
 
1. Sign in to the **Microsoft Entra admin center** as a **Global Administrator**.
2. Browse to **Entra ID** > **Devices** > **Enterprise State Roaming**.
3. Under **Users may sync settings and app data across devices**, choose one of:
   - **All** — enables ESR for all users in the tenant.
   - **Selected** — enables ESR only for a specific security group.
4. Click **Save**.
> **Note:** Global administrators can also limit settings sync to specific security groups and view a per-user device sync status report.
 
### Client-Side Configuration
 
After ESR is enabled in the admin center:
 
1. **Restart** the Windows 10/11 device.
2. The user must **sign in using their Azure AD identity** (primary sign-in account).
3. On the device, verify sync settings:  
   **Settings** > **Accounts** > **Sync your settings**
4. Confirm **Sync settings** is toggled **On**.
5. Verify the same user account is used for both the **logon account** and the **synced account**.
### Sync Rules
 
| Rule | Details |
|---|---|
| **Single account only** | ESR only works for one account at a time (Windows 10 November 2015 or later). If signed in with a work/school Microsoft Entra account, all data syncs via Azure AD. |
| **Primary account** | Windows settings always roam with the primary sign-in account. |
| **App data tagging** | App data is tagged with the account used to acquire the app. Only apps tagged with the primary account sync. |
| **No data mixing** | Data is never mixed between different user accounts on the same device. |
| **Secondary accounts** | Secondary accounts (e.g., personal Microsoft accounts) provide SSO and Store access but **cannot** power settings sync. |
 
### Verifying Sync Status
 
Use the `dsregcmd` tool to check Azure AD registration and ESR status:
 
```cmd
dsregcmd /status
```
 
Look for the `SettingsUrl` field. If it is **empty**, the user logged in before ESR was enabled. Resolution: **restart the device** and log back in.
 
**Event Viewer log location for ESR activity:**
 
```
Event Viewer > Applications and Services Logs > Microsoft > Windows > SettingSync-Azure
```
 
**Checking ESR-related URLs:**  
During ESR sync, Windows 10 accesses Azure endpoints such as `*.one.microsoft.com`. The primary endpoint URL varies based on your Azure subscription region.
 
### Troubleshooting Enterprise State Roaming
 
| Symptom | Possible Cause | Resolution |
|---|---|---|
| **"Some Windows features are only available if you are using a Microsoft account or work account"** | Device is not Azure AD–joined or Hybrid Azure AD–joined. | Verify device join status with `dsregcmd /status`. |
| **Settings not syncing after ESR was enabled** | User authenticated before ESR was turned on; `SettingsUrl` is empty. | Restart the device; have user sign in again. |
| **Multi-Factor Authentication (MFA) causing sync issues** | MFA sign-in using password conflicts with ESR. | Complete MFA via another cloud service (e.g., Microsoft 365) first, or sign in using **Windows Hello** or a **PIN**. |
| **"Sync not available for your account"** | ESR not enabled in Entra admin center, or user lacks required license. | Enable ESR in admin center; verify Azure AD P1/P2 or EMS license assignment. |
| **Multiple Azure AD tenants on one device** | Registry must be updated per tenant. | Retrieve each tenant's GUID from **Entra ID > Overview > Properties > Tenant ID** and update the device registry accordingly. |
 
---
 
## Tools and Resources Used
 
| Tool / Resource | Description | URL |
|---|---|---|
| **Microsoft Entra Admin Center** | Web portal for managing Azure AD tenants, users, devices, and ESR. | https://entra.microsoft.com |
| **Group Policy Management Console (GPMC)** | Windows Server tool for creating, linking, and managing GPOs. | Built into Windows Server |
| **Active Directory Users and Computers (ADUC)** | `dsa.msc` — manage user objects and profile paths in AD DS. | Built into Windows Server |
| **dsregcmd** | Command-line tool to check Azure AD registration status and ESR `SettingsUrl`. | Built into Windows 10/11 |
| **Event Viewer** | Windows tool for reviewing User Profile Service, Folder Redirection, and ESR logs. | Built into Windows |
| **gpupdate** | Forces immediate Group Policy refresh on client computers. | Built into Windows |
| **gpresult** | Generates a Group Policy results report. | Built into Windows |
| **Sysprep / Unattend.xml** | Used to create and capture customized default user profiles. | `C:\Windows\System32\sysprep\sysprep.exe` |
| **Azure AD Connect** | Synchronizes on-premises Active Directory with Azure AD for Hybrid scenarios. | https://learn.microsoft.com/en-us/entra/identity/hybrid/ |
| **Offline Files** | Windows feature that caches network-redirected folder content locally for offline access. | Built into Windows |
| **UE-V (User Experience Virtualization)** | On-premises roaming solution for app and OS settings (alternative to ESR). | https://learn.microsoft.com/en-us/windows/configuration/ue-v/uev-for-windows |
| **OneDrive for Business / Known Folder Move** | Cloud-based alternative to Folder Redirection for redirecting known folders. | https://learn.microsoft.com/en-us/onedrive/redirect-known-folders |
| **Microsoft Learn — Maintain User Profiles** | Primary source module for this document. | https://learn.microsoft.com/en-us/training/modules/maintain-user-profiles/ |
| **Deploy Folder Redirection (Microsoft Docs)** | Official deployment guide for Folder Redirection and Offline Files. | https://learn.microsoft.com/en-us/windows-server/storage/folder-redirection/deploy-folder-redirection |
| **Folder Redirection Overview (Microsoft Docs)** | Overview of Folder Redirection and Roaming User Profiles. | https://learn.microsoft.com/en-us/windows-server/storage/folder-redirection/folder-redirection-rup-overview |
| **Enable Enterprise State Roaming (Microsoft Docs)** | Step-by-step guide for enabling ESR in Microsoft Entra ID. | https://learn.microsoft.com/en-us/entra/identity/devices/enterprise-state-roaming-enable |
| **Enterprise State Roaming FAQ** | Answers to common ESR questions. | https://learn.microsoft.com/en-us/entra/identity/devices/enterprise-state-roaming-faqs |
| **Create Mandatory User Profiles (Microsoft Docs)** | Official guide for creating and assigning mandatory profiles. | https://learn.microsoft.com/en-us/windows/client-management/client-tools/mandatory-user-profile |
| **Deploy Roaming User Profiles (Microsoft Docs)** | Comprehensive deployment guide for roaming profiles. | https://learn.microsoft.com/en-us/windows-server/storage/folder-redirection/deploy-roaming-user-profiles |
| **Troubleshoot User Profiles with Events** | Guide for using Event Viewer to diagnose profile load/unload issues. | https://learn.microsoft.com/en-us/troubleshoot/windows-server/user-profiles-and-logon/troubleshoot-user-profiles-events |
 
---
