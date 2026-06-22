> `Windows, Microsoft Configuration Manager, Microsoft Intune`

# Microsoft Intune Device Profiles
 
> **Source:** Microsoft Learn — *Execute Device Profiles* module  
> Last reviewed: April 16, 2025  
> Author: `rstearman` (wwlpublish)
 
---
 
## Overview
 
Microsoft Intune includes settings and features that you can enable or disable on different devices within your organization. These settings and features are managed using **device profiles**.
 
**Examples of what profiles can do:**
 
- A **Wi-Fi profile** that gives different devices access to your corporate Wi-Fi.
- A **VPN profile** that gives different devices access to your VPN server within your corporate network.
---
 
## Types of Device Profiles
 
The types of profiles available depend on the platform you are targeting. The following profiles are available in Intune:
 
| Profile Type | Description |
|---|---|
| **Administrative Templates** | Hundreds of settings to control features in Microsoft Edge 77+, Internet Explorer, Microsoft Office, remote desktop, OneDrive, passwords, PINs, and more. Allows cloud-based management of ADMX-backed group policies. |
| **Certificates** | Configure trusted, SCEP, and PKCS certificates assigned to devices for authenticating Wi-Fi, VPN, and email profiles. |
| **Device Features** *(iOS and macOS)* | Controls features such as AirPrint, notifications, and shared device configurations on iOS and macOS devices. |
| **Device Restrictions** | Controls security, hardware, data sharing, and other settings. Example: prevent iOS users from using the device camera. |
| **Edition Upgrade and Mode Switch** | Automatically upgrades devices running certain Windows versions to a newer edition. |
| **Email** | Creates, assigns, and monitors Exchange ActiveSync email settings. Helps ensure consistency, reduces support calls, and lets end-users access company email on personal devices without manual setup. |
| **Endpoint Protection** | Configures BitLocker and Microsoft Defender settings for Windows devices. |
| **Identity Protection** | Controls the Windows Hello for Business experience on Windows 10 and Windows 10 Mobile devices. Configure availability, PIN requirements, and gesture requirements. |
| **Kiosk** | Configures a device to run one app or multiple apps. Allows customization of a start menu and web browser. |
| **VPN** | Assigns VPN profiles to users and devices for secure remote access to the company network. |
| **Wi-Fi** | Assigns wireless network settings to users and devices so they can connect to corporate Wi-Fi without manual configuration. |
| **Custom Profile** | Assigns device settings not built into Intune. See [Create a Custom Device Profile](#create-a-custom-device-profile) for details. |
 
---
 
## Create a Device Profile
 
The process for creating device profiles is similar across all profile types. Each platform has its own list of available profile types.
 
### Step-by-Step: Windows Device Profile
 
1. In the **Microsoft Intune admin center**, select **Devices**, then select the **Windows** platform, then select **Configuration Profiles**.
2. Select **Create Profile**.
3. Enter the following properties:
   - **Platform**: Choose which versions of Windows to include.
   - **Profile type**: Select the type you want to create.
   - <img width="903" height="562" alt="image" src="https://github.com/user-attachments/assets/536f84be-df90-4a82-8075-ae07dda476c6" />

4. Select **Create**.
> After creation, you will be prompted to configure the profile settings across several tabs.
 
---
 
### Profile Configuration Tabs
 
Once a profile is created, configure it across the following tabs:
 
#### Basics
Define a **name** for the profile and an optional **description**.
 
#### Configuration Settings
The options shown here depend on the profile type selected during creation.
 
- **Device Restrictions** profile → options for control panel availability, Microsoft Edge configurations, App Store restrictions, etc.
- **Wi-Fi** profile → options to configure SSID, EAP settings, etc.
#### Assignments
The profile can be assigned to:
 
- Selected Groups
- All Users & All Devices
- All Devices
- All Users
#### Applicability Rules
Further restrict the profile assignment or exclusion to specific OS versions or editions.
 
#### Review + Create
A summary of all profile settings is displayed. Select **Create** to finalize.
 
---
 
### Assignment Options
 
| Scope | Description |
|---|---|
| **Selected Groups** | Assign to specific Azure AD groups. |
| **All Users & All Devices** | Applies to all users and all enrolled devices. |
| **All Devices** | Applies to all enrolled devices regardless of user. |
| **All Users** | Applies to all users regardless of device. |
 
---
 
### Group Exclusion Rules and Warnings
 
> ⚠️ **Important:** Read carefully before configuring group exclusions.
 
Intune allows you to **exclude groups** from a policy assignment. However, there are critical behaviors to understand:
 
- When excluding groups, exclude **only users** or **only device groups** — not a mixture of both.
- Intune does **not** consider user-to-device relationships when evaluating exclusions.
- Including user groups while excluding device groups may **not produce expected results**.
- When mixed groups are used or conflicts arise, **inclusion takes precedence over exclusion**.
**Example — Potential Pitfall:**
 
> You want to assign a device profile to all devices **except kiosk devices**. You include the `All Users` group but exclude the `All Devices` group.  
> **Result:** All users *and their devices* get the policy — even if the user's device is part of `All Devices`. Exclusion only evaluates direct group members, not devices associated with a user.
 
**Example — Devices Without Users:**
 
> Devices that have no associated user will **not** receive a policy assigned to `All Users`, because those devices have no relationship to that group.
 
**Example — All Devices + Exclude All Users:**
 
> If you include `All Devices` and exclude `All Users`, **all devices receive the policy**. Devices with associated users are not excluded because exclusion only compares direct group membership.
 
---
 
### Applicability Rules
 
Applicability Rules allow you to further restrict or exclude profile assignments based on specific OS versions or editions. These are configured after the Assignments tab when creating or editing a profile.
 
---
 
## Create a Custom Device Profile
 
Intune may not include all the built-in settings you need. In those cases, you can create a **custom device profile** to configure settings that are not natively available in Intune.
 
> 💡 **Tip:** Before creating a custom profile, check the **Windows device restriction profile** — it contains many built-in settings that do not require custom values. Also note that Intune is frequently updated, so always verify whether a native setting now exists for your use case.
 
---
 
### Custom Profile for Windows 10 and Later
 
Windows custom profiles use **OMA-URI (Open Mobile Alliance Uniform Resource Identifier)** values to control device features. Windows exposes many settings through **Configuration Service Providers (CSPs)**, such as the **Policy CSP**.
 
**Steps:**
 
1. In the **Microsoft Intune admin center**, go to:  
   **Devices** > **Configuration profiles** > **Create profile** > Select **Windows 10 and later** as the platform.  
   The *Create profile* pane opens with the **Basics** tab selected.
2. On the **Configuration** tab, under **Custom OMA-URI Settings**, select **Add** to create a new setting.  
   *(Optional: Select **Export** to generate a `.csv` file of all configured values.)*
3. For each OMA-URI setting, enter the following:
#### OMA-URI Setting Fields
 
| Field | Description |
|---|---|
| **Name** | A unique name to identify the setting in the list. |
| **Description** | *(Optional)* A description of the setting. |
| **OMA-URI** | The OMA-URI path for the setting. **Case sensitive.** |
| **Data type** | One of: `String`, `String (XML)`, `Date and time`, `Integer`, `Floating point`, `Boolean`, `Base64` |
| **Value** | The value or file to associate with the OMA-URI. |
 
4. When finished, select **OK**.  
5. In the *Create profile* pane, select **Create**. The profile will appear in the profiles list.
---
 
### Example: Allow VPN Over Cellular
 
The following example enables the `Connectivity/AllowVPNOverCellular` setting, which allows a Windows device to open a VPN connection when on a cellular network.
 
```
OMA-URI:  ./Vendor/MSFT/Policy/Config/Connectivity/AllowVPNOverCellular
Data type: Integer
Value:     1
```
 
> ⚠️ **Compatibility Warning:** Not all settings are compatible with all Windows versions. Consult the [Configuration Service Provider reference](https://learn.microsoft.com/en-us/windows/client-management/mdm/configuration-service-provider-reference) to verify version support.  
> Additionally, Intune does not support **all** CSP settings. To confirm support, check the individual setting's article — the setting must support **Add** or **Replace** operations to work with Intune.
 
---
 
### Custom Profile for Android Devices
 
Android Enterprise custom profiles also use OMA-URI settings. The creation process is identical to Windows, but the profile is created under the **Android** platform.
 
> ⚠️ **Important:** Intune supports only a **limited** set of OMA-URI settings for Android Enterprise custom profiles. Android devices do not expose a complete list of configurable OMA-URI settings.
 
**Supported Android Enterprise Custom OMA-URI Settings:**
 
| Use Case | OMA-URI |
|---|---|
| Create a Wi-Fi profile with a pre-shared key | `./Vendor/MSFT/WiFi/Profile/SSID/Settings` |
| Create a per-app VPN profile | `./Vendor/MSFT/VPN/Profile/Name/PackageList` |
| Restrict copy/paste between work and personal apps | `./Vendor/MSFT/WorkProfile/DisallowCrossProfileCopyPaste` |
 
**OEMConfig (Additional Android Settings):**
 
In addition to custom OMA-URI settings, you can use **OEMConfig** to add, create, and customize OEM-specific settings for Android Enterprise devices. OEMConfig is typically used for settings not built into Intune. Available settings depend on what the OEM includes in their OEMConfig app.
 
---
 
### Custom Profile for Apple Devices (iOS/iPadOS/macOS)
 
For Apple devices, Intune custom profiles allow you to assign settings created using the **Apple Configurator** tool.
 
**Steps:**
 
1. In the **Microsoft Intune admin center**, create a device profile for **iOS/iPadOS** or **macOS** (as described in the [Create a Device Profile](#create-a-device-profile) section), selecting **Custom** as the profile type.
2. On the **Custom Configuration Profile** pane, configure:
   | Field | Description |
   |---|---|
   | **Custom configuration profile name** | A name for the policy as it appears on the device and in Intune status. |
   | **Configuration profile file** | Browse to the `.mobileconfig` file created with Apple Configurator. |
> ⚠️ **Compatibility Warning:** Ensure that the settings exported from Apple Configurator are compatible with the OS version on the target devices. For resolving incompatible settings, refer to the **Configuration Profile Reference** and **Mobile Device Management Protocol Reference** on the [Apple Developer website](https://developer.apple.com).
 
---
 
## Tools and Resources Used
 
| Tool / Resource | Description |
|---|---|
| [Microsoft Intune admin center](https://intune.microsoft.com) | Central portal for creating, managing, and assigning device profiles. |
| [Configuration Service Provider (CSP) Reference](https://learn.microsoft.com/en-us/windows/client-management/mdm/configuration-service-provider-reference) | Reference for Windows CSP settings usable in OMA-URI custom profiles. |
| [Policy CSP](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-configuration-service-provider) | Windows Policy Configuration Service Provider, used in OMA-URI settings. |
| [Apple Configurator](https://support.apple.com/apple-configurator) | Apple tool used to create `.mobileconfig` configuration profiles for iOS/iPadOS/macOS. |
| [Apple Developer — Configuration Profile Reference](https://developer.apple.com/business/documentation/Configuration-Profile-Reference.pdf) | Reference for Apple configuration profile settings and compatibility. |
| [Apple Developer — Mobile Device Management Protocol Reference](https://developer.apple.com) | MDM protocol documentation for Apple devices. |
| OEMConfig | Android Enterprise framework for OEM-specific device configuration settings. |
| **OMA-URI** (Open Mobile Alliance Uniform Resource Identifier) | Standard addressing scheme used for device management settings on Windows and Android. |
| **SCEP** (Simple Certificate Enrollment Protocol) | Certificate enrollment protocol supported by Intune certificate profiles. |
| **PKCS** (Public Key Cryptography Standards) | Certificate type supported by Intune for device authentication. |
| **Exchange ActiveSync** | Protocol used by the Email device profile in Intune. |
| **BitLocker** | Windows disk encryption tool configurable through Endpoint Protection profiles. |
| **Microsoft Defender** | Windows security platform configurable through Endpoint Protection profiles. |
| **Windows Hello for Business** | Identity platform configurable through Identity Protection profiles. |
 
---
 
## Source Notes
 
- **Screenshots referenced but not embedded:** The original Microsoft Learn pages include two screenshots (the Create Profile screen and the Custom OMA-URI Settings screen). These images are hosted on Microsoft's CDN and are not reproduced here. For visual reference, see the source pages linked below.
- **Android Enterprise OMA-URI list is intentionally limited:** The source explicitly states that only three OMA-URI paths are supported for Android Enterprise custom profiles. No additional paths are documented.
- **OEMConfig details are minimal:** The source only briefly mentions OEMConfig. For deeper documentation, consult the OEM's specific OEMConfig app documentation or the Android Enterprise Help Center.
- **Apple Configurator export format:** The source implies the exported file is a `.mobileconfig` file, but does not state this explicitly. This is standard Apple Configurator behavior.
- **Applicability Rules:** The source mentions this tab exists and allows OS version/edition targeting, but does not detail the available rule options. Further documentation may be found in the full Intune admin center.
- **Source pages (unit numbers) from the Microsoft Learn module:**
  - Unit 2: [Explore Intune Device Profiles](https://learn.microsoft.com/en-us/training/modules/execute-device-profiles/2-explore-intune-device-profiles)
  - Unit 3: [Create Device Profiles](https://learn.microsoft.com/en-us/training/modules/execute-device-profiles/3-create-device-profiles)
  - Unit 4: [Create a Custom Device Profile](https://learn.microsoft.com/en-us/training/modules/execute-device-profiles/4-create-custom-device-profile)
- **Last updated date on source:** April 16, 2025. Content may have been updated since this README was generated.
---
 
## GitHub README Usage
 
> This file is ready to paste directly into a `README.md` file in any GitHub repository.  
> To use it:
> 1. Copy the entire contents of this file.
> 2. Create or open your repository's `README.md`.
> 3. Paste the content and save.
> 4. If you wish to include the screenshots from the original Microsoft Learn pages, download them from the source URLs and place them in a `/docs/images/` folder, then update the image references accordingly.
