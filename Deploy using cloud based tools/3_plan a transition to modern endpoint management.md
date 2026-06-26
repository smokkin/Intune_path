# Plan the Transition to Modern Endpoint Management

---

## Overview

Moving to modern endpoint management can be a challenging task given the complexity of transitioning from existing IT systems, organizational structures, and processes. Most organizations still use some combination of on-premises Windows Server Active Directory (AD) and Configuration Manager to manage their Windows devices.

Microsoft addresses this challenge through a combination of **Co-management**, **Microsoft Intune**, **Microsoft Entra ID (formerly Azure AD)**, and **Windows Autopilot** together enabling a phased, reversible path toward fully cloud-based device management.

---

## Unit 2 — Explore Co-Management for the Transition

> **Source:** [Unit 2](https://learn.microsoft.com/en-us/training/modules/plan-transition-modern-endpoint-management/2-explore-co-management-transition)

**Co-management** is a Microsoft-designed feature that bridges on-premises management (Configuration Manager + AD) with cloud-based management (Microsoft Intune + Microsoft Entra ID).

### Microsoft Entra Hybrid Join

If you have an on-premises Active Directory environment and want to co-manage domain-joined devices, you configure them as **Microsoft Entra hybrid joined devices**. This approach:

- Maximizes user productivity through **Single Sign-On (SSO)** across cloud and on-premises resources.
- Secures access using **Conditional Access** enabling automated access control decisions for services such as SharePoint Online or Exchange Online, based on conditions.

### Intune vs. Group Policy

| Feature | Microsoft Intune | Group Policy |
|---|---|---|
| Settings granularity | Broader privacy, security, and app management | Fine-grained individual settings |
| On-premises dependency | Not required | Requires on-premises domain-joined devices |
| Internet-connected device support | Yes — ideal for mobile/remote devices | No |

> **Key insight:** Intune is the best choice for devices constantly on the go because it can target internet-connected devices without requiring on-premises domain membership.

### Phased Rollout Strategy

Once co-management is configured:

1. **Create pilot groups** of users and devices.
2. **Start small** — begin with IT department users and devices.
3. **Validate** — confirm it works as expected.
4. **Expand gradually** to the rest of the environment.

---

## Unit 3 — Prerequisites for Co-Management

> **Source:** [Unit 3](https://learn.microsoft.com/en-us/training/modules/plan-transition-modern-endpoint-management/3-examine-prerequisites-for-co-management)

To enable co-management for on-premises Active Directory devices, you must configure them as **Microsoft Entra hybrid joined devices**.

### Required Prerequisites

Before enabling Microsoft Entra hybrid join, ensure the following are in place:

-  **Microsoft Entra Connect** is running an up-to-date version and has synchronized computer objects to Microsoft Entra ID.
  - If computer objects belong to specific Organizational Units (OUs), those OUs must be configured for synchronization in Microsoft Entra Connect.
- **Intune MDM** is set up and configured for automatic enrollment.
- **Microsoft Intune** is installed.
- Active Directory joined devices are running **Windows 10 version 1709 or later** (latest version recommended).
- **Microsoft Entra automatic enrollment** is enabled.

> **Note:** Microsoft recommends always using the latest version of Windows to benefit from the newest advances in security, Microsoft Entra ID, and Intune features.

### Controlling Automatic Device Registration

Microsoft Entra hybrid join is designed to facilitate automatic registration of on-premises domain-joined devices with Microsoft Entra ID. However, during a pilot phase, you may want to control which devices register automatically.

**Behavior:** All Windows current devices automatically register with Microsoft Entra ID at device start or user sign-in.

You can control this behavior using a **Group Policy Object (GPO)** or **Configuration Manager**.

#### GPO Steps to Control Registration

The GPO to deploy is: **Register domain-joined computers as devices**

1. In the **Group Policy Management Console**, create **two new GPOs**, then navigate to:
   ```
   Computer Configuration > Policies > Administrative Templates > 
   Windows Components > Device Registration
   ```
2. **First GPO:** Apply the `Disabled` setting → prevents automatic device registration.
3. **Second GPO:** Apply the `Enabled` setting → enables automatic device registration.
4. Link the **first GPO** to all devices in your environment.
5. Link the **second GPO** only to the OU containing your pilot devices.

> **Alternative:** Use Group Policy security filtering and a security group to control which devices can automatically register with Microsoft Entra ID.

### Immediate Intune Remote Actions After Joining

After joining on-premises Active Directory devices to Microsoft Entra ID, the following **Intune remote actions** become immediately available:

- Factory reset
- Selective wipe
- Delete devices
- Restart device
- Fresh start

---

### Transition Workloads to Intune

Once Intune is configured and Windows devices are prepared for co-management, the next step is determining which workloads to transition to Intune.

>  **Important:** Before switching a workload, ensure the corresponding workload has been appropriately configured and deployed within Intune. Failing to do so may result in unmanaged states for devices.

#### Workloads Available to Transition

**Resource Access Policies**
- Email profile
- Wi-Fi profile
- VPN profile
- Certificate profile

**Windows Update Policies**

**Endpoint Protection**
- Microsoft Defender Antivirus
- Microsoft Defender Application Guard
- Windows Defender Firewall
- Microsoft Defender SmartScreen
- Windows Encryption
- Microsoft Defender Exploit Guard
- Windows Defender Application Control
- Microsoft Defender for Endpoint
- Windows Information Protection

**Device Configuration**
- Essentially the settings you configure using Group Policy.

**Microsoft 365 Click-to-Run Apps**
- Once the workload has been moved, the app will appear in the **Company Portal** on the device.

#### Recommended Approach

> Prioritize devices with simple configuration settings and migrate those workloads to Intune first. This includes:
> - Endpoint Protection
> - Windows Update policies
> - Software deployment
> - Device configuration policies that align with existing Group Policy settings

---

## Unit 4 — Modern Management Considerations

> **Source:** [Unit 4](https://learn.microsoft.com/en-us/training/modules/plan-transition-modern-endpoint-management/4-evaluate-modern-management-considerations)

Modern deployment methods take a different approach to OS deployment they **remove the imaging process** wherever possible. Instead of wiping existing data and replacing it, these options transform the existing OS with little or no user interaction.

### Benefits of Modern Methods Over Traditional Imaging

- Typically faster and more efficient
- Lower network utilization
- Minimal user interaction required
- No images involved in the process

### Device Requirements for Modern Methods

| Device State | Recommended Method |
|---|---|
| Windows 11 installed | Modern methods (preferred) |
| Windows 7 or 8.1 | In-place upgrade |
| No OS or different OS | Traditional methods (required) |

### Capabilities of Modern Deployment

Modern deployment can transform an installed Windows 11 OS in many ways:

- Removing preinstalled software
- Upgrading a Windows 11 edition
- Joining a device to AD DS or Microsoft Entra ID
- Enrolling a device in an MDM solution (Configuration Manager or Intune)
- Restricting the Administrator account creation
- Creating and auto-assigning devices to configuration groups based on device profile
- Customizing OOBE (Out-of-Box Experience) content specific to the organization

---

### Modern Transition Comparison Table

| Capability | MDT | Configuration Manager | Windows Autopilot |
|---|---|---|---|
| Require creation of golden images | Yes | Yes | **No** |
| Ability to reset existing OS | No | No | **Yes** |
| Ability to perform bare metal builds | Yes | Yes | No |
| Can be used with any preinstalled OS | Yes | Yes | Windows 10/11 only |
| Install applications during OS deployment | Yes | Yes | Yes |
| Deploy applications post-build | No | Yes | Yes |
| Migration of user data | Yes — USMT | Yes — USMT | Yes — OneDrive/ESR |
| Perform an in-place upgrade | Yes | Yes | Yes (with Configuration Manager) |

### Transition Paths

After evaluating requirements, there are two transition paths:

1. **Migration of existing technologies** — retaining the benefits of the current setup.
2. **Adoption of modern technologies from the outset** — a clean break to cloud-native management.

---

### When Traditional Imaging Is Still Needed

While modern methods are preferred, traditional imaging remains necessary in some scenarios:

- A device encounters a **Blue Screen of Death (BSOD)** and fails to boot, requiring a fresh installation.
- Deploying **complex applications and their dependencies** to a co-managed device.
- **Hardware failures** requiring network connectivity for application installation or domain joining.

These scenarios are common when:
- Replacing client storage drives
- Conducting bare-metal deployments
- Handling devices that cannot be upgraded to the target OS

---

## Unit 5 — Evaluate Upgrades and Migrations

> **Source:** [Unit 5](https://learn.microsoft.com/en-us/training/modules/plan-transition-modern-endpoint-management/5-evaluate-upgrades-migrations-modern-transitioning)

Device migration is similar to deploying a new device. The key difference is **consideration for the end user's data and configuration** the process must safely ensure that user data and settings are not lost.

### Migration Scenarios

Common reasons to migrate a user's device:

- The user's device is **being replaced**.
- The existing device is being upgraded from an older OS to Windows 11 and an in-place upgrade **is not possible** (unsupported upgrade path).
- A **clean installation is needed** due to performance/stability issues, legacy apps, or non-standard configuration.

### Migration Types

| Type | Description |
|---|---|
| **Side-by-side (Replace)** | Source and destination computers are **different** |
| **Wipe-and-load (Refresh)** | Source and destination computers are the **same** |

Both scenarios require a clean installation of Windows 11.

> **Warning:** Careful consideration should be given when choosing migration over upgrade. If IT or the user doesn't properly identify all data to be migrated, data loss is possible. A migration **cannot be rolled back** if something goes wrong during deployment.

---

### In-Place Upgrade vs. Migration Comparison

| In-Place Upgrade | Migration |
|---|---|
| Preserves the environment | Provides a standardized environment |
| No need to reinstall apps or transfer data | You can control what migrates |
| Can be rolled back if needed | Cleans up the environment |
| Only certain upgrade paths are possible | You must reinstall apps |
| Must use the default Windows image | Can use a custom Windows image |

---

### In-Place Upgrades with Autopilot

**Windows Autopilot** helps deploy the latest version of Windows to existing devices. This approach can:

- Transform a traditional domain-joined endpoint into a **Microsoft Entra ID managed device**.
- Perform a rebuild all within the same piece of automation.
- Work with legacy devices (Windows 7/8.1) when combined with a task sequence.

> **Note:** You can also build devices fresh using Autopilot from a vanilla Windows 11 PC.

**Co-management** bridges workloads between Intune and Configuration Manager, offering:

- A path to **full modern management** (workload moved entirely to Intune).
- A **gradual transition** path trial workloads in Intune while production clients run on Configuration Manager.

> **Tip:** A common practice is to set up **Autopilot groups** to test co-management workloads. For example, move a group of devices managed by Configuration Manager for software updates to Intune, which manages them using a **Windows Update for Business (WuFB)** policy.

---

## Unit 6 — Migrate Data During Modern Transitioning

> **Source:** [Unit 6](https://learn.microsoft.com/en-us/training/modules/plan-transition-modern-endpoint-management/6-migrate-data-when-modern-transitioning)

When moving a user to a new device or performing an in-place migration, it is critical to determine which user and app settings should be preserved and how to ensure that data is saved.

---

### Enterprise State Roaming (ESR)

Starting with Windows 10, **Enterprise State Roaming (ESR)** allows Microsoft Entra users to securely synchronize user settings and application settings data to the cloud. ESR operates similarly to the consumer settings sync introduced in Windows 8.

#### ESR Benefits

| Benefit | Description |
|---|---|
| **Separation of corporate and consumer data** | Organizations have complete control — no commingling of corporate and consumer data |
| **Enhanced security** | Data is encrypted before leaving the device using **Azure Rights Management (Azure RMS)**; data stays encrypted at rest in the cloud |
| **Better management and monitoring** | Visibility over who syncs settings and on which devices via the Microsoft Entra admin center |

#### What ESR Synchronizes

When ESR is enabled, users signing in to a new device automatically retain:

- Microsoft Edge browser settings
- Personalized configurations
- Passwords
- Language preferences
- Mouse settings
- Certain UWP apps (if supported by the developer)

#### ESR Limitations

> ESR **does not synchronize settings for desktop (Win32) applications**. The impact depends on the application — data in AppData may be generated automatically by the app and may not be critical user data.

**General guideline:** Migrate all settings and data users require; avoid migrating unnecessary or outdated data. Always consider the effort to migrate specific data versus the time saved.

---

### User State Migration Tool (USMT)

When synchronization solutions aren't sufficient and there's a need to migrate files or settings between devices, the **User State Migration Tool (USMT)** is the standard solution. USMT operates in two phases:

#### Phase 1 — Capture Settings

- Capture settings and data from the **source computer**.
- Store them in a **migration store** (typically a shared network folder, or local storage).

#### Phase 2 — Restore Captured Settings

- After installing an OS on the **destination computer**, restore the captured settings.
- This phase can occur separately from or during the OS installation.

> **Note:** You can capture data without restoring all of it. However, you **cannot restore data that was not captured first** it simply won't exist in the migration store.

### User State Migration in Replace and Refresh Scenarios

| Scenario | Source / Destination | Process |
|---|---|---|
| **Replace (Side-by-side)** | Different computers | Capture user state from source → Deploy Windows on destination → Restore user state |
| **Refresh (Wipe-and-load)** | Same computer | Capture user state → Store temporarily → Clean Windows install → Restore user state |

#### The Windows.old Folder

When deploying Windows on a computer with an existing supported Windows OS, Windows creates a **Windows.old folder**. Key points:

- The previous Windows installation folder, Program Files folder, and Users folder are moved to `Windows.old`.
- User data in the root folder remains unchanged.
- You can capture user state **online** (while old Windows is running) or **offline** (from the `Windows.old` folder).

---

### OneDrive Known Folder Move (KFM)

**OneDrive Known Folder Move (KFM)** enables IT to redirect commonly used folders **Desktop**, **Pictures**, and **Documents** — to OneDrive, facilitating a modern file management solution.

#### How to Enable KFM via Group Policy

1. Install the Group Policy templates located at:
   ```
   %localappdata%\Microsoft\OneDrive\[BuildNumber]\adm
   ```
2. Enable the policy: **Prompt users to move Windows known folders to OneDrive**
3. Configure it with your **tenant ID** (found in the Microsoft Entra admin center).

#### Silent KFM (No User Interaction)

To migrate folders without prompting users:

- Enable the **Silently move Windows known folders to OneDrive** Group Policy.
- Configure it with your **tenant ID**.
- Optionally, configure whether to show a notification when folders have been redirected.

#### KFM Considerations and Warnings

> **Warning:** You **cannot use KFM** if you are already using **Windows Folder Redirection** for Desktop, Pictures, or Documents folders.

Additional considerations:

- **Large organizations:** Configure the sync client upload rate GPO and consider a pilot group to assess potential network traffic impact.
- **KFM errors** will occur under these unsupported conditions:
  - Unsupported file types (e.g., Outlook or OneNote files)
  - Exceeding maximum path length
  - Known folders not in the default locations

> KFM complements ESR and allows management of Win32 application settings without legacy roaming profiles.

---

### Integrating USMT with Configuration Manager

You can utilize USMT within Configuration Manager to migrate settings between devices or retain settings during a rebuild (also known as a **Hardlink migration store**).

#### Prerequisite

After setting up the Configuration Manager site, install the **User State Migration** components from the **Windows Assessment and Deployment Kit (ADK)**.

#### Create a USMT Package from Configuration Manager

After installing prerequisites, create a custom USMT package or use the default package.

The default package is located at:
```
%Program files%\Windows Kits\10\Assessment and Deployment Kit\User State Migration
```

> **Best practice:** Copy the default package and replicate it with changes specific to your use scenario.

#### State Migration Point (Configuration Manager Site System Role)

To support multiple migrations during an upgrade, set up a **State Migration Point** (Configuration Manager Role) as a file share to store data.

Key details:
- Each request stores a unique hash identifying the device.
- Enables data to be **captured**, the device **upgraded**, and data **reinstated** afterward.
- For large enterprises, it is common to have **regionally located State Migration Points** within the site hierarchy.

#### Task Sequence Integration

Task sequences manage the entire OS deployment process and can incorporate USMT:

- **USMT is used at the initial stage** to capture user settings.
- **Used as a post-build step** to restore settings for specific users.
- The USMT package (created during site setup) is referenced within task sequence steps.

#### USMT XML Templates

USMT uses four XML template files that control what data is collected:

| File | Purpose |
|---|---|
| `MigApp.xml` | Controls application data migration |
| `MigDocs.xml` | Controls document migration |
| `MigUser.xml` | Controls user profile migration |
| `ConfigMgr.xml` | Configuration Manager-specific migration controls |

These files are executed within either the **Scan** or **Load** module and are referenced in the Configuration Manager task sequence step.

> **Customization:** Create custom versions of these XML files to specify exactly which data to collect while adhering to the XML template structure.

---

## Unit 7 — Migrate Workloads During Modern Transitioning

> **Source:** [Unit 7](https://learn.microsoft.com/en-us/training/modules/plan-transition-modern-endpoint-management/7-migrate-workloads-when-modern-transitioning)

For organizations currently using Configuration Manager, there is no single "correct" approach to transitioning to modern management. Organizations must choose which solution within **Microsoft Endpoint Manager** best fits their specific scenarios. Microsoft fully supports Endpoint Manager — including all capabilities that Intune and Configuration Manager provide.

---

### Migrate Client Management to Intune

When an organization has completely moved client devices away from legacy OS versions, a shift to **100% cloud-based management** (Intune only) may be appropriate.

> Configuration Manager requires expertise to maintain on-premises infrastructure. Eliminating that layer of complexity can simplify administration efforts.

#### When Should Smaller Organizations Consider 100% Cloud-Based Management?

Consider this path if:

- Autopilot's OS configuration capabilities meet deployment needs.
- Applications are modern and relatively simple to install.
- There are no excessive legacy applications.
- The existing configuration management deployment is relatively simple.
- Server management requirements can be met with tools such as **Windows Admin Center**.

---

### Choose Workloads Within Endpoint Manager

Larger organizations may want to continue using Configuration Manager and leverage co-management within Endpoint Manager. This is typically because:

- Enterprise organizations tend to have more complex infrastructure, legacy systems, and requirements that Configuration Manager handles best.
- Organizations that have used Configuration Manager for years have a **significant time investment** in existing configurations (e.g., hundreds of application packages, extensive settings and compliance policies).

> Customers **should not arbitrarily migrate workloads** unless there is a clear value to be gained by shifting them.

#### Where Migration Delivers Clear Value

**OS Deployment** is a common high-value migration area:

- Image management is typically an ongoing, tedious, and time-consuming effort.
- As most new devices ship with Windows 10 or Windows 11, organizations frequently see **immediate returns** by adopting **Autopilot** to replace image deployment.

**Application deployment split:**
- Modern/new applications (e.g., Microsoft 365/Office) → deploy through **Intune** (typically easier).
- Existing and complex applications → continue deploying with **Configuration Manager**.

> The **Endpoint Manager Admin Console** helps unify management of the device while still providing a choice of which underlying technology to use for delivery.

---

## Confirmed Scenarios, Detailed Solutions, and Applicable Sources

The following scenarios are directly derived from the module content. Each includes a detailed solution and applicable sources.

---

### Scenario 1 — Enabling Co-Management for Existing On-Premises AD Devices

**Situation:** Your organization has Windows 10/11 domain-joined devices managed by Configuration Manager and wants to begin co-management with Intune.

**Detailed Solution:**

1. Update Microsoft Entra Connect and verify computer objects are synchronized to Microsoft Entra ID.
2. Configure OUs for synchronization in Microsoft Entra Connect if devices are in specific OUs.
3. Enable **Microsoft Entra automatic enrollment** in Intune.
4. Configure MDM Authority in Intune to enable automatic enrollment.
5. Create two GPOs for **Register domain-joined computers as devices**:
   - GPO 1: `Disabled` → link to all devices (blocks auto-registration).
   - GPO 2: `Enabled` → link to pilot OU only (allows auto-registration for pilots).
6. Monitor pilot devices in the Endpoint Manager admin center.
7. Gradually expand the pilot group to the full environment.

**Applicable Sources:**
- [Examine prerequisites for co-management](https://learn.microsoft.com/en-us/training/modules/plan-transition-modern-endpoint-management/3-examine-prerequisites-for-co-management)
- [Configure Microsoft Entra hybrid join](https://learn.microsoft.com/en-us/azure/active-directory/devices/hybrid-azuread-join-plan)
- [Set up co-management — Microsoft Docs](https://learn.microsoft.com/en-us/mem/configmgr/comanage/how-to-enable)

---

### Scenario 2 — Transitioning a Workload (Endpoint Protection) from Configuration Manager to Intune

**Situation:** Your organization co-manages devices and wants to move Microsoft Defender Antivirus management from Configuration Manager to Intune.

**Detailed Solution:**

1. In the **Endpoint Manager Admin Console**, verify the Endpoint Protection workload configuration.
2. Create and deploy an **Endpoint Protection policy** in Intune that covers Microsoft Defender Antivirus settings equivalent to current Configuration Manager settings.
3. Test the Intune policy on a pilot device collection.
4. In Configuration Manager, navigate to **Co-management Properties** → **Workloads** tab.
5. Slide the **Endpoint Protection** workload to **Pilot Intune** or **Intune**.
6. Monitor the pilot group; validate that Defender Antivirus is receiving policies from Intune.
7. Promote to full Intune control once verified.

**Applicable Sources:**
- [Examine prerequisites for co-management (Workloads)](https://learn.microsoft.com/en-us/training/modules/plan-transition-modern-endpoint-management/3-examine-prerequisites-for-co-management)
- [Switch co-management workloads](https://learn.microsoft.com/en-us/mem/configmgr/comanage/how-to-switch-workloads)
- [Endpoint security in Intune](https://learn.microsoft.com/en-us/mem/intune/protect/endpoint-security)

---

### Scenario 3 — Deploying Windows 11 to New Devices Using Autopilot (No Imaging)

**Situation:** Your organization receives new devices pre-installed with Windows 11 and wants to provision them without creating or deploying a traditional OS image.

**Detailed Solution:**

1. Register device hardware IDs with your Microsoft Entra tenant (via vendor or manually via PowerShell/CSV upload in Intune).
2. Create an **Autopilot deployment profile** in Intune (Devices → Enroll devices → Windows Autopilot).
3. Configure the OOBE experience (skip privacy settings, skip EULA, etc.) as appropriate.
4. Create a dynamic **Microsoft Entra group** to auto-assign profiles to Autopilot devices.
5. Assign applications and configuration profiles to the Autopilot device group in Intune.
6. Power on the new device → it connects to the internet → Autopilot profile is applied → device is provisioned automatically.
7. User signs in with their organizational credentials — device is ready.

**Applicable Sources:**
- [Evaluate modern management considerations](https://learn.microsoft.com/en-us/training/modules/plan-transition-modern-endpoint-management/4-evaluate-modern-management-considerations)
- [Windows Autopilot overview](https://learn.microsoft.com/en-us/mem/autopilot/windows-autopilot)
- [Windows Autopilot deployment scenarios](https://learn.microsoft.com/en-us/mem/autopilot/deployment-scenarios)

---

### Scenario 4 — Migrating User Data Using USMT in a Side-by-Side (Replace) Scenario

**Situation:** A user's old PC is being replaced with a new device. You need to capture user data from the old device and restore it to the new one.

**Detailed Solution:**

1. Set up a **State Migration Point** in Configuration Manager as a network store.
2. Create a **USMT package** in Configuration Manager (or use the default at `%Program files%\Windows Kits\10\Assessment and Deployment Kit\User State Migration`).
3. Create a **task sequence** for the source device that runs `scanstate.exe` to capture user data:
   ```
   scanstate.exe \\migration-store\%computername% /i:MigApp.xml /i:MigDocs.xml /i:MigUser.xml /o /c /efs:copyraw
   ```
4. Data is stored in the migration store with a unique hash tied to the source device.
5. On the destination (new) device, run the Windows 11 deployment task sequence.
6. At the post-build stage, run `loadstate.exe` to restore captured data:
   ```
   loadstate.exe \\migration-store\%computername% /i:MigApp.xml /i:MigDocs.xml /i:MigUser.xml /c /lac /lae
   ```
7. Verify data integrity and user profile settings on the new device.

**Applicable Sources:**
- [Migrate data when modern transitioning](https://learn.microsoft.com/en-us/training/modules/plan-transition-modern-endpoint-management/6-migrate-data-when-modern-transitioning)
- [USMT overview](https://learn.microsoft.com/en-us/windows/deployment/usmt/usmt-overview)
- [Manage user state in Configuration Manager](https://learn.microsoft.com/en-us/mem/configmgr/osd/get-started/manage-user-state)

---

### Scenario 5 — Enabling OneDrive Known Folder Move (KFM) for Seamless Data Protection

**Situation:** Your organization wants to automatically protect users' Desktop, Documents, and Pictures folders in OneDrive without user intervention.

**Detailed Solution:**

1. Download and install the OneDrive sync client on a reference device.
2. Navigate to the Group Policy templates location:
   ```
   %localappdata%\Microsoft\OneDrive\[BuildNumber]\adm
   ```
3. Copy the `.admx` and `.adml` files to your Group Policy Central Store:
   ```
   \\[SYSVOL]\Policies\PolicyDefinitions\
   ```
4. Retrieve your tenant ID from the **Microsoft Entra admin center** (Azure Active Directory → Properties).
5. Create and configure a GPO with the policy: **Silently move Windows known folders to OneDrive**
   - Set Tenant ID = `<your-tenant-id>`
   - Optionally enable notifications to users.
6. Link the GPO to the appropriate OU.
7. Users will have their Desktop, Documents, and Pictures automatically redirected to OneDrive on next sign-in.

> **Check before deploying:** Ensure no existing Windows Folder Redirection policy is configured for Desktop, Pictures, or Documents.

**Applicable Sources:**
- [Migrate data when modern transitioning](https://learn.microsoft.com/en-us/training/modules/plan-transition-modern-endpoint-management/6-migrate-data-when-modern-transitioning)
- [OneDrive Known Folder Move](https://learn.microsoft.com/en-us/onedrive/redirect-known-folders)
- [KFM Group Policy settings](https://learn.microsoft.com/en-us/onedrive/use-group-policy)

---

### Scenario 6 — Deciding Whether to Use Intune Only vs. Co-Management (Intune + ConfigMgr)

**Situation:** An organization is assessing whether to move entirely to Intune or continue with co-management.

**Detailed Solution:**

| Factor | Move to Intune Only | Continue Co-Management |
|---|---|---|
| Organization size | Small to mid-sized | Large enterprise |
| Legacy applications | Few/none | Many complex legacy apps |
| Current ConfigMgr investment | Simple/small | Large, complex, years invested |
| Deployment needs | Autopilot-compatible | Complex bare-metal or imaging needs |
| Server management | Windows Admin Center sufficient | Requires full ConfigMgr capabilities |

**Steps for a Hybrid Decision:**

1. Inventory all existing Configuration Manager application packages and policies.
2. Identify workloads where Intune provides clear value (e.g., OS deployment via Autopilot, Microsoft 365 app deployment).
3. Migrate high-value, low-risk workloads to Intune first.
4. Leave complex, deeply integrated workloads in Configuration Manager.
5. Use the **Endpoint Manager Admin Console** to manage both from a single interface.
6. Reassess the co-management split annually as legacy systems are retired.

**Applicable Sources:**
- [Migrate workloads when modern transitioning](https://learn.microsoft.com/en-us/training/modules/plan-transition-modern-endpoint-management/7-migrate-workloads-when-modern-transitioning)
- [Co-management overview](https://learn.microsoft.com/en-us/mem/configmgr/comanage/overview)
- [Endpoint Manager overview](https://learn.microsoft.com/en-us/mem/endpoint-manager-overview)

---

## Tools and Resources Used

| Tool / Technology | Description | Reference |
|---|---|---|
| **Microsoft Intune** | Cloud-based MDM and MAM solution | [learn.microsoft.com/mem/intune](https://learn.microsoft.com/en-us/mem/intune/) |
| **Microsoft Entra ID** (Azure AD) | Cloud identity and access management | [learn.microsoft.com/azure/active-directory](https://learn.microsoft.com/en-us/azure/active-directory/) |
| **Microsoft Entra Connect** | Synchronizes on-premises AD objects to Entra ID | [learn.microsoft.com/azure/active-directory/hybrid](https://learn.microsoft.com/en-us/azure/active-directory/hybrid/) |
| **Microsoft Configuration Manager (ConfigMgr)** | On-premises endpoint management | [learn.microsoft.com/mem/configmgr](https://learn.microsoft.com/en-us/mem/configmgr/) |
| **Windows Autopilot** | Modern device provisioning and deployment | [learn.microsoft.com/mem/autopilot](https://learn.microsoft.com/en-us/mem/autopilot/) |
| **Microsoft Endpoint Manager (MEM)** | Unified console for Intune + ConfigMgr | [learn.microsoft.com/mem/endpoint-manager-overview](https://learn.microsoft.com/en-us/mem/endpoint-manager-overview) |
| **Enterprise State Roaming (ESR)** | Cloud sync for user settings across Entra ID devices | [learn.microsoft.com/azure/active-directory/devices/enterprise-state-roaming-overview](https://learn.microsoft.com/en-us/azure/active-directory/devices/enterprise-state-roaming-overview) |
| **User State Migration Tool (USMT)** | Migrate user profiles, files, and settings | [learn.microsoft.com/windows/deployment/usmt](https://learn.microsoft.com/en-us/windows/deployment/usmt/usmt-overview) |
| **OneDrive Known Folder Move (KFM)** | Redirect Desktop/Documents/Pictures to OneDrive | [learn.microsoft.com/onedrive/redirect-known-folders](https://learn.microsoft.com/en-us/onedrive/redirect-known-folders) |
| **Azure Rights Management (Azure RMS)** | Encryption service used by ESR for data in transit | [learn.microsoft.com/azure/information-protection](https://learn.microsoft.com/en-us/azure/information-protection/) |
| **Microsoft Deployment Toolkit (MDT)** | Traditional OS deployment toolkit | [learn.microsoft.com/windows/deployment/deploy-windows-mdt](https://learn.microsoft.com/en-us/windows/deployment/deploy-windows-mdt/get-started-with-the-microsoft-deployment-toolkit) |
| **Windows Update for Business (WuFB)** | Cloud-based update management for Windows | [learn.microsoft.com/windows/deployment/update/waas-manage-updates-wufb](https://learn.microsoft.com/en-us/windows/deployment/update/waas-manage-updates-wufb) |
| **Windows Admin Center** | Browser-based tool for Windows server/client management | [learn.microsoft.com/windows-server/manage/windows-admin-center](https://learn.microsoft.com/en-us/windows-server/manage/windows-admin-center/overview) |
| **Windows Assessment and Deployment Kit (ADK)** | Deployment and assessment tools including USMT | [learn.microsoft.com/windows-hardware/get-started/adk-install](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install) |
| **Group Policy Management Console (GPMC)** | Tool for managing Group Policy Objects | Built into Windows Server / RSAT |
| **Conditional Access (Microsoft Entra)** | Automated access control decisions based on conditions | [learn.microsoft.com/azure/active-directory/conditional-access](https://learn.microsoft.com/en-us/azure/active-directory/conditional-access/) |
| **Company Portal** | Intune app for end-user self-service and app access | [learn.microsoft.com/mem/intune/apps/company-portal-app](https://learn.microsoft.com/en-us/mem/intune/apps/company-portal-app) |

---
