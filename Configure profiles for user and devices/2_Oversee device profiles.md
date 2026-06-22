# Oversee Device Profiles in Microsoft Intune
 
> **Audience:** IT Administrators managing devices with Microsoft Intune
 
This document consolidates three units from the Microsoft Learn training module on overseeing device profiles in Intune:
 
1. [Monitor Device Profiles in Intune](#1-monitor-device-profiles-in-intune)
2. [Manage Device Sync in Intune](#2-manage-device-sync-in-intune)
3. [Manage Devices in Intune Using Scripts](#3-manage-devices-in-intune-using-scripts)
---
 
## Table of Contents
 
- [1. Monitor Device Profiles in Intune](#1-monitor-device-profiles-in-intune)
  - [View Existing Profiles](#view-existing-profiles)
  - [View Details on a Profile](#view-details-on-a-profile)
  - [View Conflicts](#view-conflicts)
- [2. Manage Device Sync in Intune](#2-manage-device-sync-in-intune)
  - [Sync a Device](#sync-a-device)
  - [Intune Policy Categories](#intune-policy-categories)
  - [Policy Check-in Frequency](#policy-check-in-frequency)
- [3. Manage Devices in Intune Using Scripts](#3-manage-devices-in-intune-using-scripts)
  - [Prerequisites](#prerequisites)
  - [Create a PowerShell Script Policy for Windows](#create-a-powershell-script-policy-for-windows)
  - [Create a Shell Script Policy for macOS](#create-a-shell-script-policy-for-macos)
- [Tools and Resources Used](#tools-and-resources-used)
- [Source Notes](#source-notes)
- [GitHub README Usage](#github-readme-usage)
---
 
## 1. Monitor Device Profiles in Intune
 
> **Estimated reading time:** 3 minutes
> **Source:** [Unit 2 – Monitor device profiles in Intune](https://learn.microsoft.com/en-us/training/modules/oversee-device-profiles/2-monitor-device-profiles-intune)
 
Intune includes features in the Microsoft Intune admin center to help monitor and manage your device configuration profiles. You can check the status of a profile, see which devices are assigned, and update profile properties.
 
---
 
### View Existing Profiles
 
1. In the **Microsoft Intune admin center**, select **Devices**.
2. On the **Devices overview** page, select **Monitor**, then select **Assignment status**.
All existing profiles are listed, including platform details and whether each profile is assigned to any devices.
 
---
 
### View Details on a Profile
 
After creating a device profile, Intune provides graphical charts displaying the profile's status — such as whether it was successfully assigned or shows a conflict.
 
1. Select an existing profile and open the **Overview** tab.
   - The **top chart** shows the number of devices assigned to that specific device profile (e.g., Windows 10 and later devices), plus the count of non-matching platform devices also assigned.
   - The **bottom chart** shows the number of users assigned to the profile.
2. Select the circle in the **top graphical chart** to open **Device status**.
   - Lists all devices assigned to the profile and shows whether the profile was successfully deployed.
   - Only devices matching the specific platform (e.g., Windows 10 and later) are listed.
   - Close the Device status details when done.
3. Select the circle in the **bottom graphical chart** to open **User status**.
   - Lists all users assigned to the profile and shows whether the profile was successfully deployed.
   - Only users with the specific platform are listed.
   - Close the User status details when done.
4. Back in the **Profiles** list, select a specific profile to view or modify the following:
   | Section | Description |
   |---|---|
   | **Properties** | Change the profile name or update existing settings. |
   | **Assignments** | Include or exclude devices the policy should apply to. Use **Selected Groups** to target specific groups. |
   | **Device status** | Lists devices assigned to the profile and shows successful deployment status. Select a specific device for more details, including installed apps. |
   | **User status** | Lists usernames with devices affected by this profile and shows deployment status. Select a specific user for more details. |
   | **Per-setting status** | Filters output to show individual settings within the profile and whether each setting was successfully applied. |
---
 
### View Conflicts
 
When a policy setting causes a conflict, Intune surfaces it in the **All Devices** view along with all configuration profiles that contain the conflicting setting.
 
1. In the admin center, navigate to **Devices** > **All Devices** and select an existing device.
   > **Tip:** End users can find their device name from the Company Portal app.
2. Select **Device configuration**. All configuration policies applying to the device are listed.
3. Select a policy to see all settings it applies to the device.
4. If a device has a **Conflict** state, select that row.
   - A new window shows all profiles and profile names containing the setting causing the conflict.
Identifying the conflicting setting and the policies that include it makes it easier to resolve the conflict.
 
---
 
## 2. Manage Device Sync in Intune
 
> **Estimated reading time:** 3 minutes
> **Source:** [Unit 3 – Manage device sync in Intune](https://learn.microsoft.com/en-us/training/modules/oversee-device-profiles/3-manage-device-sync-intune)
 
Syncing devices with Intune ensures they receive the latest policies and actions. The **Sync device** action forces the selected device to immediately check in with Intune, receiving any pending actions or policies that have been assigned to it. This is useful for immediately validating and troubleshooting policies without waiting for the next scheduled check-in.
 
---
 
### Sync a Device
 
1. In the **Microsoft Intune admin center**, select **Devices**, then select **All devices**.
2. Select a device from the list, select **More**, then select **Sync**.
3. When prompted to confirm, select **Yes**.
4. To view the sync action status, navigate to **Devices** > **Monitor** > **Device actions**.
---
 
### Intune Policy Categories
 
Microsoft Intune policies are groups of settings that control features on mobile devices and computers. Policies are created using templates (recommended or custom settings) and deployed to device or user groups.
 
| Policy Type | Description |
|---|---|
| **Configuration policies** | Manage security settings and features on devices, including access to company resources. See: Intune device profiles. |
| **Device compliance policies** | Define rules and settings a device must meet to be considered compliant by conditional access policies. Can also monitor and remediate compliance independently of conditional access. |
| **Conditional access policies** | Help secure email and other services based on conditions you define. |
| **Corporate device enrollment policies** | Support enrollment of corporate-owned iOS devices using Apple DEP or Apple Configurator on a Mac. |
 
When a policy or app is deployed, Intune immediately notifies the device to check in with the Intune service. This notification typically arrives **within five minutes**.
 
> **Note:** If a device doesn't check in after the first notification, Intune makes **three more attempts**. If the device is offline (powered off or disconnected from a network), it will receive the policy at its next scheduled check-in.
 
---
 
### Policy Check-in Frequency
 
**Standard check-in frequency (established devices):**
 
| Platform | Check-in Frequency |
|---|---|
| iOS | Every 6 hours |
| macOS | Every 6 hours |
| Android | Every 8 hours |
| Windows 11 | Every 8 hours |
 
**More frequent check-in frequency (recently enrolled devices):**
 
| Platform | Check-in Frequency |
|---|---|
| iOS | Every 15 minutes for 6 hours, then every 6 hours |
| macOS | Every 15 minutes for 6 hours, then every 6 hours |
| Android | Every 3 minutes for 15 minutes, then every 15 minutes for 2 hours, then every 8 hours |
| Windows PCs (enrolled as devices) | Every 3 minutes for 30 minutes, then every 8 hours |
 
> **User-initiated sync:** Users can open the **Company Portal app** and manually sync their device at any time to immediately check for policies.
 
---
 
## 3. Manage Devices in Intune Using Scripts
 
> **Estimated reading time:** 3 minutes
> **Source:** [Unit 4 – Manage devices in Intune using scripts](https://learn.microsoft.com/en-us/training/modules/oversee-device-profiles/4-manage-devices-intune-use-scripts)
 
The **Intune management extension** lets you upload PowerShell scripts (for Windows devices) and shell scripts (for macOS devices) to run on managed devices. The management extension supplements mobile device management (MDM) capabilities and makes it easier to move to modern management.
 
**Example use case:** Create a PowerShell script that installs a legacy Win32 app on Windows devices, upload it to Intune, assign it to a Microsoft Entra group, run it on Windows devices, and monitor the run status from start to finish.
 
---
 
### Prerequisites
 
| Windows | macOS |
|---|---|
| Windows version 1607 or later | macOS version 10.12 or later |
| Devices must be joined to Microsoft Entra ID (including Hybrid AD joined devices) | Devices must be managed by Intune |
| Automatic MDM enrollment must be enabled in Microsoft Entra ID | Shell scripts must begin with `#!` in a valid location (e.g., `#!/bin/sh` or `#!/usr/bin/env zsh`) |
| | Command-line interpreters for applicable shells must be installed |
 
---
 
### Create a PowerShell Script Policy for Windows
 
1. In the **Microsoft Intune admin center**, select **Devices**.
2. Under the **Policy** section, select **Scripts**, then select **Add** > **Windows 10 and later**.
   > Adding scripts follows a similar flow to creating a profile — provide a name and description, then configure script settings.
3. In **Script settings**, configure the following properties:
   | Setting | Description |
   |---|---|
   | **Script location** | Browse to the PowerShell script. The script must be less than **200 KB (ASCII)**. |
   | **Run this script using the logged on credentials** | Select **Yes** to run with the user's credentials. Select **No** (default) to run in the system context. |
   | **Enforce script signature check** | Select **Yes** if the script must be signed by a trusted publisher. Select **No** (default) if signing is not required. |
   | **Run script in 64-bit PowerShell host** | Select **Yes** to run in a 64-bit PowerShell host on 64-bit client architecture. Select **No** (default) to run in a 32-bit PowerShell host. |
4. Select **Next** and configure **scope tags** and **assignments**.
   > PowerShell scripts in Intune can be targeted to **Microsoft Entra device security groups** or **Microsoft Entra user security groups**.
---
 
### Create a Shell Script Policy for macOS
 
Adding a shell script for macOS follows the same steps as creating a PowerShell script policy, but select **macOS** after choosing **Add**. The macOS script settings differ slightly.
 
1. In **Script settings**, configure the following properties:
   | Setting | Description |
   |---|---|
   | **Upload script** | Browse to the shell script. The script must be less than **200 KB (ASCII)**. |
   | **Run script as signed-in user** | Select **Yes** to run with the user's credentials. Select **No** (default) to run as the root user. |
   | **Hide script notifications on devices** | By default, notifications are shown for each script run. End users see an *"IT is configuring your computer"* notification from Intune on macOS devices. |
   | **Script frequency** | Select how often the script should run. Choose **Not configured** (default) to run the script only once. |
   | **Max number of times to retry if script fails** | Select how many times to retry if the script returns a non-zero exit code (non-zero = failure). Choose **Not configured** (default) for no retries. |
2. Select **Next** and configure **scope tags** and **assignments**.
   > Shell scripts assigned to user groups apply to any user signing in to the Mac.
---
 
## Tools and Resources Used
 
| Tool / Resource | Description |
|---|---|
| **Microsoft Intune** | Cloud-based endpoint management service for managing devices and policies |
| **Microsoft Intune Admin Center** | Web console for managing device profiles, policies, and scripts (`intune.microsoft.com`) |
| **Company Portal App** | End-user app for device enrollment and manual sync triggering |
| **PowerShell** | Scripting language used for Windows device management scripts via Intune |
| **Shell Scripts (`#!/bin/sh`, `#!/usr/bin/env zsh`)** | Unix shell scripts for macOS device management via Intune |
| **Intune Management Extension** | Intune component that enables script execution on Windows and macOS devices |
| **Microsoft Entra ID** (formerly Azure AD) | Identity platform used for device join, MDM enrollment, and security group targeting |
| **Apple DEP** (Device Enrollment Program) | Apple program for bulk corporate iOS device enrollment |
| **Apple Configurator** | Mac tool for iOS device enrollment in corporate environments |
| **Microsoft Learn – Oversee Device Profiles Module** | [https://learn.microsoft.com/en-us/training/modules/oversee-device-profiles/](https://learn.microsoft.com/en-us/training/modules/oversee-device-profiles/) |
 
---
 
## Source Notes
 
- **Profile assignment status screenshot:** The original Unit 2 references a screenshot of the profile assignment status charts in the Intune admin center. The image is hosted on Microsoft's CDN and is not reproduced here. Refer to the original source for visual reference.
- **"Intune device profiles" link:** Unit 3 references a link to "Intune device profiles" for configuration policies but does not provide the explicit URL. This likely refers to the Microsoft Docs page on [Create a device profile in Intune](https://learn.microsoft.com/en-us/mem/intune/configuration/device-profile-create).
- **Windows 10 vs Windows 11 check-in:** The check-in frequency table in Unit 3 lists "Windows 11" but the script creation steps refer to "Windows 10 and later." This reflects Intune's combined targeting for modern Windows versions.
- **No troubleshooting section present:** The source units do not include a dedicated troubleshooting section beyond conflict resolution in Unit 2.
- **Script size limit:** Both PowerShell and shell scripts must be under 200 KB (ASCII). The sources do not specify behavior if this limit is exceeded — consult Microsoft Docs for validation errors.
- **macOS shell script interpreter requirement:** The source states command-line interpreters "for applicable shells" must be installed but does not enumerate which shells are supported beyond `sh` and `zsh`.
- **Module published/updated:** April 16, 2025 (per page metadata). Content may be updated by Microsoft after this date.
---
 
