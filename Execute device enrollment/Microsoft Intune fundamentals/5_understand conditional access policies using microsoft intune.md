# Policy and Security Management Using Microsoft Endpoint Manager

---

## 1. Policy Management Based on Groups

You can manage assignments of devices, apps, and policies based on groups of users or devices.

### Group Types

You can add the following types of groups:

| Group Type | Description |
|---|---|
| **Assigned groups** | Manually add users or devices into a static group. |
| **Dynamic groups** | Automatically add users or devices to user or device groups based on an expression you create. Requires **Microsoft Entra ID P1 or P2**. |

---

### Devices

To make managing devices easier, you can use **Microsoft Intune device categories** to automatically add devices to groups based on categories that you define.

**Device category workflow:**

1. Create categories from which users can choose when they enroll their device.
2. When users of **iOS/iPadOS** and **Android** devices enroll a device, they must choose a category from the list of configured categories. To assign a category to a **Windows device**, users must use the **Company Portal website**.
3. You can then deploy policies and apps to these groups.

**Example device categories:**

- Point-of-sale device
- Demonstration device
- Sales
- Accounting
- Manager

> After a device is enrolled, it becomes **managed**. Your organization can assign policies and apps to the device through a mobile device management (MDM) provider, such as Intune.

---

### Apps

After you add an app to Microsoft Intune, you can assign the app to users and devices whether Intune manages the device or not.

**Key concepts:**

- You can determine who has access to an app by assigning groups of users to **include** and **exclude**.
- Before assigning groups, you must set the **assignment type** for an app (available, required, or uninstall).
- You can use a combination of include and exclude group assignments. For example:
  - Include a large group to make an app broadly available.
  - Exclude a smaller group (e.g., a test group or executive group) to narrow the scope.

> **Best practice:** Create and assign apps specifically for your **user groups** and separately for your **device groups**.

**Example automation:**

- A user with the **Manager** title is automatically added to an **All managers** user group.
- A device with the **iOS/iPadOS** OS type is automatically added to an **All iOS/iPadOS devices** group.

After you assign an app to a group in Intune, you can assign policies for the app to users or devices.

---

### Policies

You can assign policies to groups using Intune. When assigning policies, you can choose who will be **included** and who will be **excluded**.

---

### User Groups vs. Device Groups

#### Device Groups

Use device groups when:

- Settings should apply **regardless of who is signed in**.
- The device doesn't have a dedicated user (e.g., shared or specialized devices).

**Examples:**

- Devices that print tickets, scan inventory, or are shared across shift workers — put these in a device group.
- A **Device Firmware Configuration Interface (DFCI)** Intune profile that updates settings in the BIOS (e.g., disabling the camera or locking boot options).
- Controlling specific Microsoft Edge settings on designated Windows devices, regardless of who is using the device create an **Administrative Template** in Intune and assign it to the device group.

> **Summary:** Use device groups when you want settings to always be on the device, regardless of who is signed in.

#### User Groups

Use user groups when:

- Profile settings should follow the **user** across all their devices.
- You want to enforce settings for information workers and knowledge workers.

**Examples:**

- Putting a **Help Desk icon** on all devices for all users — assign the profile to a user group.
- A new organization-owned device where the user signs in with their domain account — the device auto-registers in **Microsoft Entra ID** and is managed by Intune; assign to a user group.
- Controlling features in apps (such as **OneDrive** or **Office**) whenever a user signs in — for example, blocking untrusted ActiveX controls using an Administrative Template assigned to a user group.

> **Summary:** Use user groups when you want settings and rules to always go with the **user**, whatever device they use.

---

## 2. Understand Conditional Access

**Conditional Access** ensures that only **trusted users** can access organizational resources on **trusted devices** using **trusted apps**. It is built from the cloud and works the same way whether you are managing devices with **Intune** or extending a **Configuration Manager** deployment with **co-management**.

**What Conditional Access allows you to do:**

- Control which devices and apps can connect to your email.
- Provide granular access control to keep corporate data secure.
- Allow end users to work from any device while maintaining security.
- Define conditions that gate access to corporate data based on:
  - **Location**
  - **Device**
  - **User state**
  - **Application sensitivity**

> **Licensing Note:** Conditional Access is a **Microsoft Entra** capability included with a **Microsoft Entra ID P1 or P2** license. Intune enhances this capability by adding **mobile device compliance** and **mobile app management** to the solution.

---

## 3. Using Conditional Access

By using Intune or Configuration Manager, you help ensure your organization is using proper credentials to access and share company data.

### Conditional Access with Intune

Intune provides the following types of Conditional Access:

- **Device-based Conditional Access**
  - Conditional Access for Exchange on-premises
  - Conditional Access based on network access control
  - Conditional Access based on device risk
  - Conditional Access for Windows PCs
    - Corporate-owned
    - Bring your own device (BYOD)
- **App-based Conditional Access**

---

### Conditional Access Using Co-Management

With co-management, Intune evaluates every device in your network to determine how trustworthy it is, using two approaches:

**1. Configuration-based evaluation (pre-security breach):**
- Intune checks that a device or app is managed and securely configured (e.g., encryption enabled, not jailbroken).
- For co-managed devices, **Configuration Manager** also performs configuration-based evaluation for required updates or apps compliance. Intune combines this with its own assessment.

**2. Incident-based evaluation (post-security breach):**
- Intune detects active security incidents on a device using **Microsoft Defender for Endpoint** (formerly Microsoft Defender Advanced Threat Protection / Windows Defender ATP) and other mobile threat-defense providers.
- Partners run ongoing behavioral analysis on devices, detect active incidents, and pass that information to Intune for **real-time compliance evaluation**.

---

### Common Ways to Use Conditional Access

> You must configure the related compliance policies to drive Conditional Access compliance at your organization.

#### Device-Based Conditional Access

Intune and **Microsoft Entra ID** work together so that only **managed and compliant devices** can access:
- Email
- Office 365 services
- SaaS apps
- On-premises apps

You can also set a policy in Microsoft Entra ID to only enable **domain-joined computers** or **mobile devices enrolled in Intune** to access Office 365 services.

Intune provides **device compliance policy capabilities** that evaluate the compliance status of devices. This compliance status is reported to Microsoft Entra ID, which uses it to enforce the Conditional Access policy when a user tries to access company resources.

#### Conditional Access Based on Network Access Control

Intune integrates with partners such as:

- **Cisco ISE**
- **Aruba ClearPass**
- **Citrix NetScaler**

These integrations provide access controls based on the **Intune enrollment** and **device-compliance state**.

Users can be allowed or denied access to corporate **Wi-Fi** or **VPN** resources based on whether their device is managed and compliant with Intune device compliance policies.

#### Conditional Access Based on Device Risk

Intune partners with **Mobile Threat Defense (MTD)** vendors to detect malware, Trojans, and other threats on mobile devices.

**How Intune and Mobile Threat Defense integration works:**
1. Mobile devices have the MTD agent installed.
2. The agent sends compliance state messages back to Intune when a threat is found.
3. This integration plays a factor in Conditional Access decisions based on **device risk**.

#### Conditional Access for Windows PCs

**Corporate-owned devices:**

| Scenario | Description |
|---|---|
| **Microsoft Entra hybrid joined** | For organizations comfortable managing PCs through AD group policies or Configuration Manager. |
| **Microsoft Entra domain joined + Intune management** | For cloud-first or cloud-only organizations. Device joins Microsoft Entra ID, enrolls in Intune, and uses this as a Conditional Access criteria when accessing corporate resources. Works well in hybrid environments for access to both cloud and on-premises apps. |

**Bring Your Own Device (BYOD):**

| Scenario | Description |
|---|---|
| **Workplace join + Intune management** | Users join personal devices to access corporate resources. Workplace join + Intune MDM enrollment provides device-level policies as another Conditional Access evaluation option. |

---

### App-Based Conditional Access

Intune and Microsoft Entra ID work together to make sure only **managed apps** can access corporate email or other **Office 365 services**.

---

## 4. Benefits of Conditional Access

Conditional Access combines **granular control over organizational data** with a user experience that maximizes worker productivity on any device from any location.

**With Conditional Access, you can determine if:**

- Every device is **encrypted**
- **Malware** is installed
- Settings are **updated**
- The device is **jailbroken or rooted**

**Additional benefits:**

- With **co-management**, Intune can incorporate Configuration Manager's responsibilities for assessing security standards compliance (required updates or apps). This is important for IT organizations that want to continue using Configuration Manager for complex app and patch management.
- Conditional Access is a critical component of developing a **Zero Trust Network architecture**. Compliant device access controls cover the foundational layers of Zero Trust. This functionality is a key part of how organizations secure their future infrastructure.

---

## 5. Implementing Security Rules

You can use the **Endpoint Security** node in Microsoft Intune to:
- Configure device security.
- Manage security tasks for devices that are at risk.
- Identify at-risk devices, remediate them, and restore them to a compliant or more secure state.

  <img width="645" height="381" alt="image" src="https://github.com/user-attachments/assets/fdd75d91-a4db-4913-bd4b-3ab238dd6b1b" />

**The Endpoint Security node provides the following tools:**

### Review Status of All Managed Devices
- View device compliance from a high level.
- Drill into specific devices to understand which compliance policies aren't met, so you can resolve them.

### Deploy Security Baselines
- Intune includes **security baselines** for Windows devices and a growing list of applications (e.g., **Microsoft Defender for Endpoint**, **Microsoft Edge**).
- Security baselines are pre-configured groups of Windows settings that apply a known group of settings and default values recommended by relevant security teams.
- Use security baselines to rapidly deploy a **best practice** configuration of device and application settings to protect users and devices.
- **Supported on:** Windows 10 version **1809** and later.

### Manage Security Configurations via Tightly Focused Policies
Each endpoint security policy focuses on aspects of device security:

- Antivirus
- Disk encryption
- Firewalls
- Additional areas made available through integration with **Microsoft Defender for Endpoint**

### Establish Device and User Requirements via Compliance Policy
With compliance policies, you set rules that devices and users must meet to be considered compliant. Rules can include:

- OS versions
- Password requirements
- Device-threat levels

When integrated with **Microsoft Entra Conditional Access policies**, you can gate access to corporate resources for both managed and unmanaged devices.

> **Important:** Endpoint security policies are one of several methods in Intune to configure settings on devices. Understand what other methods are in use in your environment to **avoid configuration conflicts**.

### Integrate Intune with Microsoft Defender for Endpoint
- Gain access to **security tasks** that closely tie Microsoft Defender for Endpoint and Intune together.
- Your security team can identify at-risk devices and hand off detailed **remediation steps** to Intune admins who can act.

### Integrate Configuration Manager with Microsoft Defender for Endpoint
- Using **tenant attach** in a co-managed endpoint management scenario, you can integrate Configuration Manager with Microsoft Defender for Endpoint.
- Gain access to security tasks that help enterprises **detect, investigate, and respond to advanced attacks** on their networks.

---

## Tools and Resources Used

| Tool / Service | Description |
|---|---|
| **Microsoft Intune** | Mobile device management (MDM) and mobile app management (MAM) provider |
| **Microsoft Endpoint Manager** | Unified endpoint management platform (includes Intune and Configuration Manager) |
| **Microsoft Entra ID (formerly Azure AD)** | Cloud identity service; provides Conditional Access. Requires P1 or P2 license for dynamic groups and Conditional Access |
| **Microsoft Configuration Manager** | On-premises endpoint management used in co-management scenarios |
| **Microsoft Defender for Endpoint** | Threat detection and response platform integrated with Intune |
| **Microsoft Edge** | Browser for which security baselines are provided in Intune |
| **Cisco ISE** | Network access control partner for Conditional Access integration |
| **Aruba ClearPass** | Network access control partner for Conditional Access integration |
| **Citrix NetScaler** | Network access control partner for Conditional Access integration |
| **Mobile Threat Defense (MTD) vendors** | Third-party security partners for device risk-based Conditional Access |
| **Company Portal** | Microsoft app/website used by end users to enroll and manage devices |
| **Device Firmware Configuration Interface (DFCI)** | Intune profile type for BIOS-level settings on supported Windows devices |
| **Zero Trust Network architecture** | Security model that Conditional Access supports through compliant device access controls |
| **Microsoft Learn** | Source training platform — [learn.microsoft.com](https://learn.microsoft.com) |

---

## Confirmed and Worked-On Scenarios

The following are real-world scenarios that map directly to the content above, with detailed steps and applicable sources.

---

### ✅ Scenario 1: Automatically Add Devices to Groups by Category

**Problem:** IT wants iPads used at point-of-sale to automatically receive a specific app and configuration profile without manual assignment.

**Solution:** Use **Intune Device Categories** to create a "Point-of-Sale" category, then create a **Dynamic Device Group** that targets devices in that category.

**Steps:**
1. In the [Microsoft Intune admin center](https://intune.microsoft.com), go to **Devices** > **Device categories** > **Create device category**.
2. Create a category named `Point-of-Sale`.
3. Go to **Groups** in Microsoft Entra ID > **New group** > **Dynamic Device** membership rule:
   ```
   (device.deviceCategory -eq "Point-of-Sale")
   ```
4. Assign your app and configuration profile to this dynamic group.
5. When users enroll an iOS/iPadOS device and select "Point-of-Sale," the device is automatically placed in the group.

**Sources:**
- [Create device categories in Intune](https://learn.microsoft.com/en-us/mem/intune/enrollment/device-group-mapping)
- [Dynamic group rules for devices](https://learn.microsoft.com/en-us/entra/identity/users/groups-dynamic-membership)

---

### ✅ Scenario 2: Block Non-Compliant Devices from Accessing Exchange Online

**Problem:** Users on personal, unmanaged phones are accessing corporate email. IT wants to enforce that only enrolled and compliant devices can reach Exchange Online.

**Solution:** Combine an **Intune device compliance policy** with a **Microsoft Entra Conditional Access policy** targeting Exchange Online.

**Steps:**
1. In Intune admin center, go to **Devices** > **Compliance policies** > **Create policy**.
2. Set rules: require encryption, minimum OS version, no jailbreak/root.
3. Assign the compliance policy to **All Users** or a relevant group.
4. In **Microsoft Entra ID** > **Security** > **Conditional Access** > **New policy**:
   - **Assignments:** All users (or a group)
   - **Target resources:** Office 365 Exchange Online
   - **Conditions:** (optionally filter by platform)
   - **Grant:** Require device to be marked as compliant
5. Enable the policy.

**Sources:**
- [Create a compliance policy in Intune](https://learn.microsoft.com/en-us/mem/intune/protect/create-compliance-policy)
- [Conditional Access — Require compliant device](https://learn.microsoft.com/en-us/entra/identity/conditional-access/howto-conditional-access-policy-compliant-device)

---

### ✅ Scenario 3: Block Access If Mobile Threat Is Detected (Device Risk)

**Problem:** A user's Android phone is compromised with malware. IT wants Intune to automatically revoke access to corporate resources.

**Solution:** Integrate a **Mobile Threat Defense (MTD)** vendor with Intune and set a device risk level requirement in the compliance policy.

**Steps:**
1. In Intune admin center, go to **Tenant administration** > **Connectors and tokens** > **Mobile Threat Defense**.
2. Add and configure your MTD connector (e.g., Microsoft Defender for Endpoint, Lookout, Better Mobile).
3. Create a **compliance policy** and set **Device threat level** = `Secured` or `Low`.
4. Assign the compliance policy to users.
5. Create a **Conditional Access policy** that grants access only to compliant devices.
6. When the MTD agent detects a threat, it marks the device non-compliant → Conditional Access blocks access.

**Sources:**
- [Mobile Threat Defense integration with Intune](https://learn.microsoft.com/en-us/mem/intune/protect/mobile-threat-defense)
- [Set device compliance with MTD partners](https://learn.microsoft.com/en-us/mem/intune/protect/mtd-device-compliance-policy-create)

---

### ✅ Scenario 4: Deploy Security Baseline for Windows 10/11 Devices

**Problem:** IT needs to quickly apply Microsoft-recommended security settings to all corporate Windows devices without writing individual policies.

**Solution:** Use **Intune Security Baselines** for Windows.

**Steps:**
1. In Intune admin center, go to **Endpoint security** > **Security baselines**.
2. Select **Security Baseline for Windows 10 and later**.
3. Click **Create profile** and name it (e.g., `Corporate Windows Baseline`).
4. Review the pre-configured settings. Adjust any settings that conflict with your organizational requirements.
5. Under **Assignments**, add the device group to apply the baseline to.
6. Review and **Create**.
7. Monitor compliance in **Endpoint security** > **Security baselines** > select the profile > **Device status**.

> **Note:** Security baselines require Windows 10 version 1809 or later.

**Sources:**
- [Use security baselines in Intune](https://learn.microsoft.com/en-us/mem/intune/protect/security-baselines)
- [Security baseline settings for Windows](https://learn.microsoft.com/en-us/mem/intune/protect/security-baseline-settings-mdm-all)

---

### ✅ Scenario 5: Restrict App Access Using App-Based Conditional Access

**Problem:** Users are accessing SharePoint Online from personal devices using unmanaged browser apps. IT wants to restrict access to managed apps only.

**Solution:** Use **App-based Conditional Access** with Intune App Protection Policies.

**Steps:**
1. Create an **App Protection Policy** in Intune for iOS and Android targeting SharePoint/OneDrive.
2. In **Microsoft Entra ID** > **Conditional Access** > **New policy**:
   - **Target resources:** Office 365 SharePoint Online
   - **Grant:** Require approved client app OR Require app protection policy
3. Assign the policy to the appropriate user group.
4. Users accessing SharePoint from unmanaged apps are blocked; they must use the Microsoft Edge or OneDrive app.

**Sources:**
- [App-based Conditional Access with Intune](https://learn.microsoft.com/en-us/mem/intune/protect/app-based-conditional-access-intune)
- [App protection policies overview](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy)

---

### ✅ Scenario 6: BYOD Enrollment with Workplace Join

**Problem:** Employees want to access corporate email and OneDrive from personal Windows laptops, without full corporate management of their device.

**Solution:** Use **Workplace Join** combined with **Intune MAM (without enrollment)** or lightweight MDM enrollment for BYOD.

**Steps:**
1. Guide users to **Settings** > **Accounts** > **Access work or school** > **Connect** on their personal Windows device.
2. User enters their corporate credentials — device is **Workplace joined** (not fully domain-joined).
3. In Intune, configure a **Conditional Access policy** that allows Workplace-joined devices as a valid criterion.
4. Optionally, enroll the device in **Intune MDM** for fuller compliance evaluation.
5. Deploy **App Protection Policies** for Office apps so corporate data is protected without fully managing the device.

**Sources:**
- [BYOD and Intune](https://learn.microsoft.com/en-us/mem/intune/enrollment/byod-technology-decisions)
- [Workplace join overview](https://learn.microsoft.com/en-us/entra/identity/devices/concept-device-registration)

---

### ✅ Scenario 7: Implement Zero Trust with Conditional Access

**Problem:** The organization is moving toward a Zero Trust security model and needs to ensure all access to corporate resources is verified.

**Solution:** Use Conditional Access as the access enforcement layer for Zero Trust.

**Steps:**
1. Define **Conditional Access policies** that require:
   - Multi-Factor Authentication (MFA) for all users
   - Device compliance (must be enrolled and compliant in Intune)
   - Named locations to flag logins from unusual geographies
2. Enable **Microsoft Defender for Endpoint** integration with Intune for real-time risk signals.
3. Use **Security Baselines** to harden all Windows endpoints.
4. Review the **Zero Trust deployment guide** for Microsoft 365 to align your policies.
5. Monitor access with **Microsoft Entra ID Sign-in logs** and **Intune compliance reports**.

**Sources:**
- [Zero Trust with Microsoft Intune](https://learn.microsoft.com/en-us/mem/intune/fundamentals/zero-trust-with-microsoft-intune)
- [Microsoft Zero Trust guidance](https://learn.microsoft.com/en-us/security/zero-trust/)

---
