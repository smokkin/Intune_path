# Windows Autopilot: Deploy Devices — Complete Reference Guide

---

## 1. Overview — Windows Autopilot for Modern Deployment

Windows Autopilot is a **cloud-based deployment method** introduced with Windows 10. It allows IT administrators to set up and pre-configure new and existing Windows 10/11 devices. End users go through a streamlined Out-of-Box Experience (OOBE) to configure their devices **without needing a Windows image**.

### Advantages of Autopilot

- No need to use custom Windows images
- No need to inject drivers or customize deployments at the image level
- No on-premises deployment infrastructure required
- Simpler configuration compared to traditional image creation and management
- No heavy bandwidth consumption since no images are deployed
- Devices can join Azure AD automatically
- Auto-enrollment into MDM services (e.g., Microsoft Intune)
- Administrator account creation can be restricted
- OOBE content can be customized specifically for the organization

> **Cloud foundation:** Windows Autopilot is built around **Azure AD Premium** and **Microsoft Intune**.

---

### New Devices

Most organizations purchase new devices from an Original Equipment Manufacturer (OEM), typically pre-loaded with Windows 11 Pro. Previously, organizations would reimage these devices to meet organizational requirements.

With Autopilot, reimaging is no longer necessary. Regardless of the OEM image or pre-installed applications, Autopilot can:

- Reconfigure the device to meet organizational requirements
- Install Line-of-Business (LOB) apps
- Switch Windows edition (e.g., Pro → Enterprise)

From the user's standpoint, the device powers on into an OOBE during which the organization determines which options can be configured. A **Zero Touch Installation (ZTI)** experience is possible — the device is ready to use simply by plugging it in and turning it on.

---

### Refresh Existing Devices

Over time, devices may need refreshing due to performance degradation, accumulated app installs, or user reassignment. IT may choose to perform a **wipe-and-load**, and instead of traditional methods, Autopilot can be used to reset the device with a new OOBE as if it were wiped and reimaged.

Windows 10 and later still support traditional deployment. Many organizations used image-based deployment to upgrade to Windows 11, though **in-place upgrade** is the recommended path from Windows 8.1. Once on Windows 11, organizations can choose image-based deployment or adopt modern methods such as Autopilot or Feature Update.

---

### Autopilot vs. Traditional Methods

| Feature | Traditional Deployment | Modern Deployment (Autopilot) |
|---|---|---|
| Deploys Windows 11 images | ✅ Yes | ❌ No |
| Works with any preinstalled OS | ✅ Yes | ❌ No |
| Requires a previous Windows 11 installation | ❌ No | ✅ Yes |
| Uses on-premises infrastructure | ✅ Yes | ❌ No |
| Tools used | Windows ADK, WDS, MDT, Configuration Manager | Windows Configuration Designer, Windows Autopilot |

---

### When Traditional Methods Must Be Used

There are specific scenarios where Autopilot **cannot** be used and traditional deployment methods are required:

- **Bare-metal deployments** (no existing OS)
- When the **storage hardware** (where Windows is installed) must be replaced
- In the event of a **corrupted Windows 11 installation**
- When an organization requires **custom user information prompts** beyond what OOBE provides (e.g., customizing the LTI interface with MDT)

---

## 2. Requirements for Windows Autopilot

> ⚠️ **Important:** Only *known* devices can be deployed using Windows Autopilot. Hardware IDs must be uploaded before deployment can occur.

### Prerequisites

| Requirement | Details |
|---|---|
| **Windows version** | Devices must have Windows 10/11 pre-installed. Autopilot transforms an existing OS installation via OOBE; it does **not** deploy a Windows image. Must be Windows Pro, Enterprise, or Education. Windows Home is **not** supported. Minimum version: Creators Update (1703). Some features require more recent versions. |
| **Internet connectivity** | Autopilot is a cloud service. Devices must be connected to the internet. DNS resolution for internet names must be available. Ports **80 (HTTP)**, **443 (HTTPS)**, and **123 (UDP/NTP)** must be open. |
| **Microsoft Entra ID** | Mandatory. Autopilot depends on Microsoft Store for Business or Intune, both of which require Entra ID. Deployed devices automatically join Entra ID. For **automatic MDM enrollment**, Entra ID P1 or P2 is required. |
| **Intune or Microsoft Store for Business** | At least one must be used to upload device-specific information and configure the Autopilot deployment profile. |
| **MDM service (optional)** | After Entra ID join, devices can be auto-enrolled in Intune or another MDM service for device management. |

---

### Required URLs / Network Endpoints

The following URLs must be accessible from Autopilot-registered devices:

```
go.microsoft.com
login.microsoftonline.com
login.live.com
account.live.com
signup.live.com
licensing.mp.microsoft.com
licensing.md.mp.microsoft.com
ctldl.windowsupdate.com
download.windowsupdate.com
```

---

## 3. Prepare Device IDs for Autopilot

### Setup Process Overview

After meeting all prerequisites, the setup process for Windows Autopilot deployment includes:

1. Obtain the **hardware IDs** of the devices to be deployed
2. **Upload** the hardware IDs to the cloud service
3. **Create** a Windows Autopilot deployment profile
4. **Apply** the deployment profile to devices or device groups

---

### Configure Automatic MDM Enrollment in Intune

To manage Autopilot in Microsoft Intune, configure **automatic MDM enrollment** for Entra ID-joined Windows 11 devices:

1. In the **Microsoft Entra admin center**, select **Microsoft Entra ID**
2. On the Entra ID blade, select **Mobility (MDM and MAM)**, then select **Microsoft Intune**
3. Under **MDM user scope**:
   - Select **All** to allow all users to enroll
   - Select **Some** to restrict enrollment to specific groups
4. Select **Save**

---

### Get the CSV File from Your OEM Partner

If your OEM partner supports Windows Autopilot, you can obtain a CSV file when purchasing devices. The CSV file must contain the following three columns:

| Column | Description |
|---|---|
| Device Serial Number | Unique serial number of the device |
| Windows Product ID | The Windows product identifier |
| Hardware Hash | Cryptographic hardware hash identifying the device |

---

### Generate Your Own CSV File (PowerShell)

If the OEM does not provide a CSV file, generate one using the **Get-WindowsAutopilotInfo** PowerShell script.

**Step 1 — Install the script:**

```powershell
Install-Script -Name Get-WindowsAutopilotInfo
```

**Step 2 — Generate the CSV file:**

```powershell
Get-WindowsAutopilotInfo.ps1 -OutputFile D:\Devices\Device1.csv
```

> Replace `D:\Devices\Device1.csv` with your desired output path.

---

### Upload the Device CSV File to Intune

> **Required role:** Intune Administrator or Global Administrator

**Steps to upload:**

1. In **Microsoft Intune admin center**, navigate to **Devices** > **Enroll Devices** > **Devices**
2. Select **Import**
3. Browse and locate your CSV file
4. Import the file
5. After import completes, go to **Device enrollment** > **Windows enrollment** > **Windows Autopilot** > **Devices**, then select **Sync**
6. Refresh the view to confirm the new devices appear

**During upload you can:**
- Add devices to an **existing** Windows Autopilot deployment group
- **Create a new** Windows Autopilot deployment group
- Add them **without groups**

If the upload fails, an error CSV file can be downloaded with error descriptions and URLs pointing to detailed error documentation.

---

### Import a Device Hash Directly into Intune

Useful for **testing scenarios** or when a batch of machines is being built by onsite technicians. This method directly imports the device hash and assigns a Dynamic variable tag for Azure AD group assignment.

Open a PowerShell prompt on the Autopilot device at the **Welcome screen**:

```powershell
Install-Script -Name Get-WindowsAutopilotInfo

Get-WindowsAutoPilotInfo.ps1 -online -GroupTag "Autopilot-Devices" -Assign
```

> The `-GroupTag` value (`Autopilot-Devices`) maps to an Azure AD dynamic group. The `-Assign` flag automatically assigns the device to the associated group.

---

## 4. Device Registration and Out-of-Box Customization

### Create a Windows Autopilot Deployment Profile

A deployment profile specifies settings applied to devices during Autopilot deployment. Multiple profiles can exist, but only **one profile** is applied per device.

**High-level procedure in Intune:**

1. Sign in to Intune as a **Global Admin**
2. Navigate to **Device enrollment** > **Windows enrollment** > **Deployment Profiles**
3. Select **Create Profile**
4. Provide a **Name** and **Description**
5. For **Deployment mode**, choose one of:
   - **User-driven** — User specifies credentials during deployment; device is associated with the user account
   - **Self-deploying** — No user credentials required; device is **not** associated with a user account
6. Configure the **Out-of-box experience (OOBE)** settings:
   - Language
   - Keyboard
   - EULA
   - Privacy settings
   - User account settings
7. Select **Create** — the profile is now available to assign to devices

---

### Apply a Deployment Profile

Until a deployment profile is applied, Autopilot does **not** manage the OOBE setup phase.

**Procedure:**

1. Sign in to Intune as a **Global Admin**
2. Navigate to **Device enrollment** > **Windows enrollment** > **Deployment Profiles**
3. Choose a profile
4. Select **Assignments**
5. Select the group(s) to assign the profile to

After setup, powering on the devices triggers Autopilot-managed OOBE.

---

### Default OOBE vs. Autopilot OOBE

**Default OOBE** presents several questions to the end user, including:

- Preferred keyboard layout
- Acceptance of Microsoft Software License Terms
- Whether to join AD DS or Azure AD
- Privacy settings configuration

> ⚠️ These prompts can be confusing for employees, often resulting in help desk calls or misconfiguration. A user completing the default OOBE also becomes a **local Administrator** of the device, which can create security and management problems.

**With Windows Autopilot OOBE:**

- Administrators control the entire OOBE setup phase for known devices
- After hardware IDs are identified and a profile applied, devices connect to the Autopilot cloud service on startup
- Employees are asked only for their **company credentials** (Entra ID email and password)
- Many dialog boxes are pre-configured and hidden from users
- Devices automatically join Entra ID and can auto-enroll in Intune
- **Users do NOT become local Administrators** of the device — this is unique to Autopilot deployments

> 📌 **Note:** Windows Autopilot cannot perform advanced provisioning; use MDM solutions such as Intune for that. You can manage Windows Autopilot in Microsoft Store for Business or in Intune.

<img width="921" height="567" alt="image" src="https://github.com/user-attachments/assets/fbcd927b-45de-4be6-84f1-4c958ae8eb5a" />

---

## 5. Autopilot Deployment Scenarios

Windows Autopilot supports five deployment scenarios:

| Scenario | Use Case |
|---|---|
| User-Driven Mode | New device, user provides credentials during OOBE |
| Self-Deploying Mode | Kiosk / shared devices, zero user interaction |
| Autopilot for Existing Devices | Migrating Windows 8.1 devices to Autopilot-managed Windows 10/11 |
| Pre-Provisioned Deployment | IT/partner pre-configures device before delivery to user |
| Autopilot Reset | Reset device to initial state without reimaging |

---

### User-Driven Mode

Designed to transform new Windows 10/11 devices **directly from the factory** into a ready-to-use state without IT personnel ever touching the device.

**End user steps:**

1. Unbox the device, plug it in, and turn it on
2. Choose a language, locale, and keyboard
3. Connect to a wireless or wired network with internet access
4. Enter email address and password for the organizational account

**Requirements for Entra ID join (User-Driven):**

- Users must be permitted to join Entra ID
- User-driven mode must be selected in the Autopilot profile (if using Intune, not Microsoft Store for Business)
- The Autopilot profile must be assigned to an Entra ID **device group**
- The device must be registered in Windows Autopilot with a profile assigned

**Additional requirements for Hybrid Entra ID join:**

- Device must be running **Windows 1809 or later**
- **Hybrid Microsoft Entra ID joined** must be selected under "Join to Microsoft Entra ID as" in the profile
- Device must be able to access the internet **and** an Active Directory domain controller
- **Intune Connector for Active Directory** must be installed (performs the on-premises AD join)

---

### Self-Deploying Mode

Enables a device to be deployed with **little to no user interaction**, achieving a true ZTI experience. All OOBE prompts are pre-configured. The Enrollment Status Page displays during configuration, then the sign-in screen appears. Kiosk devices automatically sign in using a locally configured account.

**Requirements:**

- Create an Autopilot profile with **self-deploying mode** selected (only available in Intune, **not** Microsoft Store for Business)
- Profile must be assigned to the device **before** deployment
- Device must have **TPM 2.0**
- Requires **Windows 10 version 1903 or later**

> ⚠️ Some interaction may still be required: if only wireless is available, the network must be selected; if multiple languages are pre-installed, a language must be selected.

---

### Autopilot for Existing Devices

Allows re-imaging of a **Windows 8.1 device** for Autopilot user-driven mode using a single, native **Configuration Manager task sequence**. This transitions a traditionally image-managed device to modern Autopilot management.

**Process:**

**Step 1 — Create Intune profiles and generate configuration files**

```powershell
# Install required modules
Install-Module AzureAD
Install-Module WindowsAutopilotIntune
Import-Module WindowsAutopilotIntune

# Sign in to Intune
Connect-AutopilotIntune

# Retrieve Autopilot profiles
$AutopilotProfile = Get-AutopilotProfile

# Create JSON configuration file for each profile
$AutopilotProfile | ForEach-Object {
    $_ | ConvertTo-AutopilotConfigurationJSON | Set-Content -Encoding Ascii "~\Desktop\$($_.displayName).json"
}
```

> Save one profile as an ANSI-encoded file named exactly: **`AutopilotConfigurationFile.json`**

**Step 2 — Create the task sequence in Configuration Manager**

1. Add the desired OS image to Configuration Manager
2. Create a package containing `AutopilotConfigurationFile.json`
3. Create a Task Sequence using **"Deploy Windows Autopilot for existing devices"**
4. Configure the Task Sequence:
   - Enable **"Partition and format the target computer before installing the operating system"**
   - Select **"Join a workgroup"** — *Sysprep will fail if joined to a domain*
   - Select **"Do not install any software updates"**
   - On the System Preparation page, select the package containing `AutopilotConfigurationFile.json`
   - **For Windows 1903 and 1909:** Disable the **"Prepare Windows for Capture"** step and add a new **Run command-line** step:
     ```
     c:\windows\system32\sysprep\sysprep.exe /oobe /reboot
     ```
     This prevents Sysprep from deleting the `AutopilotConfigurationFile.json`.
5. Complete the task sequence wizard

> **Warning:** Devices **must not** already be registered with the Windows Autopilot service before running this task sequence.

---

### Windows Autopilot for Pre-Provisioned Deployment

Available from **Windows 10 version 1903**. Allows partners or IT staff to pre-configure a Windows PC so that it's **fully configured and business-ready** before delivery to the end user.

The provisioning process is **split**:
- Time-consuming portions (device/user app installs, policies) are completed by IT first
- Final user settings and policies are applied when the user powers on the device

**Pre-provisioning steps:**

1. Enable **"Windows Autopilot for pre-provisioned deployment"** in the desired Autopilot profile
2. Connect device via **ethernet** (required for pre-provision) and boot it
3. At the first OOBE screen, press the **Windows key five times**
4. In the additional dialog, select **"Windows Autopilot provisioning"**
5. Verify the device information. If changes are needed, update in Intune and select **Refresh**
6. Select **Provision** to begin provisioning
7. When complete, select **Reseal**
8. Deliver the device to the user

**End user steps after receiving the device:**

1. Connect and power on the device
2. Select regional and keyboard settings
3. Select a Wi-Fi network (if applicable)
4. Sign in with organizational credentials

**Requirements:**

- Windows 1903 or later
- Intune subscription
- Device must support **TPM 2.0** and device attestation
- Virtual machines are **not** supported
- Internet connectivity required during the final user process
- For Hybrid Entra ID join, connectivity to a domain controller is also required

> 💡 **Tip:** Pre-provisioned deployment enables an administrator to install the bulk of machine-targeted applications before delivery, leaving only user-specific applications for the final onboarding step. This drastically reduces device provisioning time and improves user experience.

---

### Windows Autopilot Reset

Used to reset devices to their initial state after use — for example, temporary employees, training room computers, or device reassignment.

**What Autopilot Reset does:**
- Removes all personal files, apps, and settings
- Resets the Windows device to its initial state from the lock screen
- Optionally re-deploys organizational apps and settings via Intune or another MDM

> **Advantage over traditional wipe-and-load:** No Windows image redeployment is required.

**Two reset scenarios:**

#### Local Windows Autopilot Reset

Uses native Windows reset functionality. Works regardless of current device management method.

**Preserves:**
- Device name
- Azure AD membership
- MDM enrollment

> ⚠️ **Local Windows Autopilot Reset is disabled by default.** This prevents accidental triggering.

**To enable:**
- Set the policy **`DisableAutomaticReDeploymentCredentials`** to **`0`** (false)

**To trigger:**
- At the **Windows lock screen**, press **`Ctrl + Windows logo key + R`**
- Only users with **administrative permissions** can start a local reset

#### Remote Windows Autopilot Reset

Initiated via an MDM service such as Microsoft Intune. No physical access to each device is required.

**Steps in Intune:**

1. In **Microsoft Intune admin center**, navigate to **Devices** > **Windows**
2. Select the device to reset
3. Select **More** (ellipsis `...`) > **Autopilot Reset**

---

## 6. Troubleshooting Windows Autopilot

### Key Factors to Verify

When troubleshooting, verify the following:

| Factor | Check |
|---|---|
| **Configuration** | Has Entra ID and Intune (or equivalent MDM) been configured per Autopilot requirements? |
| **Network connectivity** | Can the device access all required Autopilot service URLs? |
| **Autopilot OOBE behavior** | Were only the expected OOBE screens displayed? Was the Entra ID credentials page customized with org-specific details? |
| **Entra ID join** | Was the device able to join Entra ID? |
| **MDM enrollment** | Was the device able to enroll in Intune or equivalent MDM? |

---

### Troubleshoot Autopilot OOBE Issues

If expected Autopilot behavior doesn't occur during OOBE, check whether the device received an Autopilot profile and review the profile settings.

**Registry location for Autopilot profile settings:**

```
HKLM\SOFTWARE\Microsoft\Provisioning\Diagnostics\Autopilot
```

**Event log location:**

```
Event Viewer > Application and Services Logs > Microsoft > Windows > Provisioning-Diagnostics-Provider > Autopilot
```

**Additional diagnostics:**
- Use **Event Tracing for Windows (ETW)** to capture detailed information from Autopilot and related components
- ETW trace files can be viewed in **Windows Performance Analyzer** or similar tools

---

### Event Log Reference Table

| Event ID | Type | Description |
|---|---|---|
| 100 | Warning | `"Autopilot policy [name] not found."` — Typically a temporary problem while the device waits for an Autopilot profile to be downloaded. |
| 171 | Error | `"AutopilotManager failed to set TPM identity confirmed. HRESULT=[error code]."` — Indicates an issue with TPM attestation, needed to complete the **self-deploying mode** process. |
| 172 | Error | `"AutopilotManager failed to set Autopilot profile as available. HRESULT=[error code]."` — Typically related to Event ID 171. |

---

### Windows Autopilot Diagnostics Script

Aggregates many troubleshooting techniques into an easily readable format to isolate issues. Run directly on the device from PowerShell:

```powershell
Set-ExecutionPolicy ByPass
Install-Script Get-AutoPilotDiagnostics -force
Get-AutoPilotDiagnostics -Online
```

> Accept any download prompts. Connect to the tenant using an account with appropriate credentials. The script uses GraphAPI extensions for Intune to display a list of policies, apps, and their status.

---

### Troubleshoot Entra ID Join Issues

The most common Entra ID join issue is **insufficient permissions**.

**Check:**
- The correct configuration is in place to allow users to join devices to Entra ID
- The user has not exceeded the **maximum number of devices** they are allowed to join (as configured in Entra ID)

**Common error:**

| Error Code | Description |
|---|---|
| `801C0003` | Displayed on a "Something went wrong" page — indicates a failed attempt to join Entra ID |

---

### Troubleshoot Intune Enrollment Issues

Common causes:
- Incorrect or missing **licenses** assigned to the user
- User has **too many devices** enrolled

**Common error:**

| Error Code | Description |
|---|---|
| `80180018` | Displayed on a "Something went wrong" page with title — indicates a failed MDM enrollment |

> 📖 For full Intune enrollment troubleshooting, see:  
> [Troubleshoot device enrollment in Intune](https://learn.microsoft.com/en-us/troubleshoot/mem/intune/device-enrollment/troubleshoot-device-enrollment-in-intune)

---

### Troubleshoot Device Import Errors

**Symptom:**
Importing a device CSV file results in no action, and a **`400` error** appears in network trace with the message:

```
Cannot convert the literal '[DEVICEHASH]' to the expected type 'Edm.Binary'
```

**Cause:** The device hash within the file is either **corrupted** or **not properly padded**.

**Resolution:** Make a minor edit to the CSV file to ensure the device hash is in the correct format and re-import.

---

## 7. Confirmed Working Scenarios with Detailed Solutions

The following are real-world applicable scenarios derived from the documentation, with detailed step-by-step solutions and supporting sources.

---

### Scenario 1 — New Device Deployment via Autopilot (Zero Touch)

**Problem:** IT needs to provision laptops purchased from an OEM and ship them directly to remote employees without touching the devices.

**Solution:**

1. Contact your OEM to request the hardware CSV file (Serial Number, Windows Product ID, Hardware Hash) at time of purchase. Many major OEMs (Dell, HP, Lenovo, Microsoft Surface) support direct Autopilot registration.
2. Upload the CSV to Intune: **Devices > Enroll Devices > Devices > Import**
3. Sync the devices: **Device enrollment > Windows enrollment > Windows Autopilot > Devices > Sync**
4. Create an Autopilot deployment profile with **User-Driven** mode and assign it to an Entra ID device group
5. Ship devices directly to employees. On first boot, employees connect to Wi-Fi and sign in with corporate credentials
6. The device automatically joins Entra ID and enrolls in Intune, applying all assigned policies and apps

**Sources:**
- [Windows Autopilot overview](https://learn.microsoft.com/en-us/autopilot/windows-autopilot)
- [OEM registration for Autopilot](https://learn.microsoft.com/en-us/autopilot/oem-registration)

---

### Scenario 2 — Generating and Uploading Hardware Hash for Existing Devices

**Problem:** The organization has existing Windows 10/11 devices that were not registered with Autopilot at purchase.

**Solution:**

1. On each device (or via Configuration Manager/Intune script deployment), run:
   ```powershell
   Install-Script -Name Get-WindowsAutopilotInfo
   Get-WindowsAutopilotInfo.ps1 -OutputFile C:\AutopilotInfo\Device1.csv
   ```
2. Collect all CSV files and merge them if needed
3. Upload to Intune and assign a deployment profile
4. When the device needs to be reset/refreshed, Autopilot manages the process

**Sources:**
- [Get-WindowsAutopilotInfo on PowerShell Gallery](https://www.powershellgallery.com/packages/Get-WindowsAutopilotInfo)
- [Manually register devices with Windows Autopilot](https://learn.microsoft.com/en-us/autopilot/manual-registration)

---

### Scenario 3 — Kiosk / Shared Device Deployment (Self-Deploying Mode)

**Problem:** IT needs to deploy shared kiosk devices in a retail environment with no user login required during setup.

**Solution:**

1. Verify devices have **TPM 2.0** and run **Windows 10 1903 or later**
2. In Intune, create an Autopilot profile with **Self-Deploying** mode selected
3. Assign the profile to the device group containing kiosk devices
4. Configure a **Kiosk profile** in Intune to restrict the device to a single app after enrollment
5. Power on the device — Autopilot auto-completes OOBE, enrolls in Intune, applies the kiosk configuration, and signs in automatically using the configured local account

> ⚠️ Self-deploying mode is **only available in Intune** — not Microsoft Store for Business.

**Sources:**
- [Windows Autopilot self-deploying mode](https://learn.microsoft.com/en-us/autopilot/self-deploying)
- [Set up a kiosk with Intune](https://learn.microsoft.com/en-us/mem/intune/configuration/kiosk-settings-windows)

---

### Scenario 4 — Migrating Windows 8.1 Devices to Autopilot (Existing Devices Scenario)

**Problem:** Organization has older Windows 8.1 devices that need to be migrated to Windows 10/11 under modern management without buying new hardware.

**Solution:**

1. Create the Autopilot profile in Intune for User-Driven mode
2. Install PowerShell modules and generate `AutopilotConfigurationFile.json` using the commands in [Section 5 — Autopilot for Existing Devices](#autopilot-for-existing-devices)
3. In Configuration Manager, create a task sequence using **"Deploy Windows Autopilot for existing devices"**
4. Include the JSON configuration file as a package in the task sequence
5. Set the task sequence to join a **workgroup** (not a domain — Sysprep will fail otherwise)
6. For Windows 1903/1909, disable "Prepare Windows for Capture" and add a manual `sysprep.exe /oobe /reboot` command line
7. Run the task sequence on the 8.1 device — it re-images to Windows 10/11 and drops the Autopilot configuration file in place
8. On first boot after imaging, the device goes through Autopilot OOBE as a new Autopilot-managed device

**Sources:**
- [Windows Autopilot for existing devices](https://learn.microsoft.com/en-us/autopilot/existing-devices)
- [Configuration Manager Autopilot task sequence](https://learn.microsoft.com/en-us/mem/configmgr/osd/deploy-use/windows-autopilot-for-existing-devices)

---

### Scenario 5 — Resetting a Device for a New Employee (Autopilot Reset)

**Problem:** An employee leaves the organization. Their laptop needs to be wiped and re-provisioned for a new hire without IT physically handling the device.

**Solution — Remote Reset via Intune:**

1. In **Microsoft Intune admin center**, go to **Devices > Windows**
2. Locate the target device
3. Select **More (...)** > **Autopilot Reset**
4. The device remotely wipes personal files, apps, and settings
5. It retains its Entra ID membership and MDM enrollment
6. On next boot, the new user completes the Autopilot OOBE and is automatically configured

**Solution — Local Reset (for IT staff with physical access):**

1. Ensure the policy `DisableAutomaticReDeploymentCredentials` is set to **`0`** on the device
2. At the **Windows lock screen**, press **`Ctrl + Windows logo key + R`**
3. The device initiates the reset

**Sources:**
- [Windows Autopilot Reset](https://learn.microsoft.com/en-us/autopilot/windows-autopilot-reset)
- [Remote Windows Autopilot Reset via Intune](https://learn.microsoft.com/en-us/mem/intune/remote-actions/windows-autopilot-reset)

---

### Scenario 6 — Troubleshooting a Device That Did Not Receive an Autopilot Profile

**Problem:** A device went through OOBE but did not receive the expected Autopilot experience (default Windows OOBE appeared instead).

**Diagnosis steps:**

1. Check the registry for profile data:
   ```
   HKLM\SOFTWARE\Microsoft\Provisioning\Diagnostics\Autopilot
   ```
2. Check Event Viewer logs:
   ```
   Application and Services Logs > Microsoft > Windows > Provisioning-Diagnostics-Provider > Autopilot
   ```
   Look for **Event ID 100** (profile not found — typically temporary while downloading).

3. Run the diagnostics script:
   ```powershell
   Set-ExecutionPolicy ByPass
   Install-Script Get-AutoPilotDiagnostics -force
   Get-AutoPilotDiagnostics -Online
   ```

**Common root causes and fixes:**

| Root Cause | Fix |
|---|---|
| Device not registered in Autopilot | Upload device hardware hash CSV to Intune |
| Profile not assigned to device or group | Assign the Autopilot profile to the appropriate device group in Intune |
| Network issue — required URLs blocked | Verify firewall/proxy allows all required Autopilot URLs (see [Section 2](#required-urls--network-endpoints)) |
| Profile sync delay | Trigger a manual sync in Intune: **Windows Autopilot > Devices > Sync** |

**Sources:**
- [Troubleshooting Windows Autopilot (level 100/200)](https://aka.ms/AA6d57a)
- [Troubleshooting Windows Autopilot](https://aka.ms/AA80h34)
- [Windows Autopilot known issues](https://learn.microsoft.com/en-us/autopilot/known-issues)

---

### Scenario 7 — Device CSV Import Returns a 400 Error

**Problem:** Importing the device CSV file to Intune results in no action and a `400` error: `Cannot convert the literal '[DEVICEHASH]' to the expected type 'Edm.Binary'`

**Cause:** The hardware hash in the CSV is corrupted or improperly padded.

**Fix:**

1. Open the CSV file in a text editor (not Excel, which may silently modify the hash)
2. Verify the hash column contains no extra spaces, line breaks, or truncated characters
3. If re-generating the hash, use `Get-WindowsAutopilotInfo.ps1` and save without modifications
4. Re-upload the corrected file to Intune

**Sources:**
- [Troubleshooting Windows Autopilot](https://aka.ms/AA80h34)
- [Manually register devices](https://learn.microsoft.com/en-us/autopilot/manual-registration)

---

## 8. Tools and Resources Used

| Tool / Resource | Purpose |
|---|---|
| **Windows Autopilot** | Cloud-based modern deployment service for Windows 10/11 |
| **Microsoft Intune** | MDM platform for device enrollment, management, and Autopilot profile configuration |
| **Microsoft Entra ID (Azure AD)** | Identity platform; required for Autopilot device join and user authentication |
| **Microsoft Entra ID P1 / P2** | Required for automatic MDM enrollment |
| **Microsoft Store for Business** | Alternative to Intune for managing Autopilot deployments |
| **Get-WindowsAutopilotInfo.ps1** | PowerShell script for generating device hardware hash CSV files |
| **Get-AutoPilotDiagnostics** | PowerShell script for automated Autopilot troubleshooting |
| **Windows Performance Analyzer** | Tool for viewing ETW trace files from Autopilot diagnostics |
| **Event Viewer** | For reviewing Autopilot OOBE event logs |
| **Configuration Manager (SCCM)** | Used in the "Autopilot for existing devices" task sequence scenario |
| **Windows ADK** | Traditional deployment toolset (referenced for comparison) |
| **Windows Deployment Services (WDS)** | Traditional deployment toolset (referenced for comparison) |
| **Microsoft Deployment Toolkit (MDT)** | Traditional deployment toolset (referenced for comparison) |
| **Windows Configuration Designer** | Modern deployment toolset alternative to Autopilot |
| **TPM 2.0** | Required for self-deploying mode and pre-provisioned deployment |
| **PowerShell modules: AzureAD, WindowsAutopilotIntune** | Required for generating Autopilot config JSON files for existing devices |
| **ETW (Event Tracing for Windows)** | Advanced diagnostics for capturing Autopilot component traces |

**Official Microsoft Documentation Links:**

| Link | Description |
|---|---|
| [Windows Autopilot overview](https://learn.microsoft.com/en-us/autopilot/windows-autopilot) | Main Autopilot documentation hub |
| [Manual device registration](https://learn.microsoft.com/en-us/autopilot/manual-registration) | How to manually register existing devices |
| [OEM registration for Autopilot](https://learn.microsoft.com/en-us/autopilot/oem-registration) | OEM-based hardware hash submission |
| [Self-deploying mode](https://learn.microsoft.com/en-us/autopilot/self-deploying) | Full self-deploying documentation |
| [Autopilot for existing devices](https://learn.microsoft.com/en-us/autopilot/existing-devices) | Migrating older devices via Config Manager |
| [Pre-provisioned deployment](https://aka.ms/AA6dcx6) | Pre-provisioned deployment detailed guide |
| [Windows Autopilot Reset](https://learn.microsoft.com/en-us/autopilot/windows-autopilot-reset) | Full Autopilot Reset documentation |
| [Troubleshoot device enrollment in Intune](https://learn.microsoft.com/en-us/troubleshoot/mem/intune/device-enrollment/troubleshoot-device-enrollment-in-intune) | Intune enrollment troubleshooting |
| [Troubleshooting Windows Autopilot (level 100/200)](https://aka.ms/AA6d57a) | Autopilot troubleshooting levels 100 and 200 |
| [Troubleshooting Windows Autopilot](https://aka.ms/AA80h34) | General Autopilot troubleshooting reference |
| [Get-WindowsAutopilotInfo on PowerShell Gallery](https://www.powershellgallery.com/packages/Get-WindowsAutopilotInfo) | Hardware hash generation script |
| [Config Manager Autopilot task sequence](https://learn.microsoft.com/en-us/mem/configmgr/osd/deploy-use/windows-autopilot-for-existing-devices) | Task sequence guide for existing devices |

---

