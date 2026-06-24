# Protect Endpoints with Microsoft Endpoint Manager (Microsoft Intune)
---

## 1. Managing and Protecting Devices

You can create and apply **device policies** as part of your efforts to protect endpoints. Microsoft Intune helps you protect the devices you manage and the data stored on those devices. Protected devices are also known as **managed devices**.

Intune includes settings and features you can enable or disable for different devices within your organization. These settings and features are added to **configuration profiles**. You can create profiles for different devices and different platforms, including:

- iOS/iPadOS
- macOS
- Android
- Android Enterprise
- Windows

You then use Intune to **apply** (or "assign") the profile to the devices.

<img width="837" height="589" alt="image" src="https://github.com/user-attachments/assets/20caacad-5f43-43f6-9a16-3702460428b2" />

---

### Types of Device Policies

Device policies allow you to perform several different types of actions to protect devices:

| Action | Description |
|---|---|
| **Restrict** | Limit hardware features or app capabilities |
| **Reset** | Reset passcodes or device states |
| **Require** | Enforce compliance requirements (e.g., PIN) |
| **Configure** | Define settings for compliant/noncompliant apps |
| **Protect** | Protect apps and the data they use |
| **Control** | Control sign-in methods (e.g., Windows Hello for Business) |
| **Retire** | Remove devices and wipe data |

---

### Examples of Device Configuration Policies

Intune configuration policies help you protect and configure devices by allowing you to control a wide range of settings and features. For example, you can:

- **Restrict** the use of hardware features on the device, such as the camera or Bluetooth.
- **Reset** the passcodes when users are locked out of their devices.
- **Require** devices to be compliant with your organization's protection requirements, such as requiring each device to use a PIN to access the device.
- **Configure** compliant and noncompliant apps. You get an alert if a noncompliant app is installed (some platforms can also block the install entirely).
- **Protect** apps and the data they use.
- **Protect** devices based on identity by adding an extra layer of protection.
- **Control** the settings for **Windows Hello for Business**, which is an alternative sign-in method for Windows 10 and later.
- **Retire** devices and remove data.
- **Configure email** — allow end users to access company email on their personal devices without any required setup on their part.

> 📌 **Note:** Configuration profiles are created per platform. One profile cannot be shared across different OS families.

---

## 2. Managing and Protecting Apps

Microsoft Intune provides a range of features to help you manage and protect apps and data. Intune supports **mobile application management (MAM)** policies **without requiring device enrollment**.

> ⚠️ **Important:** Intune MAM capabilities are supported whether you choose to enroll the device or not.

---

### MAM Configurations

Intune MAM supports two primary configurations:

#### Configuration 1 — MDM + MAM (Enrolled Devices)

- IT administrators manage apps using **MAM and app-protection policies** on devices **enrolled** with Intune MDM.
- Best for corporate-owned or company-managed devices.

#### Configuration 2 — MAM Without Device Enrollment (MAM-WE)

- IT administrators manage apps using MAM and app-protection policies on devices **not enrolled** with Intune MDM.
- Supports devices enrolled with **third-party enterprise mobility management (EMM)** providers.
- Supports **Bring Your Own Device (BYOD)** scenarios.

> 📌 **Key takeaway:** Even on devices your organization doesn't manage at the OS level, Intune can still manage and protect the apps on those devices.

---

### Examples of App Policies

By using MAM policies in Intune, you can configure, secure, publish, monitor, and update apps for devices and users of your organization. For example, you can:

- Add and assign apps to devices and users.
- Assign apps to devices **not enrolled** with Intune.
- Use **app-configuration policies** to control app startup behavior.
- **Prevent data** from being backed up from a protected app.
- **Restrict copy and paste** actions to other apps.
- **Require a PIN** to access an app.

---

## 3. Managing and Protecting Your Organization's Data

Using Intune, you can prevent data leaks and prevent unauthorized access to your organization's data.

---

### Prevent Data Leaks

You can control how users share and save data without risking intentional or accidental data leaks. Microsoft Intune provides **app-protection policies** that you set to secure your company data on user-owned devices. The devices **do not need to be enrolled** in the Intune service.

App-protection policies configured with Intune also work on **devices managed with a non-Microsoft device-management solution**. The personal data on the devices is never touched; the IT department only manages company data.

You can set app-protection policies for **Office mobile apps** on devices running:

- Windows
- iOS/iPadOS
- Android

These policies enforce device requirements such as:

- An **app-based PIN**
- **Company data encryption**
- Advanced settings to restrict **cut, copy, paste, and save-as** features between managed and unmanaged apps

You can also **remotely wipe company data** without requiring that users enroll their devices.

---

### Prevent Unauthorized Access

You can classify, label, and protect **Office 365 documents and emails** so only authorized users have access to the data.

- Settings are managed automatically after IT administrators or users set the rules and conditions.
- The IT team can provide recommended settings for users to follow.
- Administrators and users can **revoke access** to data already shared with others without assistance from another authority.
- The goal is to control who opens or updates protected data — **even when the data leaves the company's network**.

---

## 4. Understanding the Endpoint Environment

Microsoft Intune is used to manage your organization's **endpoints**. Endpoints include:

- Mobile devices
- Desktop computers
- Virtual machines
- Embedded devices
- Servers
- Apps used by your organization

These endpoints are managed in different environments based on their location. Microsoft Intune manages **three primary endpoint environments**:

---

### Cloud Endpoint Management

You can manage devices, apps, and data using a cloud-based **MDM and MAM** service such as Microsoft Intune.

Intune integrates with:

| Service | Purpose |
|---|---|
| **Microsoft 365** | Enable workforce productivity on all devices |
| **Microsoft Entra ID** | Control who has access and what they access |
| **Azure Information Protection** | Protect organizational data |

When you use Intune with Microsoft 365, you can enable your workforce to be productive on all their devices while keeping your organization's information protected.

---

### On-Premises Endpoint Management

With an on-premises endpoint-management solution, you can:

- Manage on-premises **Windows 10/11** devices, apps, and data.
- **Optimize downloads and content**.
- Make your environment more secure by restricting access and location.

> ⚠️ **Warning:** If you only use an on-premises solution, **remote devices are not protected** as corporately recognized endpoints. End users will not be able to access company apps and data using remote devices.

> 💡 **Recommendation:** If you use **Configuration Manager**, you should attach your Configuration Manager deployment to the **Microsoft 365 cloud (cloud attach)**, which provides integration with Intune, Microsoft Entra ID, Microsoft Defender for Endpoint, and other cloud services.

---

### Cloud + On-Premises Endpoint Management

For Configuration Manager managed devices, data can also flow to Microsoft Endpoint Management through the **ConfigMgr connector**.

- The ConfigMgr connector is attached to the cloud via **tenant attach**.
- Tenant attach requires a connection to an **Intune tenant**, but **does not** require turning on co-management.

---

### Co-Managed Endpoint Management

**Co-management** is the concurrent management of Windows 10/11 devices with both **Configuration Manager** and **Microsoft Intune**.

- Combines your existing **on-premises Configuration Manager** investment with the cloud.
- Uses Intune and other **Microsoft 365 cloud services**.
- You choose whether **Configuration Manager or Intune** is the management authority.
- Some tasks remain on-premises while other tasks run in the cloud with Intune.

---

## 5. Platform Considerations

Microsoft Intune supports several device platforms. When creating your endpoint-management design, you must:

1. Determine the **platforms** that must be supported in your endpoint environment.
2. Verify whether **Intune supports each platform**.

Supported platforms include:

- **iOS/iPadOS**
- **Windows**
- **Android**
- **macOS**

> 📌 Each device type offers slightly different **enrollment methods**, **device settings**, **protection policy settings**, **configuration policy settings**, **custom policy settings**, and **remote actions**.

---

### Intune Supported Operating Systems

#### Apple

- Apple iOS **16.0 and later**
- Apple iPadOS **16.0 and later**
- macOS X **13.0 and later**

#### Google

- Android **8.0 and later** (including Samsung KNOX Standard 3.0 and higher)
- **Android Enterprise**

#### Microsoft

| OS Version | Notes |
|---|---|
| Windows 11 | Home, S, Pro, Education, Enterprise editions |
| Windows 10 | Home, S, Pro, Education, Enterprise versions |
| Windows 10 & 11 Cloud PCs | On Windows 365 |
| Windows 10 Enterprise 2021 LTSC | Long-Term Servicing Channel |
| Windows 10 Enterprise 2019 LTSC | Long-Term Servicing Channel |
| Windows 10 IoT Enterprise | x86, x64 |
| Windows 10 Teams | Surface Hub |
| Windows Holographic for Business | HoloLens |
| Surface Hub | — |
| Windows 10 version 1709 (RS3) and later | — |
| Windows 8.1 RT | Sustaining mode |
| PCs running Windows 8.1 | Sustaining mode |

> 📌 For the full and most current list, see: [Supported operating systems and browsers in Intune](https://learn.microsoft.com/en-us/mem/intune/fundamentals/supported-devices-browsers)

---

### Configuration Manager Supported Operating Systems

Configuration Manager supports several dozen OS versions for clients and devices:

#### Windows Computers

- Windows 10/11 (x86, x64, Arm64): Enterprise, Pro, Education, Pro Education, Pro for Workstation, Windows Insider
- Windows 10 Enterprise 2015 LTSB
- Windows 10 Enterprise 2016 LTSB
- Windows 10 Enterprise LTSC 2019
- Windows 8.1 (x86, x64): Professional, Enterprise

#### Other

- **Windows Servers** — several variations available
- **Windows Server Core** — several variations available
- **Azure Virtual Desktop**
- **Windows Embedded computers** — several variations available
- **Windows 10 IoT Mobile Enterprise**
- **Windows 10 Team for Surface Hub**

> 📌 For the full list, see: [Supported OS versions for clients and devices for Configuration Manager](https://learn.microsoft.com/en-us/mem/configmgr/core/plan-design/configs/supported-operating-systems-for-clients-and-devices)

---

## Tools and Resources Used

| Tool / Service / Resource | Description | Link |
|---|---|---|
| **Microsoft Intune** | Cloud-based MDM and MAM service | [Microsoft Intune Overview](https://learn.microsoft.com/en-us/mem/intune/fundamentals/what-is-intune) |
| **Microsoft Endpoint Manager Admin Center** | Central portal for managing Intune and Configuration Manager | [https://endpoint.microsoft.com](https://endpoint.microsoft.com) |
| **Microsoft Configuration Manager** | On-premises endpoint management solution | [Configuration Manager Docs](https://learn.microsoft.com/en-us/mem/configmgr/) |
| **Microsoft Entra ID** (formerly Azure AD) | Identity and access management | [Microsoft Entra Docs](https://learn.microsoft.com/en-us/entra/identity/) |
| **Azure Information Protection** | Data classification and protection | [AIP Docs](https://learn.microsoft.com/en-us/azure/information-protection/) |
| **Microsoft 365** | Productivity suite integrated with Intune | [M365 Docs](https://learn.microsoft.com/en-us/microsoft-365/) |
| **Microsoft Defender for Endpoint** | Threat protection integrated via cloud attach | [Defender Docs](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/) |
| **Windows Hello for Business** | Alternative sign-in method for Windows 10/11 | [Windows Hello Docs](https://learn.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/) |
| **Office 365 Apps (Mobile)** | Office apps that support app-protection policies | [Office Mobile Apps](https://www.microsoft.com/en-us/microsoft-365/mobile) |
| **MAM (Mobile Application Management)** | Manage apps without full device enrollment | [MAM Docs](https://learn.microsoft.com/en-us/mem/intune/apps/app-management) |
| **MDM (Mobile Device Management)** | Full device management via enrollment | [MDM Docs](https://learn.microsoft.com/en-us/mem/intune/enrollment/) |
| **ConfigMgr Connector / Tenant Attach** | Connects on-premises Configuration Manager to Intune cloud | [Tenant Attach Docs](https://learn.microsoft.com/en-us/mem/configmgr/tenant-attach/) |
| **Supported OS for Intune** | Full list of supported devices and browsers | [Intune Supported OS](https://learn.microsoft.com/en-us/mem/intune/fundamentals/supported-devices-browsers) |
| **Supported OS for ConfigMgr** | Full list of supported client OS for Configuration Manager | [ConfigMgr Supported OS](https://learn.microsoft.com/en-us/mem/configmgr/core/plan-design/configs/supported-operating-systems-for-clients-and-devices) |

---

## Confirmed Scenarios, Detailed Solutions, and Applicable Sources

---

### ✅ Scenario 1 — BYOD Employee Accessing Corporate Email on a Personal (Unenrolled) Phone

**Situation:** An employee uses a personal iPhone that is not enrolled in Intune. The organization needs to ensure corporate email and data are protected on that device.

**Solution:** Use **MAM-WE (MAM Without Enrollment)** with an **App Protection Policy** targeting the Outlook app.

**Detailed Steps:**

1. In the **Microsoft Endpoint Manager Admin Center** (`endpoint.microsoft.com`), go to **Apps > App protection policies**.
2. Click **+ Create policy**, select **iOS/iPadOS**.
3. Name the policy (e.g., `BYOD-Outlook-Protection`).
4. Under **Apps**, select **Microsoft Outlook**.
5. Under **Data protection**, configure:
   - **Prevent backups** → Enabled
   - **Restrict cut, copy, paste** → Policy managed apps only
   - **Save copies of org data** → Block
6. Under **Access requirements**, configure:
   - **PIN for access** → Require
   - **Work or school account credentials for access** → Require
7. Under **Conditional launch**, set minimum OS version requirements.
8. **Assign** the policy to the target user group.
9. Users install **Outlook** from the App Store; upon signing in with their corporate account, the MAM policy is automatically applied.

**Applicable Sources:**
- [App protection policies overview](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy)
- [MAM without enrollment](https://learn.microsoft.com/en-us/mem/intune/apps/mam-faq)

---

### ✅ Scenario 2 — Restricting Camera Use on Corporate Android Devices

**Situation:** A healthcare organization needs to disable camera access on all company-owned Android devices to protect patient privacy.

**Solution:** Create a **Device Configuration Profile** in Intune with a **device restriction** policy targeting the camera.

**Detailed Steps:**

1. In the **Endpoint Manager Admin Center**, go to **Devices > Configuration profiles**.
2. Click **+ Create profile**.
3. Select **Platform: Android Enterprise** (or Android device administrator, depending on enrollment type).
4. Select **Profile type: Device restrictions**.
5. Navigate to **General** settings.
6. Set **Camera** → **Block**.
7. Optionally set **Bluetooth** → **Block** if required.
8. **Assign** the profile to the corporate device group.
9. After the policy syncs, the camera will be disabled on all targeted devices.

**Applicable Sources:**
- [Android device restriction settings in Intune](https://learn.microsoft.com/en-us/mem/intune/configuration/device-restrictions-android-for-work)
- [Create device configuration profiles](https://learn.microsoft.com/en-us/mem/intune/configuration/device-profile-create)

---

### ✅ Scenario 3 — Preventing Data Leakage From Managed Apps to Personal Apps

**Situation:** Employees are copying corporate data from Word or Teams into personal apps such as WhatsApp or personal Notes apps on their iPhones.

**Solution:** Configure an **App Protection Policy** with **cut/copy/paste restrictions** between managed and unmanaged apps.

**Detailed Steps:**

1. Go to **Apps > App protection policies** in the Endpoint Manager Admin Center.
2. Create or edit an existing policy for **iOS/iPadOS**.
3. Under **Data protection > Restrict cut, copy, and paste between other apps**, select:
   - **Policy managed apps** — Only allow cut/copy/paste within other Intune-managed apps.
4. Under **Data protection > Send org data to other apps**, select:
   - **Policy managed apps** — Only allow data to be sent to other managed apps.
5. Under **Data protection > Receive data from other apps**, select:
   - **Policy managed apps** — Only allow data from other managed apps.
6. Enable **Encrypt org data** → **Require**.
7. Assign to the relevant user group.
8. Deploy policy; the next time the user opens a managed app, the restrictions take effect.

**Applicable Sources:**
- [Data protection settings in iOS app protection policies](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy-settings-ios)
- [How to create and assign app protection policies](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policies)

---

### ✅ Scenario 4 — Transitioning from On-Premises Configuration Manager to Co-Management

**Situation:** An organization uses on-premises Configuration Manager but wants to extend management to remote workers and enable Intune cloud features without decommissioning their existing infrastructure.

**Solution:** Enable **Co-management** by attaching Configuration Manager to Microsoft Intune (cloud attach and co-management).

**Detailed Steps:**

1. Ensure **Configuration Manager version 1906 or later** is installed.
2. In the Configuration Manager console, go to **Administration > Cloud Services > Cloud Attach**.
3. Run the **Cloud Attach Configuration Wizard**.
4. Sign in with your **Microsoft 365 Global Admin** credentials.
5. Select **Upload to Microsoft Endpoint Manager admin center** (Tenant Attach).
6. Optionally enable **Co-management** to split workloads between ConfigMgr and Intune.
7. In the **Co-management Configuration Wizard**, choose which workloads to shift to Intune (e.g., Compliance policies, Resource access policies, Endpoint Protection).
8. Set the **Co-management enrollment** to pilot or all devices.
9. Verify devices appear in the Endpoint Manager Admin Center under **Devices > All devices**.

> ⚠️ **Warning:** Tenant attach does not require co-management to be turned on. These are separate features that can be independently enabled.

**Applicable Sources:**
- [What is co-management?](https://learn.microsoft.com/en-us/mem/configmgr/comanage/overview)
- [How to enable co-management](https://learn.microsoft.com/en-us/mem/configmgr/comanage/how-to-enable)
- [Tenant attach: Device sync and actions](https://learn.microsoft.com/en-us/mem/configmgr/tenant-attach/device-sync-actions)

---

### ✅ Scenario 5 — Remotely Wiping Company Data From a Lost or Stolen Personal Device

**Situation:** An employee reports their personal iPhone (BYOD, not MDM-enrolled) lost or stolen. It has Outlook and Teams installed with corporate data.

**Solution:** Use Intune's **selective wipe** capability, which removes only corporate data from managed apps without wiping the personal device.

**Detailed Steps:**

1. In the **Endpoint Manager Admin Center**, go to **Apps > App selective wipe**.
2. Click **+ Create wipe request**.
3. Select the **user** whose device needs to be wiped.
4. Confirm the device and apps targeted.
5. Click **Create** to trigger the selective wipe.
6. The next time the device connects to the internet, all corporate data is removed from Intune-managed apps; personal data and apps are unaffected.

> ⚠️ **Note:** Selective wipe only removes corporate data from apps protected by Intune MAM policies. For enrolled devices, you can perform a **full device wipe** or **retire** action instead.

**Applicable Sources:**
- [How to wipe only corporate data from Intune-managed apps](https://learn.microsoft.com/en-us/mem/intune/apps/apps-selective-wipe)
- [Retire or wipe devices](https://learn.microsoft.com/en-us/mem/intune/remote-actions/devices-wipe)

---

### ✅ Scenario 6 — Enforcing Windows Hello for Business on Windows 11 Devices

**Situation:** An organization wants to replace passwords with biometric or PIN-based sign-in for all Windows 11 endpoints to improve security posture.

**Solution:** Create a **Windows Hello for Business** policy via Intune.

**Detailed Steps:**

1. In the **Endpoint Manager Admin Center**, go to **Devices > Windows > Windows enrollment > Windows Hello for Business**.
2. Click **Properties** and set **Configure Windows Hello for Business** → **Enabled**.
3. Configure:
   - **Minimum PIN length** → 6 (recommended)
   - **Maximum PIN length** → 127
   - **Lowercase/Uppercase/Special characters in PIN** → as per policy
   - **PIN expiration** → optional
   - **PIN history** → optional
   - **Biometric authentication** → Allowed
   - **Use enhanced anti-spoofing** → Enabled (recommended)
4. Click **Save**.
5. The policy applies to all enrolled Windows devices at next check-in.

**Applicable Sources:**
- [Windows Hello for Business settings in Intune](https://learn.microsoft.com/en-us/mem/intune/protect/identity-protection-windows-settings)
- [Windows Hello for Business overview](https://learn.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/)

---
