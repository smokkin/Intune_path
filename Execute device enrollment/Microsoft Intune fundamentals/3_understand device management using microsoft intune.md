# Managing Devices with Microsoft Endpoint Manager (Microsoft Intune)

> `IT Administrators, System Engineers, Security Engineers, and anyone preparing for Microsoft 365 / Intune certifications`

## Overview

Microsoft Endpoint Manager (now primarily referred to as **Microsoft Intune**) is a cloud-based device management solution that helps organizations provision, enroll, configure, protect, and retire managed devices across all major platforms Windows, iOS/iPadOS, Android, and macOS.

The device lifecycle in Intune follows this general flow:

```
Provision → Enroll → Configure → Protect → Retire
```

This document covers each stage: provisioning, enrolling, protecting, retiring, and the specific enrollment methods available per platform.

---

## Understanding Provisioning

> **Estimated reading time:** 5 minutes  
> **Source:** [Unit 2 — Understand Provisioning](https://learn.microsoft.com/en-us/training/modules/manage-devices-with-microsoft-endpoint-manager/2-understand-provisioning)

Provisioning is the process by which your organization issues new or repurposed devices to individuals. Before a device is enrolled with Intune or Configuration Manager, you may need to **provision** the device first. During provisioning, the user signs in and connects to your organization. Devices can be enrolled during or after the provisioning process, depending on the platform and organizational requirements.

> ⚠️ **Important:** If your organization currently uses **image-based provisioning**, you should consider transitioning to **modern provisioning** going forward.

### Modern Provisioning Benefits

Modern provisioning provides the following advantages over traditional image-based provisioning:

- ✅ Decreases costly IT workload by auto-generating device image creation
- ✅ Provides self-service provisioning directly by end users
- ✅ Provides faster time to productivity for end users
- ✅ Increases out-of-the-box security
- ✅ Lowers operational expenses

---

### Provisioning Windows Devices

Windows devices are provisioned using **Windows Autopilot**. Autopilot simplifies the enrollment process by eliminating the need to build, maintain, and apply custom OS images.

With Microsoft Intune and Autopilot, you can:
- Ship new devices directly to end users
- Have the device configure itself automatically
- Manage policies, profiles, apps, and more post-enrollment

#### Windows Autopilot Deployment Types

| Mode | Description |
|---|---|
| **Self-Deploying Mode** | For kiosks, digital signage, or shared devices. Joins the device to Microsoft Entra ID and enrolls it via MDM automatically. Prevents desktop access until fully provisioned (via Enrollment Status Page). |
| **White Glove** | Enables partners or IT staff to pre-provision a Windows 10/11 PC so it's fully configured and business-ready before the user receives it. |
| **Autopilot for Existing Devices** | Enables easy deployment of the latest version of Windows 10/11 to existing (older) devices. |
| **User Driven Mode** | Designed for traditional users. Simple enough that anyone can complete it; devices can be shipped directly to users with minimal instructions. |

#### Alternative Windows Provisioning Methods

- **Configuration Manager** — Supports deploying Windows OS via:
  - Software Center
  - Standalone Media
  - Preboot Execution Environment (PXE)
  - Multicast
- **Intune Bulk Enrollment** — Provision devices using a provisioning package for your Microsoft Entra tenant.

---

### Provisioning Apple Devices

Apple devices (iOS, iPadOS, macOS) are provisioned using:

- **Apple Business Manager (ABM)**
- **Apple School Manager (ASM)**

Both integrate with **Apple's Automated Device Enrollment (ADE)** (formerly known as the Device Enrollment Program DEP).

**How ADE Works:**
1. Devices (iPhones, iPads, MacBooks) are purchased and registered in ABM/ASM.
2. Devices can be shipped directly to end users.
3. When the user powers on the device, **Setup Assistant** runs with preconfigured settings.
4. The device automatically enrolls into Intune management.

> No IT technician needs to physically handle the device before it reaches the user.

---

### Provisioning Android Devices

Corporate-owned Android devices are provisioned using the **Android Enterprise zero-touch** enrollment program.

**How Zero-Touch Works:**
1. The device is started.
2. The device checks whether an enterprise configuration has been assigned.
3. If a configuration is found, it starts the **fully managed device provisioning** process automatically.

> Large numbers of devices can be provisioned and enrolled without ever being physically touched by IT staff.

---

## Enrolling Devices

> **Estimated reading time:** 4 minutes  
> **Source:** [Unit 3 — Learn about enrolling devices](https://learn.microsoft.com/en-us/training/modules/manage-devices-with-microsoft-endpoint-manager/3-enroll-devices)

After provisioning, devices must be **enrolled in Intune** before they can access organizational resources. Enrollment means the device is recognized by Intune via a **mobile device management (MDM) certificate**.

Enrollment method selection depends on three key factors:

| Factor | Options |
|---|---|
| **Device Ownership** | Corporate-owned (COD) or Personal (BYOD) |
| **Device Type / Platform** | iOS/iPadOS, Windows, Android, macOS |
| **Device Management Requirements** | Resets required, user affinity, device locking |

<img width="796" height="496" alt="image" src="https://github.com/user-attachments/assets/1f9a74f7-3e65-420c-b2ad-e1009033f3a1" />

> **Note (Configuration Manager):** For Configuration Manager-managed devices, you install the Configuration Manager client on the device, which then registers with the site rather than using MDM certificate-based enrollment.

---

### Device Ownership

#### Corporate-Owned Devices (COD)

- Phones, tablets, and PCs owned and distributed by the organization
- Supports: automatic enrollment, shared devices, preauthorized enrollment
- Common enrollment method: **Device Enrollment Manager (DEM)** — used by an admin or manager
- iOS/iPadOS can be enrolled via **Apple's Automated Device Enrollment (ADE)**
- Devices can be tagged/identified using **IMEI (International Mobile Equipment Identifier)** numbers

#### Bring Your Own Device (BYOD)

- Personally owned phones, tablets, and PCs
- Users install and run the **Company Portal app** to self-enroll
- Users can perform common tasks through Company Portal:
  - Enroll their device
  - Install apps
  - Locate organization information and IT support details

---

### Device Types (Platforms)

Intune supports the following device platforms:

| Platform | Notes |
|---|---|
| iOS/iPadOS | Full MDM and MAM support |
| Windows | Richest set of enrollment methods; GPO, Autopilot, co-management |
| Android | Supports Android Enterprise; work profile and fully managed modes |
| macOS | MDM support; requires Apple MDM Push certificate |

Each platform offers its own set of:
- Enrollment methods
- Device settings
- Protection policy settings
- Configuration policy settings
- Custom policy settings
- Remote actions

---

### Device Management Requirements

Before a device can connect to the organization, specific initial requirements may be enforced:

| Requirement | Description |
|---|---|
| **Reset Required** | Wipes the device during enrollment, ensuring a clean state |
| **User Affinity** | Associates each device with a specific user |
| **Locked** | Prevents users from unenrolling their device |

---

## Device Enrollment Methods

> **Estimated reading time:** 4 minutes  
> **Source:** [Unit 7 — Understand device enrollment methods](https://learn.microsoft.com/en-us/training/modules/manage-devices-with-microsoft-endpoint-manager/7-device-enrollment-methods)

---

### iOS/iPadOS and macOS Enrollment Methods

| Method | Platforms | Description |
|---|---|---|
| **BYOD** | iOS/iPadOS, macOS | End users enroll their own device via Intune. Requires an **Apple MDM Push certificate** to manage iOS/iPadOS and macOS devices. |
| **DEM (Device Enrollment Manager)** | iOS/iPadOS, macOS | A special user account used to enroll and manage **multiple corporate-owned devices**. Good for point-of-sale or utility app devices. Not suitable for users needing email or company resource access. |
| **Apple Automated Device Enrollment (ADE)** | iOS/iPadOS, macOS | Enrollment through Apple Business Manager or Apple School Manager. Devices auto-enroll when powered on. |
| **Apple Configurator (USB)** | iOS/iPadOS | Direct USB-based enrollment via the Apple Configurator tool. |

---

### Windows Enrollment Methods

| Method | Description |
|---|---|
| **BYOD** | End users install and run the **Company Portal app** to enroll personal devices. Provides access to company resources like email. |
| **Windows Autopilot** | Enrollment details are configured up front before the user receives the device. The Autopilot process runs on first power-on, enabling employees to configure devices to be business-ready with just a few clicks. |
| **Bulk Enrollment** | Uses the **Windows Configuration Designer (WCD)** app to create a provisioning package. The package is applied to corporate devices, joining them to the Microsoft Entra tenant and enrolling them in Intune. Once applied, Microsoft Entra users can sign in. |
| **Device Enrollment Manager (DEM)** | A special account allowing large-scale enrollment of multiple devices. |
| **Automatic Enrollment** | Devices auto-enroll via Microsoft Entra ID join or hybrid domain join. |
| **Co-Management** | Devices managed by both Configuration Manager and Intune simultaneously. |
| **Group Policy (GPO)** | Enrollment triggered via Active Directory Group Policy for hybrid domain-joined Windows devices. |

---

### Android Enrollment Methods

Android Enterprise enrollment supports two primary device modes:

| Mode | Description |
|---|---|
| **Corporate-Owned, Single User** | Device is exclusively for work; not for personal use. |
| **Work Profile (Personally Owned)** | IT maintains a clear boundary between work and personal data on the same device. Work data is managed; personal data is not. |

#### Personal Android Enrollment Methods

| Method | Description |
|---|---|
| **BYOD / Corporate Owned (Company Portal)** | End users enroll using the **Company Portal app** for secure access to company email, apps, and data. |
| **Android Enterprise Work Profile** | Work profile management is set up during enrollment. Separates personal data from work files. The organization manages the work profile but cannot manage personal data. |

#### Corporate Android Enrollment Methods

| Method | Description |
|---|---|
| **DEM (Device Enrollment Manager)** | Uses a DEM account (Intune permission applied to a Microsoft Entra user) to enroll up to **1,000 devices**. Useful when devices are enrolled and prepared before handing out to users. There is a **limit of 150 DEM accounts** in Microsoft Intune. |
| **IMEI / Serial Number (SN)** | Identify and pre-register corporate devices using their IMEI number or serial number before user assignment. |

<img width="788" height="484" alt="image" src="https://github.com/user-attachments/assets/f9c87c98-98a7-4c70-9768-35e4738e79f2" />

---

### Enrollment Options

Intune administrators can configure the following enrollment options:

| Option | Description |
|---|---|
| **Terms and Conditions** | Optionally require users to accept company terms before enrolling and accessing resources. |
| **Enrollment Restrictions** | Restrict enrollment by device platform or limit the number of devices per user. Can block personal device enrollment entirely. |
| **Enable Apple Device Enrollment** | An MDM push certificate is required for iOS/iPadOS and macOS enrollment. |
| **Corporate Identifiers** | Pre-list IMEI numbers and serial numbers to identify corporate-owned devices. |
| **Multi-Factor Authentication (MFA)** | Require an extra verification method (phone, PIN, or biometric) at enrollment time. |
| **Device Enrollment Manager** | Assign users the DEM role to allow them to enroll large numbers of mobile devices with a single account. |
| **Device Categories** | Use device categories to automatically add devices to groups based on defined categories, making device management easier. |

---

## Protecting Devices

> **Estimated reading time:** 4 minutes  
> **Source:** [Unit 5 — Explore how to protect devices](https://learn.microsoft.com/en-us/training/modules/manage-devices-with-microsoft-endpoint-manager/5-protect-devices)

Protecting devices from unauthorized access is one of the most critical tasks in modern IT. In addition to the configuration steps in the device lifecycle, Intune provides capabilities to protect managed devices from unauthorized access and malicious attacks.

---

### Compliance Policies

Intune features **device compliance policies** that let you evaluate and in some cases remediate devices that don't meet your defined rules.

You can get reports on the following device characteristics before taking action:

- Jailbroken iOS/iPadOS devices
- Encrypted or unencrypted devices
- The health of Windows 10/11 devices (as determined by the **Health Attestation Service**)

You can also:
- Restrict use of hardware features, such as the **camera** or **Bluetooth**
- Configure **actions for noncompliance** (e.g., email the user, remotely lock, or retire the device)

---

### App and Data Protection

Intune provides a range of features to protect apps and the data they use through **Mobile Application Management (MAM) policies**:

| MAM Policy Capability | Description |
|---|---|
| Prevent backup from protected apps | Stops users from backing up work data to personal cloud services |
| Restrict copy and paste | Limits clipboard sharing between managed and unmanaged apps |
| Require a PIN to access an app | Forces authentication before opening protected apps |

---

### Additional Protection Layers

| Feature | Description |
|---|---|
| **Multifactor Authentication (MFA)** | Adds an extra layer of authentication (e.g., phone call or text message) to user sign-ins before granting access to devices and resources. |
| **Windows Hello for Business** | An alternative sign-in method that allows users to sign in using a **gesture** such as a fingerprint or facial recognition without needing a password. |
| **Conditional Access** | Ensures that only **trusted users** can access organizational resources on **trusted devices** using **trusted apps**. Conditional Access is a critical companion to device compliance policies. |

> **Note:** Conditional Access is a separate topic covered in more depth within the broader module.

---

### Software Updates

Effective software update management is essential for:
- Maintaining operational efficiency
- Overcoming security vulnerabilities
- Maintaining network infrastructure stability

> Because of the changing nature of technology and the continual appearance of new security threats, effective software update management requires **consistent and continual attention**.

#### Intune Software Updates

Intune helps secure managed computers by managing software updates — ensuring the latest patches and updates are quickly installed across devices.

#### Configuration Manager Software Updates

Configuration Manager provides a comprehensive set of tools and resources to help manage the complex task of tracking and applying software updates to client computers in larger enterprise environments.

<img width="578" height="590" alt="image" src="https://github.com/user-attachments/assets/f71a6d5a-fbe7-4962-a507-8b33b43f2071" />

---

## Retiring Devices

> **Estimated reading time:** 2 minutes  
> **Source:** [Unit 6 — Learn about retiring devices](https://learn.microsoft.com/en-us/training/modules/manage-devices-with-microsoft-endpoint-manager/6-retire-devices)

When a device is **lost, stolen, or needs to be replaced**, it is time to retire or wipe it. Intune supports several retirement methods.

> **Important:** Retiring a device from Intune **leaves the user's personal data on the device**. If full data removal is required, use **Wipe** instead.

### What the Retire Action Does

The **Retire** action performs the following:

- Removes **managed app data** (where applicable)
- Removes **settings** assigned by Intune
- Removes **email profiles** assigned by Intune
- Removes the device from **Intune management**

> **Note:** The actual retirement happens the **next time the device checks in** and receives the remote retire action. The device will still appear in Intune until it checks in. To remove stale devices immediately, use the **Delete** action instead.

### Noncompliance Actions

In Intune, you can configure **actions for noncompliance** using a time-ordered sequence:

1. Send an email to the end user notifying them of noncompliance
2. Remotely lock the device
3. Retire the noncompliant device

This automated workflow ensures devices that violate policy are systematically handled.

### Summary of Retire vs. Wipe vs. Delete

| Action | Effect on Personal Data | Effect on Corporate Data | Immediate? |
|---|---|---|---|
| **Retire** | Preserved on device | Removed | No — on next check-in |
| **Wipe** | Removed (factory reset) | Removed | Configurable |
| **Delete** | No action on device | No action on device | Yes — removes from Intune console immediately |

<img width="700" height="292" alt="image" src="https://github.com/user-attachments/assets/bfc1a36a-5edd-4dd4-a760-776903d7f29a" />


> **Note:** The source only explicitly describes Retire and Delete. Wipe behavior is implied by context but not detailed in these specific module units.

---

## Tools and Resources Used

| Tool / Resource | Description | Link |
|---|---|---|
| **Microsoft Intune** | Cloud-based MDM and MAM solution for managing devices and apps | [https://intune.microsoft.com](https://intune.microsoft.com) |
| **Microsoft Endpoint Manager** | Unified endpoint management platform (Intune + Configuration Manager) | [https://endpoint.microsoft.com](https://endpoint.microsoft.com) |
| **Windows Autopilot** | Modern provisioning tool for Windows devices | [Autopilot Docs](https://learn.microsoft.com/en-us/autopilot/windows-autopilot) |
| **Apple Business Manager (ABM)** | Apple's portal for enterprise device management and ADE | [https://business.apple.com](https://business.apple.com) |
| **Apple School Manager (ASM)** | Apple's portal for education device management | [https://school.apple.com](https://school.apple.com) |
| **Apple Automated Device Enrollment (ADE)** | Formerly Device Enrollment Program (DEP); allows zero-touch Apple device enrollment | [ADE Docs](https://learn.microsoft.com/en-us/mem/intune/enrollment/device-enrollment-program-enroll-ios) |
| **Apple Configurator** | USB-based tool for configuring and enrolling iOS/iPadOS devices | [Apple Configurator](https://learn.microsoft.com/en-us/mem/intune/enrollment/apple-configurator-enroll-ios) |
| **Android Enterprise Zero-Touch** | Google's zero-touch enrollment program for corporate Android devices | [Zero-Touch Docs](https://learn.microsoft.com/en-us/mem/intune/enrollment/android-dedicated-devices-fully-managed-enroll) |
| **Microsoft Configuration Manager (SCCM)** | On-premises endpoint management tool; used with co-management alongside Intune | [ConfigMgr Docs](https://learn.microsoft.com/en-us/mem/configmgr/) |
| **Company Portal App** | End-user app for device enrollment, app access, and self-service tasks | [Company Portal](https://learn.microsoft.com/en-us/mem/intune/user-help/company-portal-app) |
| **Windows Configuration Designer (WCD)** | Tool for creating provisioning packages for bulk Windows enrollment | [WCD Docs](https://learn.microsoft.com/en-us/windows/configuration/provisioning-packages/provisioning-install-icd) |
| **Microsoft Entra ID** | Cloud identity service used for MDM auto-enrollment and device join | [Entra ID Docs](https://learn.microsoft.com/en-us/entra/identity/) |
| **Windows Hello for Business** | Passwordless authentication using biometrics or PIN | [WHfB Docs](https://learn.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/) |
| **Conditional Access** | Policy engine ensuring only trusted users/devices access resources | [Conditional Access Docs](https://learn.microsoft.com/en-us/entra/identity/conditional-access/) |
| **Health Attestation Service** | Windows service that reports on device health for compliance evaluation | [Health Attestation](https://learn.microsoft.com/en-us/windows/security/hardware-security/tpm/tpm-fundamentals) |

---

## Confirmed and Applicable Scenarios with Detailed Solutions

The following are real-world IT scenarios that can be resolved using the concepts covered in this module, with detailed steps and applicable sources.

---

### Scenario 1 — A new employee receives a Windows laptop. IT wants zero-touch setup with no reimaging.

**Solution: Windows Autopilot — User Driven Mode**

**Steps:**
1. Purchase devices from an Autopilot-registered reseller, or manually upload hardware hash to Intune.
2. In **Intune Admin Center** → **Devices** → **Windows** → **Windows Enrollment** → **Deployment Profiles**, create a new Autopilot Deployment Profile (User Driven).
3. Assign the profile to a device group containing the new device.
4. Ship the device directly to the employee.
5. Employee powers on the device → Autopilot process launches automatically.
6. Employee signs in with their Microsoft Entra credentials → device configures itself, installs assigned apps, and applies policies.

**Sources:**
- [Windows Autopilot Overview](https://learn.microsoft.com/en-us/autopilot/windows-autopilot)
- [Set up Windows Autopilot in Intune](https://learn.microsoft.com/en-us/mem/intune/enrollment/enrollment-autopilot)

---

### Scenario 2 — The organization needs to enroll 500 Android devices at once for a warehouse deployment.

**Solution: Android Enterprise Zero-Touch Enrollment**

**Steps:**
1. Purchase zero-touch compatible Android devices from a supported reseller.
2. Register in the **Google Zero-Touch Portal** at [https://partner.android.com/zerotouch](https://partner.android.com/zerotouch).
3. Assign an enterprise DPC (Device Policy Controller) configuration pointing to Intune.
4. In Intune, configure an **Android Enterprise Fully Managed** enrollment profile.
5. Devices shipped to the warehouse — when powered on, they automatically enroll and receive the configuration.

**Sources:**
- [Android Enterprise Zero-Touch Enrollment](https://learn.microsoft.com/en-us/mem/intune/enrollment/android-dedicated-devices-fully-managed-enroll)
- [Android zero-touch for Intune](https://learn.microsoft.com/en-us/mem/intune/enrollment/android-dedicated-devices-fully-managed-enroll#enroll-by-using-google-zero-touch)

---

### Scenario 3 — A personal iPhone (BYOD) needs access to corporate email without IT managing the whole device.

**Solution: iOS BYOD Enrollment with MAM App Protection Policies**

**Steps:**
1. User downloads the **Company Portal app** from the App Store.
2. User opens Company Portal and signs in with their corporate Microsoft Entra credentials.
3. User follows the prompts to enroll the device.
4. In Intune, assign an **App Protection Policy** (MAM) to the user targeting Outlook/Teams.
5. Policies enforce: require PIN, prevent copy-paste to personal apps, block backup of work data.

**Note:** For stricter BYOD scenarios, consider MAM-without-enrollment (MAM-WE), which applies app protection without full device enrollment.

**Sources:**
- [iOS BYOD Enrollment](https://learn.microsoft.com/en-us/mem/intune/enrollment/ios-enroll)
- [App Protection Policies Overview](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy)

---

### Scenario 4 — A device is reported lost or stolen. IT needs to protect corporate data immediately.

**Solution: Remote Retire or Wipe via Intune**

**Steps:**
1. In **Intune Admin Center** → **Devices** → **All Devices**, locate the device.
2. Select the device → **Retire** (removes corporate data; preserves personal data) OR **Wipe** (factory reset; removes everything).
3. If the device never checks in and needs to be removed from the console: select **Delete**.
4. Optionally, also trigger **Remote Lock** or **Reset Passcode** as interim steps.

> ⚠️ Retire only takes effect when the device next checks in. Use **Wipe** for immediate data removal on lost/stolen corporate devices.

**Sources:**
- [Remove devices from Intune](https://learn.microsoft.com/en-us/mem/intune/remote-actions/devices-wipe)
- [Intune Remote Actions](https://learn.microsoft.com/en-us/mem/intune/remote-actions/device-management)

---

### Scenario 5 — A Windows device fails a compliance check (e.g., no BitLocker encryption). IT needs to automate the response.

**Solution: Intune Compliance Policy with Noncompliance Actions**

**Steps:**
1. In **Intune Admin Center** → **Devices** → **Compliance Policies**, create a new policy for Windows 10/11.
2. Set a rule: **Require BitLocker** = Enabled.
3. Under **Actions for noncompliance**, configure a time-ordered sequence:
   - Day 0: Mark device as noncompliant.
   - Day 1: Send email notification to the user.
   - Day 3: Remotely lock the device.
   - Day 7: Retire the device.
4. Assign the policy to the target device group.

**Sources:**
- [Intune Compliance Policies](https://learn.microsoft.com/en-us/mem/intune/protect/device-compliance-get-started)
- [Actions for Noncompliance](https://learn.microsoft.com/en-us/mem/intune/protect/actions-for-noncompliance)

---

### Scenario 6 — A school needs to deploy 300 iPads to students with preset configurations using ADE.

**Solution: Apple Automated Device Enrollment (ADE) via Apple School Manager**

**Steps:**
1. Purchase iPads through an Apple Authorized Reseller and link them to **Apple School Manager (ASM)**.
2. In Intune, navigate to **Devices** → **iOS/iPadOS** → **iOS/iPadOS Enrollment** → **Enrollment Program Tokens**.
3. Download the Intune public key, upload it to ASM, and download the ADE token from ASM.
4. Upload the ADE token to Intune.
5. Create an **Enrollment Profile** in Intune and assign it to the devices in ASM.
6. Ship devices to students — they auto-enroll when powered on and Setup Assistant runs.

**Sources:**
- [Apple ADE Enrollment for iOS](https://learn.microsoft.com/en-us/mem/intune/enrollment/device-enrollment-program-enroll-ios)
- [Apple School Manager](https://support.apple.com/en-us/guide/apple-school-manager/welcome/web)

---

### Scenario 7 — IT needs to ensure only compliant, trusted devices can access Microsoft 365 resources.

**Solution: Conditional Access + Intune Compliance Policies**

**Steps:**
1. Create an **Intune Compliance Policy** defining minimum standards (e.g., OS version, encryption, no jailbreak).
2. In **Microsoft Entra ID** → **Security** → **Conditional Access**, create a new policy.
3. Set **Cloud Apps** = Microsoft 365 (Exchange Online, SharePoint, Teams, etc.).
4. Set **Conditions** → **Device Platforms** and **Device State**.
5. Set **Grant** = Require device to be marked as compliant.
6. Enable the policy.

Now, any device that fails the Intune compliance check will be blocked from accessing Microsoft 365.

**Sources:**
- [Conditional Access with Intune](https://learn.microsoft.com/en-us/mem/intune/protect/conditional-access)
- [Require compliant device with Conditional Access](https://learn.microsoft.com/en-us/entra/identity/conditional-access/howto-conditional-access-policy-compliant-device)

---
