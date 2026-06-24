# Enrolling Devices with Microsoft Intune — Complete Reference Guide

## 1. Manage Mobile Devices with Intune

### Intune Admin Center

Intune administration is accessed via the **Intune admin center** at:

```
https://intune.microsoft.com
```

This console includes all Intune management capabilities, as well as common tasks such as managing users and groups. It replaces the legacy Intune console previously found in the Azure portal.

### Intune Company Portal

The **Company Portal** is available as:

- A **web application** at `https://portal.manage.microsoft.com/`
- An **application for desktops and mobile devices** on Windows, iOS, and Android

Users use the Company Portal to:
- Self-manage device enrollment
- Access applications published by their company administrator

### Device Management Lifecycle

Managing mobile devices follows a four-phase lifecycle:

| Phase | Description |
|---|---|
| **Enroll** | Devices register with the MDM solution. Intune can enroll both mobile devices (phones, tablets) and Windows PCs. |
| **Configure** | Ensures enrolled devices are secure and comply with configuration or security policies. Common tasks (e.g., Wi-Fi setup) can be automated. |
| **Protect** | Provides ongoing monitoring of the settings established in the Configure phase. Includes deploying software updates to maintain compliance. |
| **Retire** | When a device is no longer needed, lost, or stolen, data is protected by resetting the device (full wipe) or removing only corporate-owned data (selective wipe). |

---

## 2. Enable Mobile Device Management

> **Source Unit:** [Unit 3](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/3-enable-mobile-device-management) · 3 min read

### What is MDM?

Mobile Device Management (MDM) is an **industry-standard** approach for managing mobile devices such as smartphones, tablets, laptops, and desktop computers. As a modern desktop administrator, MDM is essential for scenarios where on-premises solutions cannot manage devices effectively or where MDM policies are better suited than traditional methods like Group Policy.

> **Clarification:** The term "Mobile Device Manager" originated from managing personal phones and tablets. In modern management, the form factor (desktop, kiosk) is not a determining factor — MDM is the unified solution for all devices.

### Activate MDM Services

To support enrollment of mobile devices, the **MDM Authority** must be set within Intune configuration.

**Location:** Device Enrollment section → Microsoft Intune Overview dashboard

**Steps:**
1. Navigate to the Intune portal.
2. Go to **Device Enrollment**.
3. The default is set to **None**. Enable MDM by choosing the **Intune MDM Authority** option.

> ⚠️ **Important Note:** Hybrid MDM with Configuration Manager is **retired**. While this option may still be visible for legacy implementations, new connections to Configuration Manager cannot be created. This should not be confused with **Co-Management**, which is different from Hybrid MDM and still uses the Intune MDM Authority option.

### Configure Intune for Apple Device Support

By default, Intune is configured to allow enrollment of:
- ✅ Windows devices
- ✅ Android devices
- ✅ Samsung Knox Standard devices

To manage **iOS and macOS devices**, an **Apple MDM Push Certificate** is required.

**Configuration Steps:**

1. Go to the Intune portal.
2. Navigate to **Device enrollment** → **Apple Enrollment** → **Apple MDM Push Certificate**.
3. **Download** a Certificate Signing Request (CSR).
4. Navigate to the **Apple Certificate Portal** (link provided in the Intune interface).
5. **Create a certificate** by uploading the CSR and downloading the generated certificate file.
6. Return to the Intune Portal and **upload the certificate file**.

> ⚠️ **Warning:** Generating a certificate in the Apple Certificate Portal requires an Apple ID that is a **member of the Apple Developer program**. This should be a company-authorized account — **never use a personal Apple ID**.

---

## 3. Considerations for Device Enrollment

> **Source Unit:** [Unit 4](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/4-explain-considerations-device-enrollment) · 7 min read

### Enrollment Methods by Device Type

| Scenario | Method |
|---|---|
| Device already joined to on-premises AD DS | Use Group Policy to automatically enroll to MDM |
| Join Windows device to Microsoft Entra ID | Device is automatically enrolled to MDM |
| Manual enrollment (Windows) | Settings app, provisioning packages, or Company Portal app |
| Android and iOS devices | Manual enrollment via Company Portal app only |

> **Note:** Automatic enrollment to MDM works **only for Windows devices**, because only Windows devices can be joined to on-premises AD DS and Microsoft Entra ID. Android and iOS devices can only be enrolled manually.

For iOS enrollment, you must configure MDM with a valid **Apple Push Notification (APN) certificate**. This applies to iPhones, iPads, and macOS devices regardless of the MDM product used.

For more details: [Enable Windows device automatic enrollment](https://learn.microsoft.com/en-us/intune/intune-service/user-help/enroll-windows-10-device)

### Supported Devices

Intune supports device enrollment for the following operating systems:

| Platform | Supported Versions |
|---|---|
| **Windows** | Windows 10/11 (Home, Pro, Education, S mode, Enterprise) |
| **Windows 365** | Windows 10/11 Cloud PCs |
| **Windows IoT/Holographic** | Windows 10 IoT, Windows 10 Holographic |
| **Windows LTSC** | Windows 10 2019 LTSC |
| **Surface Hub** | Surface Hub, Windows 10 Teams (Surface Hub) |
| **Apple iOS/iPadOS** | 14.0 and later |
| **macOS** | 11.0 and later |
| **Android** | 8.0 and later, including Samsung KNOX Standard 3.0+ |
| **Linux** | Ubuntu Desktop 20.04 or 22.04 LTS (x86/64) |
| **Chrome OS** | Supported |

> **Note:** Intune offered a software client for legacy operating systems. This is not required on current supported operating systems.

### Define Allowed Devices — Enrollment Restrictions

By default, all users with an Intune license may enroll supported device types. Administrators can configure enrollment restrictions including:

- **Maximum number of devices per user** (default: **5 devices per user**)
- **Device platforms** that can be enrolled
- **Required OS version** for iOS, Android, Android work profile, and Windows:
  - Minimum version
  - Maximum version
- **Restrict personal device enrollment** (configurable for iOS, Android, Android work profile, macOS, and Windows 10/11)

### Enrollment Configuration Options

| Option | Description |
|---|---|
| **Terms and Conditions** | Require users to accept company T&Cs before using the Company Portal |
| **Enrollment Restrictions** | Control which device types can be enrolled, block personal devices, and limit enrollment count per user |
| **Enable Apple Device Enrollment** | Control whether Apple devices can be enrolled (requires APN certificate) |
| **Corporate Identifiers** | List IMEI numbers and serial numbers to identify company-owned devices |
| **Multifactor Authentication** | Require additional verification (phone, PIN, biometric) during enrollment |
| **Device Enrollment Manager (DEM)** | Special account that can enroll up to **1,000 devices**, bypassing per-user device limits |

### Ensuring Users Enroll Their Devices

To enforce device enrollment, organizations can configure:

- A **Security policy in Microsoft 365**, or
- A **Conditional Access policy in Intune**

These policies allow access to company resources **only from enrolled devices**. If an unenrolled device attempts to access resources (e.g., Exchange Online mailbox), the user is denied access and redirected to enroll first.

For Windows-only automatic enrollment via Group Policy:

> Enable the **"Enable automatic MDM enrollment using default Microsoft Entra credentials"** Group Policy setting for devices already joined to on-premises AD DS synced to Microsoft Entra ID.

---

## 4. Manage Corporate Enrollment Policy

> **Source Unit:** [Unit 5](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/5-manage-corporate-enrollment-policy) · 3 min read

### Custom Domain Configuration

When signing up for Intune, your organization receives an initial domain:

```
your-domain.onmicrosoft.com
```

> ⚠️ **Best Practice:** Before creating user accounts or synchronizing on-premises Active Directory, **add one or more custom domain names**. This simplifies user management and allows users to sign in with their familiar credentials. The `onmicrosoft.com` domain should be used only for initial setup or testing. It cannot be renamed or removed.

### Add and Verify a Custom Domain

1. Go to the **Microsoft 365 admin center** and sign in with your administrator account.
2. In the navigation pane, choose **Setup** → **Domains**.
3. Choose **Add domain** and type your custom domain name. Select **Next**.
4. The **Verify domain** dialog box opens with values for creating a **TXT record** in your DNS hosting provider:
   - **GoDaddy users:** Microsoft 365 admin center redirects you to GoDaddy's login page. After entering credentials and accepting the domain change permission agreement, the TXT record is created automatically.
   - **Register.com users:** Follow the step-by-step instructions to create the TXT record.

### Configure Automatic MDM Enrollment

Automatic enrollment lets users enroll their Windows devices in Intune by adding their work account or joining to Microsoft Entra ID.

**Steps:**

1. Sign in to [Microsoft Intune admin center](https://intune.microsoft.com).
2. Select **Devices** → **Enrollment** → **Automatic enrollment**.
3. Configure the **MDM User scope**:
   - **None** — MDM automatic enrollment is disabled
   - **Some** — Select specific groups that can automatically enroll Windows devices
   - **All** — All users can automatically enroll Windows devices
4. Use the default values for:
   - MDM Terms of use URL
   - MDM Discovery URL
   - MDM Compliance URL
5. Select **Save**.

### Simplify Manual Enrollment via CNAME (Optional)

If you don't have Microsoft Entra ID P1 or P2, you can create a DNS alias (CNAME record) to redirect enrollment requests to Intune servers. Without a CNAME record, users are prompted to manually enter the MDM server name: `enrollment.manage.microsoft.com`.

#### Step 1: Create CNAME Records

For a company with domain `contoso.com`, create:

| Type | Host Name | Points To | TTL |
|---|---|---|---|
| CNAME | `EnterpriseEnrollment.contoso.com` | `EnterpriseEnrollment-s.manage.microsoft.com` | 1 hour |
| CNAME | `EnterpriseRegistration.contoso.com` | `EnterpriseRegistration.windows.net` | 1 hour |

> If your company uses **more than one UPN suffix**, create two CNAME records for each domain name pointing to `EnterpriseEnrollment-s.manage.microsoft.com` and `EnterpriseRegistration.windows.net` respectively.

#### Step 2: Verify CNAME

1. Sign in to [Microsoft Intune admin center](https://intune.microsoft.com).
2. In the left navigation, select **Devices** → **Enrollment**.
3. On the Windows enrollment page, select **CNAME Validation**.
4. In the **Domain** box, enter the company website and select **Test**.

> ⚠️ **Note:** DNS changes may take **up to 72 hours** to propagate. You cannot verify the DNS change in Intune until the DNS record propagates.

---

## 5. Enroll Windows Devices in Intune

> **Source Unit:** [Unit 6](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/6-enroll-windows-devices-intune) · 7 min read

Enrolling Windows devices establishes a connection between the device and Intune, enabling remote management, policy delivery (email, Wi-Fi, security settings), and data protection.

### Windows Enrollment Methods

There are **8 enrollment methods** for Windows devices:

---

#### Method 1: Add Work or School Account

**Type:** User-driven  
**Description:** Microsoft Entra joins the device. If Microsoft Entra ID P1 or P2 licenses are configured with auto-enrollment, the device is also enrolled in Intune.

- **When to use:** Preferred when Autopilot is not used.
- **How:** Provide users with instructions to access **"Set up a work or school account"** from the **Settings** app.

---

#### Method 2: Enroll Only in Device Management (User Driven)

**Type:** User-driven  
**Description:** Enrolls the device in Intune **without** Microsoft Entra joining it.

- **When to use:** Environments without Microsoft Entra ID P1 or P2 licenses.

---

#### Method 3: Microsoft Entra Join (OOBE)

**Type:** User-driven (at setup)  
**Description:** Same as Method 1, but device is enrolled during the **Out of Box Experience (OOBE)**, not from the Settings app. The user chooses **Setup for an organization** and signs in with a Work Account.

- **When to use:** Remote offices where devices are delivered directly with Windows pre-installed (typically Windows Pro). The version is typically uplifted to Windows Enterprise via an Intune profile setting.

---

#### Method 4: Microsoft Entra Join (Autopilot – User-Driven Mode)

**Type:** IT-configured, user-driven  
**Description:** Enrolls during customized OOBE. Many OOBE screens can be skipped for a smoother setup experience. Desktop is shown only after apps and policies are applied.

- **When to use:** Preferred enrollment method. **Requires Microsoft Entra ID P1 or P2 licenses** with auto-enrollment configured.

---

#### Method 5: Microsoft Entra Join (Autopilot Self-Deploying Mode)

**Type:** Fully automated  
**Description:** All OOBE screens are skipped after first power-on. Microsoft Entra join and Intune enrollment are **fully automated with no user interaction**.

- **When to use:** User-less devices (kiosks). Can also be used for normal users by pre-assigning a user (user only needs to supply a password). Most streamlined setup experience.

---

#### Method 6: Enroll in MDM Only (Device Enrollment Manager)

**Type:** IT admin-driven  
**Description:** Performed by IT admins using a **Device Enrollment Manager (DEM) account**. Devices are enrolled and prepared before handing them to users. The DEM enrolls devices, logs on to the Company Portal, and installs required apps. **Can enroll up to 1,000 devices**.

- **Requirement:** IT administrator must have local administrator credentials.

---

#### Method 7: Configuration Manager Co-Management

**Type:** IT admin-configured  
**Description:** Manages Windows devices concurrently using **both Configuration Manager and Intune**. Bridges traditional and modern management.

- **When to use:** Preferred for existing devices already managed by Microsoft Configuration Manager. Allows a phased transition to modern management.

---

#### Method 8: Microsoft Entra Join (Bulk Enrollment)

**Type:** IT admin-configured  
**Description:** Sets up many devices to be Intune-managed without reimaging. Uses a **provisioning package** created with the **Windows Configuration Designer app** (available from the Store). Package is applied during OOBE or from the Settings app.

- **When to use:** Large-scale deployments where individual user instructions are impractical.
- **Requires:** Microsoft Entra ID P1 or P2 licenses and Windows Configuration Designer.

---

## 6. Enroll Android Devices in Intune

> **Source Unit:** [Unit 7](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/7-enroll-android-devices-intune) · 3 min read

### Personal (BYOD) Android Enrollment

To enroll a personal Android device using the Company Portal:

1. Install the free **Intune Company Portal** app from **Google Play Store**.
2. Open the Company Portal app.
3. On the Welcome screen, tap **Sign in** and sign in with your work or school account.
4. Follow the instructions in the Company Portal. The end-user experience may vary based on policies assigned to the user and/or device.

### Android Enterprise Management

Android Enterprise makes it easier to define the scope of management, balancing what IT controls versus what the device user/owner controls.

| Mode | Description |
|---|---|
| **Android Enterprise Work Profile** | Separates work and personal data. Management applies only to the work profile created during enrollment. Users can install any app on the personal side. Only supported on certain Android devices. |
| **Android Enterprise Dedicated** | For corporate-owned, single-use devices (digital signage, ticket printing, inventory management). Admins lock usage to a limited set of apps and web links. |
| **Android Enterprise Fully Managed** | For corporate-owned, single-user devices used **exclusively for work**. Admins manage the entire device and enforce all policy controls. |

**Setup Requirement:** To use Android Enterprise, you must connect your Intune tenant account to your **Managed Google Play account** and set up an enrollment profile matching your management method.

For more information: [Enroll Android devices](https://learn.microsoft.com/en-us/intune/intune-service/user-help/enroll-device-android-company-portal)

---

## 7. Enroll iOS Devices in Intune

> **Source Unit:** [Unit 8](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/8-enroll-ios-devices-intune) · 8 min read

### Personal (BYOD) iOS Enrollment

To enroll a personal iOS device using the Company Portal:

1. Download and install the **Intune Company Portal** from the **Apple App Store**.
2. Open the Company Portal app.
3. On the Welcome screen, tap **Sign in** and sign in with your work or school account.
4. Follow the instructions in the Company Portal. The experience may vary based on policies assigned to the user and/or device.

### Company-Owned iOS Enrollment Methods

For organizations that purchase devices for their users, Intune supports:

| Method | Description |
|---|---|
| **Apple Business Manager (ABM)** | Apple's portal for managing corporate devices and apps |
| **Apple's Automated Device Enrollment (ADE)** | Automates enrollment and configuration over the air |
| **Apple School Manager** | For educational institutions |
| **Apple Configurator** | Manual configuration for corporate devices |
| **Intune DEM Account** | Device Enrollment Manager for bulk enrollment |
| **Device Enrollment Program (DEP)** | Deploy enrollment profiles "over the air"; available only for devices purchased through Apple or authorized resellers |

### Apple Device Enrollment Program (DEP) — Detailed Steps

The Apple DEP is an online service that automates iOS device enrollment and configuration to MDM. Available only for organization-purchased devices.

**DEP Enrollment Process:**

1. Turn on the iOS device.
2. After selecting **Language**, connect the device to **Wi-Fi**.
3. The **Configuration** screen appears confirming the organization will automatically configure the device.
4. On the **Set up iOS device** screen, choose:
   - Set up as new device
   - Restore from iCloud backup
   - Restore from iTunes backup
5. **Log in with your Apple ID** to install the Company Portal and management profile.
6. Agree to the **Terms and Conditions** and decide on diagnostic data sharing with Apple.
7. Complete any remaining prompts (e.g., password for email, setting a passcode).

> **Key Advantage:** DEP allows you to ship devices (iPhones, iPads) directly to users. When the user turns on the device, Setup Assistant runs with preconfigured settings and the device enrolls into management automatically.

For more information: [Automatically enroll iOS devices with Apple's Device Enrollment Program](https://learn.microsoft.com/en-us/intune/intune-service/enrollment/tutorial-use-device-enrollment-program-enroll-ios)

### iOS Supervised Mode

An iOS device in **supervised mode** can be managed with more controls and is especially useful for corporate-owned devices. Intune supports configuring devices for supervised mode as part of DEP.

> ⚠️ **Important:** With **iOS 11 and later**, supervised mode should **always be enabled**. Support for unsupervised mode was deprecated in iOS 11.

---

## 8. Explore Device Enrollment Manager (DEM)

> **Source Unit:** [Unit 9](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/9-explore-device-enrollment-manager) · 4 min read

### What is DEM?

The **Device Enrollment Manager (DEM)** account is a special Intune user account that can enroll **up to 1,000 devices**. It is useful for scenarios where devices are enrolled and prepared before being handed out to end users.

**Typical DEM workflow:**
1. DEM enrolls the device.
2. DEM logs on to the Company Portal.
3. DEM installs apps required by the user.
4. Device is handed off to the end user.

> **Note:** If the user requires individual configuration (e.g., email profiles), the user should enroll the device themselves, DEM should not be used in that case.

>  **Security Best Practice:** The DEM user should **not** also be an Intune admin.

>  **Compatibility Note:** The DEM enrollment method **cannot** be used with: Apple Configurator with Setup Assistant, Apple Configurator with direct enrollment, Apple School Manager (ASM), or Device Enrollment Program (DEP).

### Example Scenario

> A restaurant wants to provide **50 point-of-sale tablets** for its wait staff and order monitors for kitchen staff. Employees never need to access company data or sign in as users. The Intune admin creates a DEM account for the restaurant supervisor (separate from the supervisor's primary account) used only for enrolling shared devices. The supervisor enrolls all 50 tablets using the DEM credentials.

### What Can a DEM User Do?

- Enroll up to **1,000 devices** in Intune
- Sign in to the Company Portal to get company apps
- Configure access to company data by deploying role-specific apps

### Limitations of DEM-Enrolled Devices

| Limitation | Details |
|---|---|
| **No per-user access** | Devices have no assigned user → no email or company data access. VPN configurations can still provide app-level data access. |
| **Cannot self-unenroll** | DEM user cannot unenroll DEM-enrolled devices via Company Portal. Only the Intune admin can unenroll. |
| **Company Portal visibility** | Only the local device appears in the Company Portal app or website. |
| **No Apple VPP user licenses** | Cannot use Apple Volume Purchase Program apps with user licenses (due to per-user Apple ID requirements). |
| **iOS limitation** | If DEM is used for iOS, Apple Configurator, DEP, and ASM cannot be used. Device cannot be put in supervised mode. |
| **Android work profile limit** | Up to **10 Android work profile devices** per DEM account (does not apply to legacy Android enrollment). |
| **VPP device licenses** | Devices can install VPP apps if they have device licenses. |
| **License** | An Intune device license is **not required** to use DEM. |

### Add a Device Enrollment Manager

1. Sign in to [Microsoft Intune admin center](https://intune.microsoft.com).
2. In the left navigation, select **Devices** → **Enroll devices** → **Device enrollment managers**.
3. Select **Add**.
4. On the **Add User** panel, enter a user principal name for the DEM user and select **Add**.

### Required Permissions for DEM

To complete DEM-related tasks in the Admin Portal or access all DEM users (regardless of RBAC permissions), a **Global Administrator** or **Intune Service Administrator** Microsoft Entra role is required.

> A user without these roles but with read permissions for the Device Enrollment Managers role can only access the DEM users they created.

---

## 9. Monitor Device Enrollment

> **Source Unit:** [Unit 10](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/10-monitor-device-enrollment) · 3 min read

### Monitoring Enrolled Devices (Intune Admin Center)

1. Sign in to the **Microsoft Intune admin center**.
2. Select **Devices**.

Available views:

| View | Description |
|---|---|
| **Overview** | Visual snapshot of enrolled devices by platform (Android, iOS, Windows, etc.) |
| **All Devices** | List of all enrolled managed devices. Supports **Export** to `.csv` (10,000 records in Internet Explorer; 30,000 in Microsoft Edge/Chrome). Select any device for hardware details, installed apps, compliance policy status, and more. |
| **Monitor** | Reports on device status, including: **Enrollment failures** (by failure type and enrollment method) and **Incomplete user enrollments** (useful for improving onboarding documentation) |

### Monitoring Microsoft Entra Joined Devices

Not all devices appear in the Intune admin center — only those **enrolled in Intune**. Domain-joined devices not enrolled in Intune can be viewed in the **Azure portal**:

1. Sign in to the **Azure portal**.
2. Select **Microsoft Entra ID** → **Devices**.

| View | Description |
|---|---|
| **All Devices** | Lists all devices registered or joined with Microsoft Entra ID |
| **Device Settings** | Settings for joining devices to Microsoft Entra ID, local admin accounts, personal device registration, and MFA requirements |
| **Enterprise State Roaming** | Defines which groups may sync settings and app data across devices |
| **Audit Logs** | Logs all activities that generate changes in Microsoft Intune (create, update, delete, assign). Enabled by default, **cannot be disabled**. Filterable by various criteria or retrievable via Graph API (up to **1 year** of audit events). |

---

## 10. Manage Devices Remotely

> **Source Unit:** [Unit 11](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/11-manage-devices-remotely) · 5 min read

### Viewing Device Details

1. Sign in to the **Microsoft Intune admin center**.
2. Select **Devices** → **All Devices**.
3. Select an individual device to view.

The **Overview** page shows the device name, whether it is BYOD, last check-in time, and available actions.

### Remote Actions Available

The available remote actions depend on device platform and configuration:

| Remote Action | Platforms | Description |
|---|---|---|
| **Retire** | All | Removes managed app data, settings, and email profiles assigned by Intune. Removes device from Intune management. |
| **Wipe** | All | Restores device to factory defaults. User data is kept if **"Retain enrollment state and user account"** is checked; otherwise all data, apps, and settings are removed. |
| **Delete** | All | Removes device from the Intune portal. Company data is removed on next check-in. |
| **Remote Lock** | Android, iOS, macOS | Locks the device. Device owner must enter passcode to unlock. Devices without a PIN/password cannot be remotely locked. |
| **Sync** | All | Forces device to immediately check in with Intune and receive any pending actions or policies. Useful for troubleshooting without waiting for scheduled check-in. |
| **Reset Passcode** | iOS, Android | Resets the device-level passcode. |
| **Restart** | Most (not macOS or Android work profile) | Causes device to restart within 5 minutes. Device owner is not notified automatically. |
| **Fresh Start** | Windows 10+ | Removes all installed apps, including preinstalled OEM apps. |
| **AutoPilot Reset** | Windows 10+ | Initiates reset process remotely, without requiring IT staff to visit each device. |
| **Quick Scan** | Windows 10+ | Performs a Microsoft Defender quick scan on common malware locations (registry keys, known startup folders). |
| **Full Scan** | Windows 10+ | Performs a Microsoft Defender full scan. |
| **Update Microsoft Defender Security Intelligence** | Windows 10+ | Instructs device to check for signature updates. |
| **BitLocker Key Rotation** | Windows 10+ (v1909+) | Remotely rotates the BitLocker recovery key. |
| **Rename Device** | All | Renames the device in Intune and on the device itself. |
| **New Remote Assistance Session** | All | Initiates a remote session using **TeamViewer** (third-party, purchased separately). |
| **Locate Device** | Windows 10+, iOS, Android Enterprise dedicated | Identifies the location (or last known location for Android dedicated) or initiates a sound alert. |

### Device Detail Pages

From the device details page, you can also manage:

| Section | Details |
|---|---|
| **Properties** | Assign a device category; change device ownership (personal or corporate). |
| **Hardware** | Device ID, OS version, storage, model, manufacturer, conditional access settings, and more. |
| **Discovered Apps** | Lists all apps Intune found on the device with versions. Exportable to `.csv`. |
| **Device Compliance** | Shows all assigned compliance policies and current compliance status. |
| **Device Configuration** | Shows all assigned configuration policies and whether each policy succeeded or failed. |

> **Note on App Inventory:** Intune collects a full app list on **corporate-owned devices**. On **personal devices**, only organizational managed apps are checked. For Windows clients, modern apps are listed for corporate-owned devices. Win32 apps are collected if the **Intune management extension** is installed. Not all apps may be collected depending on carrier.

---

## 11. Tools and Resources Used

| Tool / Resource | Description | Link |
|---|---|---|
| **Microsoft Intune admin center** | Primary portal for all Intune management tasks | [https://intune.microsoft.com](https://intune.microsoft.com) |
| **Intune Company Portal (Web)** | User-facing portal for device enrollment and app access | [https://portal.manage.microsoft.com/](https://portal.manage.microsoft.com/) |
| **Microsoft 365 Admin Center** | Manage domains, users, and subscriptions | [https://admin.microsoft.com](https://admin.microsoft.com) |
| **Azure Portal** | View Microsoft Entra ID devices, audit logs | [https://portal.azure.com](https://portal.azure.com) |
| **Apple Certificate Portal** | Create Apple MDM Push Certificates | Linked from Intune portal |
| **Google Play Store** | Download Intune Company Portal for Android | [https://play.google.com](https://play.google.com) |
| **Apple App Store** | Download Intune Company Portal for iOS | [https://www.apple.com/app-store/](https://www.apple.com/app-store/) |
| **Windows Configuration Designer** | Create provisioning packages for bulk Windows enrollment | Available in Microsoft Store |
| **Apple Device Enrollment Program (DEP) / ADE** | Automates iOS/macOS enrollment for org-purchased devices | Accessed via Apple Business Manager |
| **Apple Business Manager (ABM)** | Portal for managing corporate Apple devices and apps | [https://business.apple.com](https://business.apple.com) |
| **TeamViewer** | Third-party remote assistance tool integrated with Intune | [https://www.teamviewer.com](https://www.teamviewer.com) |
| **Microsoft Graph API** | Retrieve Intune audit logs (up to 1 year) | [https://learn.microsoft.com/en-us/graph/overview](https://learn.microsoft.com/en-us/graph/overview) |
| **Microsoft Defender** | Antivirus/antimalware; quick scan, full scan, signature update via Intune remote actions | Built into Windows |
| **BitLocker** | Windows drive encryption; key rotation available as Intune remote action (Windows 10 v1909+) | Built into Windows |

### Source Module Links

| Unit | Topic | URL |
|---|---|---|
| Unit 2 | Manage mobile devices with Intune | [Link](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/2-manage-mobile-devices-intune) |
| Unit 3 | Enable mobile device management | [Link](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/3-enable-mobile-device-management) |
| Unit 4 | Considerations for device enrollment | [Link](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/4-explain-considerations-device-enrollment) |
| Unit 5 | Manage corporate enrollment policy | [Link](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/5-manage-corporate-enrollment-policy) |
| Unit 6 | Enroll Windows devices in Intune | [Link](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/6-enroll-windows-devices-intune) |
| Unit 7 | Enroll Android devices in Intune | [Link](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/7-enroll-android-devices-intune) |
| Unit 8 | Enroll iOS devices in Intune | [Link](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/8-enroll-ios-devices-intune) |
| Unit 9 | Explore Device Enrollment Manager | [Link](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/9-explore-device-enrollment-manager) |
| Unit 10 | Monitor device enrollment | [Link](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/10-monitor-device-enrollment) |
| Unit 11 | Manage devices remotely | [Link](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/11-manage-devices-remotely) |

---

## 12. Confirmed Scenarios and Detailed Solutions

The following real-world scenarios are directly applicable from the module content, with detailed steps and supporting references.

---

### Scenario 1: Employee's New Laptop Must Be Enrolled Before Accessing Company Email

**Problem:** A new employee cannot access Outlook/Exchange Online from their personal Windows laptop.  
**Cause:** Conditional Access policy requires enrollment before accessing company resources.

**Solution — Enroll a Personal Windows Device:**

1. Open **Settings** → **Accounts** → **Access work or school**.
2. Select **Connect**.
3. Enter your work email and follow the prompts to sign in with your work/school account.
4. If Microsoft Entra ID P1/P2 auto-enrollment is configured, the device will be enrolled automatically.
5. If not, select **Enroll only in device management** and follow prompts.
6. Access to email is granted after compliance is verified.

**Sources:**
- [Unit 5 — Configure Automatic MDM Enrollment](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/5-manage-corporate-enrollment-policy)
- [Unit 3 — Considerations for Device Enrollment](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/4-explain-considerations-device-enrollment)
- [Microsoft Docs — Enroll Windows 10 device](https://learn.microsoft.com/en-us/intune/intune-service/user-help/enroll-windows-10-device)

---

### Scenario 2: Organization Deploys 200 Corporate iPhones Without Touching Each Device

**Problem:** IT must enroll and configure 200 iPhones for employees working in field offices. Manual setup is not feasible.  
**Solution — Use Apple DEP / Automated Device Enrollment (ADE):**

1. Purchase iPhones through **Apple Business Manager (ABM)** or an authorized reseller.
2. In the [Intune admin center](https://intune.microsoft.com), navigate to **Devices** → **Enrollment** → **Apple Enrollment** → **Enrollment Program Tokens**.
3. Upload an Apple MDM Push Certificate (if not already done).
4. Create a DEP/ADE enrollment profile with preconfigured settings (Wi-Fi, apps, skip OOBE screens).
5. Assign the serial numbers of the 200 devices to the enrollment profile in ABM.
6. Ship devices directly to employees. When powered on, the iPhone downloads the profile and enrolls automatically.
7. Enable **supervised mode** for maximum policy control.

**Sources:**
- [Unit 8 — Enroll iOS Devices in Intune](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/8-enroll-ios-devices-intune)
- [Automatically enroll iOS devices with Apple's DEP](https://learn.microsoft.com/en-us/intune/intune-service/enrollment/tutorial-use-device-enrollment-program-enroll-ios)

---

### Scenario 3: Restaurant Needs 50 Shared Point-of-Sale Tablets Enrolled

**Problem:** 50 tablets will be shared among wait staff. No individual user accounts just shared kiosk-style devices.  
**Solution — Use Device Enrollment Manager (DEM):**

1. In [Microsoft Intune admin center](https://intune.microsoft.com), go to **Devices** → **Enroll devices** → **Device enrollment managers**.
2. Select **Add** and create a DEM account for the restaurant supervisor (separate from their personal account).
3. The supervisor logs in with DEM credentials, installs the Company Portal, and enrolls each tablet.
4. Install the required POS apps from the Company Portal.
5. Hand tablets to staff — no personal sign-in required.

> **Note:** DEM cannot be used with Apple DEP/ASM/Apple Configurator for iOS. Consider Android Enterprise Dedicated mode for this type of scenario.

**Sources:**
- [Unit 9 — Explore Device Enrollment Manager](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/9-explore-device-enrollment-manager)
- [Unit 7 — Android Enterprise Dedicated](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/7-enroll-android-devices-intune)

---

### Scenario 4: IT Needs to Remotely Wipe a Lost Corporate iPhone

**Problem:** An employee reports their company iPhone is lost or stolen. IT must protect company data immediately.  
**Solution — Remote Wipe via Intune:**

1. Sign in to [Microsoft Intune admin center](https://intune.microsoft.com).
2. Go to **Devices** → **All Devices**.
3. Search for and select the affected device.
4. Select **Wipe** from the remote actions menu.
5. Confirm the action. The device will be restored to factory defaults on next check-in/when online.
   - To preserve the user's personal data: check **"Retain enrollment state and user account"**.
   - To remove everything: leave unchecked.

**Alternative — Retire (for BYOD/selective wipe):**
- Select **Retire** to remove only corporate-managed data, settings, and email profiles while leaving personal data intact.

**Sources:**
- [Unit 11 — Manage Devices Remotely](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/11-manage-devices-remotely)
- [Microsoft Docs — Retire or wipe devices](https://learn.microsoft.com/en-us/intune/intune-service/remote-actions/devices-wipe)

---

### Scenario 5: Enrollment Failures Are Occurring — How to Investigate

**Problem:** Multiple users are failing to enroll their devices. IT needs to identify patterns.  
**Solution — Monitor Enrollment Failures:**

1. Sign in to [Microsoft Intune admin center](https://intune.microsoft.com).
2. Go to **Devices** → **Monitor**.
3. Review:
   - **Enrollment failures** — Shows devices by failure type and enrollment method.
   - **Incomplete user enrollments** — Shows where users are dropping off (e.g., Terms of Use screen).
4. Based on findings:
   - If users are quitting at Terms of Use → simplify or re-word the Terms of Use.
   - If specific platforms are failing → verify enrollment restrictions and OS version requirements.
   - If MDM authority issues → verify MDM Authority is set to Intune (not None).
5. Export the device list from **All Devices** as a `.csv` for further analysis.

**Sources:**
- [Unit 10 — Monitor Device Enrollment](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/10-monitor-device-enrollment)
- [Unit 4 — Define Allowed Devices](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/4-explain-considerations-device-enrollment)

---

### Scenario 6: Set Up Android Work Profile for BYOD Employees

**Problem:** Employees want to use personal Android phones for work. IT needs to manage work apps/data without touching personal data.  
**Solution — Android Enterprise Work Profile:**

1. In [Intune admin center](https://intune.microsoft.com), connect the Intune tenant to your **Managed Google Play account** (**Tenant Administration** → **Connectors and tokens** → **Managed Google Play**).
2. Create a **Work Profile** enrollment profile under **Devices** → **Enrollment** → **Android** → **Enrollment profiles**.
3. Assign the profile to the target user group.
4. Instruct users to install the **Intune Company Portal** from Google Play.
5. Users sign in with their work account and follow prompts to create a work profile.
6. A separate work profile is created on the device — work apps appear with a badge icon, personal side is untouched.

**Sources:**
- [Unit 7 — Enroll Android Devices in Intune](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/7-enroll-android-devices-intune)
- [Microsoft Docs — Set up Android Enterprise work profile enrollment](https://learn.microsoft.com/en-us/intune/intune-service/enrollment/android-work-profile-enroll)

---

### Scenario 7: Migrating from On-Premises Configuration Manager to Intune via Co-Management

**Problem:** Organization has been using Microsoft Configuration Manager for years and wants to gradually adopt Intune without disrupting existing management.  
**Solution — Co-Management (Method 7):**

1. Ensure devices are running **Windows 10/11** and are already managed by **Configuration Manager**.
2. In Configuration Manager, enable **Co-management** (Administration → Cloud Services → Co-management).
3. Authenticate with your Microsoft Entra/Intune credentials to connect Configuration Manager to Intune.
4. Set workload sliders to control which workloads are managed by Configuration Manager vs. Intune:
   - Compliance policies
   - Device configuration
   - Resource access policies
   - Client apps
   - Windows Update policies
   - Endpoint protection
5. Gradually shift workloads to Intune as readiness increases.
6. Monitor compliance in both the Configuration Manager console and the Intune admin center.

**Sources:**
- [Unit 6 — Method 7: Configuration Manager Co-Management](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/6-enroll-windows-devices-intune)
- [Microsoft Docs — What is co-management?](https://learn.microsoft.com/en-us/mem/configmgr/comanage/overview)

---

### Scenario 8: Deploying 500 Windows Laptops Without User Interaction (Autopilot Self-Deploying)

**Problem:** IT needs to deploy 500 Windows laptops as kiosk devices with no user interaction required at setup.  
**Solution — Windows Autopilot Self-Deploying Mode:**

1. Register device hardware IDs with Windows Autopilot (via vendor or manually via `.csv` upload in Intune).
2. In [Intune admin center](https://intune.microsoft.com), go to **Devices** → **Enrollment** → **Windows** → **Windows Autopilot** → **Deployment Profiles**.
3. Create a new profile and select **Self-Deploying (preview)** mode.
4. Configure:
   - Language/region settings
   - Keyboard layout
   - OOBE screens to skip
5. Assign the profile to the device group.
6. When devices are powered on, they auto-join Microsoft Entra ID, enroll in Intune, and receive all policies and apps — with zero user interaction.

**Sources:**
- [Unit 6 — Method 5: Autopilot Self-Deploying Mode](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/6-enroll-windows-devices-intune)
- [Microsoft Docs — Windows Autopilot self-deploying mode](https://learn.microsoft.com/en-us/autopilot/self-deploying)

---
