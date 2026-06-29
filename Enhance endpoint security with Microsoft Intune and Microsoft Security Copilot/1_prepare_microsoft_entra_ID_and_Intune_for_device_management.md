> `Role: Administrator, Security Engineer, Security Operations Analyst`
> `Subject: Asset management, Cloud computing, Cloud security Compliance, IT management and monitoring, Security Threat protection`

> learnt how to prepare your environment for device management, enroll and validate devices, configure and secure endpoints, protect data with conditional access, harden security with Defender for Endpoint, and accelerate remediation using Microsoft Security Copilot. Gain hands-on experience with real-world labs and practical scenarios to implement Zero Trust principles and modern endpoint security.
> **Prerequisites**
> Basic understanding of IT security principles
> Familiarity with Microsoft 365, Microsoft Intune, and Microsoft Entra ID
> (Optional) Access to Microsoft Intune and Microsoft Security Copilot for hands-on practice


# Prepare Microsoft Entra ID and Intune for Device Management
---

## 1. Introduction

Modern organizations face increasing challenges securing and managing endpoints in a world where employees work from anywhere on diverse devices. Traditional on-premises management tools cannot follow devices outside the corporate network, leaving gaps in security and visibility. As an IT administrator, you need a comprehensive solution that can protect corporate data while providing a seamless experience for users — whether they're connecting from a company-owned laptop, a personal tablet, or a mobile device.

### Mobile Device Management (MDM) vs. Mobile Application Management (MAM)

| Approach | Scope | Use Case |
|---|---|---|
| **MDM** | Manages and secures entire devices | Enforces encryption, deploys policies, ensures compliance — even without corporate network access |
| **MAM** | Protects corporate data within specific apps | Enables BYOD scenarios without requiring full device control |

Microsoft Intune provides cloud-native MDM and MAM capabilities that help organizations secure, deploy, and manage all their users' devices from anywhere. Unlike traditional management tools that require on-premises infrastructure, Intune operates entirely in the cloud — devices only need internet access.

---

### Scenario: Contoso Corporation's Modernization Journey

> **Context**: You're an IT administrator at Contoso Corporation, a mid-sized organization that plans to modernize its endpoint management strategy. Your current environment relies on on-premises tools, and leadership has decided to adopt Microsoft Intune to support hybrid work scenarios, improve security posture, and enable Zero Trust principles.

**Zero Trust core principles as applied to Intune:**

| Principle | Description | Intune Implementation |
|---|---|---|
| **Verify explicitly** | Authenticate every access request | Verifying device compliance before granting access |
| **Use least privilege access** | Limit permissions to only what's needed | Enforcing app protection policies |
| **Assume breach** | Minimize damage from compromised devices | Integrating with Microsoft Defender for Endpoint |

Your first responsibility is to prepare the foundation: configure identity and licensing, set up your tenant for device enrollment, and establish enrollment pathways for different device types and ownership scenarios.

---

### What You Learn

In this module, you complete the essential preparation steps before enrolling devices in Microsoft Intune:

- Configure Microsoft Entra ID roles and licensing required for device management
- Prepare your Microsoft Intune tenant with groups, settings, and configurations
- Choose and explain appropriate enrollment methods based on device platforms and ownership
- Understand the readiness requirements for common enrollment scenarios

---

### Main Goal

By the end of this module, you are ready to onboard devices into Microsoft Intune with confidence. You understand the prerequisites, have configured the necessary identity and tenant settings, and can recommend the right enrollment approach for different scenarios in your organization.

---

## 2. Set Up Microsoft Entra ID Roles and Licensing

Before you manage devices with Microsoft Intune, you need to ensure users and administrators have the right permissions and licenses. Microsoft Entra ID (formerly Azure Active Directory) serves as the identity foundation for Microsoft Intune, controlling who can access the service and what they can do.

<img width="1180" height="647" alt="image" src="https://github.com/user-attachments/assets/d5fbffee-2bf1-4c1b-9d8d-0e0cecbc1fcf" />

---

### Key Roles for Device Management

Microsoft Entra ID provides several built-in administrative roles that control access to Intune and device management capabilities. Choosing the right roles helps you implement the **principle of least privilege** — giving administrators only the permissions they need.

| Role | Permissions | Best Use Case |
|---|---|---|
| **Global Administrator** | Highest level of access across all Microsoft 365 services; full access to Intune and all device management capabilities | Emergency access and critical administrative tasks only. Should be limited to a small number of trusted administrators. |
| **Intune Administrator** | Full permissions to manage Microsoft Intune; can create and manage device enrollment policies, configuration profiles, and compliance policies; can view and manage all devices, users, and groups in Intune | Dedicated device management administrators who need full control over Intune configurations. |
| **Cloud Device Administrator** | Limited device management permissions in Microsoft Entra ID; can enable/disable devices and read BitLocker keys; **cannot** manage Intune-specific configurations like policies or profiles | Helpdesk staff who need device-level access without full Intune permissions. |
| **Security Administrator** | Can manage security features across Microsoft 365 services; can view security reports and set security policies | Combining device management with broader security responsibilities. |

---

### Best Practices for Role Assignment

These practices align with Zero Trust's **least privilege access** principle:

- **Assign roles to groups, not individual users** — Use Microsoft Entra security groups for role assignments rather than assigning directly to users. This simplifies management, improves consistency, and makes it easier to audit administrative access.
- **Limit Global Administrator usage** — Reserve for emergency access and critical administrative tasks. Most administrators should use lower-privileged roles like Intune Administrator or Cloud Device Administrator.
- **Protect administrative roles with Conditional Access** — Apply stricter Conditional Access policies to admin roles, requiring MFA, compliant devices, and trusted locations for all administrative sign-ins (**verify explicitly** for privileged access).
- **Use Privileged Identity Management (PIM)** — Require just-in-time elevation for sensitive administrative roles, ensuring privileges are granted only when needed and for limited time periods.
- **Create custom roles when needed** — Use Intune's custom role capabilities for specialized scenarios that require specific permission combinations.
- **Document role assignments** — Maintain an audit trail of who has what permissions, including justification for elevated access.

---

### License Requirements for Device Management

To use Microsoft Intune for device management, users must have one of the following licenses:

| License | What's Included | Best For |
|---|---|---|
| **Microsoft Intune** | Standalone Intune subscription | Organizations that only need device management without other Microsoft 365 services |
| **Microsoft 365 E3 or E5** | Intune plus Office apps, email, collaboration tools | Enterprise organizations needing comprehensive productivity and management |
| **Microsoft 365 F3** | Intune for frontline workers | Frontline and shift workers with simplified app and device needs |
| **Microsoft 365 Business Premium** | Intune plus business productivity tools | Small to medium businesses (up to 300 users) |
| **Enterprise Mobility + Security (EMS) E3 or E5** | Intune plus Microsoft Entra ID P1/P2, Azure Information Protection, and other security services | Organizations prioritizing security and identity management |

---

### Additional Licensing Considerations

#### Windows Autopilot
- Included with Intune licenses
- Requires Windows 10/11 Pro or Enterprise editions on devices
- No additional license required for cloud-based provisioning

#### Microsoft Defender for Endpoint
- Requires a separate Microsoft Defender for Endpoint license or Microsoft 365 E5
- Enables advanced threat protection and security baselining
- Integrates with Intune for device compliance

#### Conditional Access
- Included with Microsoft Entra ID P1 (part of EMS E3 or Microsoft 365 E3/E5)
- Allows device compliance to control access to corporate resources
- Essential for Zero Trust security models — enforces the **verify explicitly** principle by requiring devices to prove compliance before accessing data

---

### Device-Based Licensing

In addition to user-based licensing, Microsoft offers device-based licensing for shared devices:

- **Microsoft Intune Device subscription** — For devices not tied to a specific user (kiosks, conference room devices, shared tablets)
- Billed per device rather than per user
- Useful for scenarios where multiple users share the same device

---

### User Licensing

#### Set Usage Location

> **⚠️ Important**: Before assigning licenses, ensure each user has a **usage location** configured in Microsoft Entra ID. This is required due to licensing restrictions in certain countries/regions.

**To set usage location:**

1. Navigate to **Microsoft Entra ID** > **Users**
2. Select a user and choose **Profile**
3. Set the **Usage location** property
4. Save changes

> **Note**: Usage location can be set manually or automatically through directory synchronization if you're using Microsoft Entra Connect or Microsoft Entra Cloud Sync.

---

#### Direct License Assignment (Microsoft 365 Admin Center)

To manually assign an Intune license to a user:

1. Sign in to the [Microsoft 365 admin center](https://admin.microsoft.com)
2. Navigate to **Users** > **Active users**
3. Select an unlicensed user
4. Select **Licenses and apps**
5. Check the box for **Intune** (or choose Enterprise Mobility + Security E5 or another bundle containing Intune)
6. Select **Save changes**

The user now has the permissions needed to enroll devices into Intune management.

---

#### Group-Based License Assignment (Microsoft Entra Admin Center)

Use a security group to assign Intune (or bundled) licenses to multiple users:

1. Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com/) with **License Administrator** or **Global Administrator** role
2. Go to **Identity** > **Billing** > **Licenses**
3. Select **All products** and choose the product (e.g., **Enterprise Mobility + Security E5**, **Microsoft Intune Plan 1**, or a Microsoft 365 bundle)
4. Select **Licensed groups**
5. Select **+ Assign**
   <img width="1362" height="847" alt="image" src="https://github.com/user-attachments/assets/c668e6e2-2e0e-4769-ad74-368fa69f3aa6" />

7. Search for and select the target security group
8. Select **Assignment options** to review service plans and deselect any you don't want — keep dependent plans together
9. Select **Review + assign**, then **Assign**
10. Monitor status under **Licensed groups** (e.g., *Assignment in progress*, *Assignment successful*)

To verify later, revisit **Billing > Licenses > All products > [Product] > Licensed groups**.

---

### Configure Microsoft Entra Device Settings

Microsoft Entra ID includes several device-related settings that impact enrollment and management. These settings control which users can join devices and how devices are registered.

| Setting | Options | Recommendation |
|---|---|---|
| **Users may join devices to Microsoft Entra ID** | **All** (any user can join — default) / **Selected** (only specific users/groups) / **None** (blocks all joins) | Use **Selected** for production to control which users can enroll devices. Use **All** only for testing. |
| **Require MFA to register or join devices** | **Yes** (users must complete MFA before adding a device) / **No** (not required — default) | Enable for enhanced security, especially for BYOD scenarios. |
| **Maximum number of devices per user** | Default: 50 / Can be reduced or set to **Unlimited** | Reduce from default to limit device sprawl. Set to **Unlimited** only in testing environments. |
| **Local administrator settings** | Control whether user who joins device becomes local administrator | Configure based on security requirements and user roles. Consider removing local admin rights for standard users. |

> **⚠️ Important**: The **"Require MFA to register or join devices"** setting applies to Microsoft Entra joined devices and Microsoft Entra registered devices. It does **NOT** apply to:
> - Microsoft Entra hybrid joined devices
> - Microsoft Entra joined VMs in Azure
> - Microsoft Entra joined devices using Windows Autopilot self-deployment mode

**To configure these settings:**

1. Navigate to **Microsoft Entra ID** > **Devices** > **Device settings**
2. Adjust the settings based on your organization's security policies
3. Save changes
<img width="1000" height="607" alt="image" src="https://github.com/user-attachments/assets/1b82bdff-a44e-4d2d-87c6-7d37820c626e" />

---

### Identity and Licensing at Contoso

| Area | What Contoso Configured |
|---|---|
| **Administrative Roles** | Created "Intune Admins" security group with Intune Administrator role; limited Global Administrator to 2 emergency access accounts |
| **User Licensing** | Assigned Microsoft 365 E3 licenses to all 500 employees using group-based licensing; configured usage location for compliance |
| **Device Settings** | Set "Users may join devices" to "Selected" targeting pilot group; enabled MFA requirement for device registration |
| **Device Limits** | Reduced maximum devices per user from 50 to 5 for standard employees; set 10 for IT staff |
| **Local Admin Control** | Removed Microsoft Entra joined device users from local Administrators group for security |

---

## 3. Configure Your Tenant for Device Onboarding

With identity and licensing configured, you're ready to prepare your Microsoft Intune tenant for device enrollment. This involves setting the Mobile Device Management (MDM) authority, organizing groups for device targeting, and configuring enrollment restrictions to control which devices can enroll.
<img width="1320" height="763" alt="image" src="https://github.com/user-attachments/assets/6d07555d-bcd0-4459-a9ae-16f25ab202ce" />

---

### Set the MDM Authority

The **MDM authority** determines which service manages your mobile devices. Before enrolling any devices, you must designate Microsoft Intune as your MDM authority.

**The MDM authority defines:**
- Which service (Intune, Configuration Manager, or co-management) manages devices
- Where device inventory and policies are stored
- How devices communicate with management infrastructure

#### Verify Intune as MDM Authority

For new tenants, Microsoft Intune is automatically set as the MDM authority. No manual configuration is required. To verify:

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com)
2. Navigate to **Devices** > **Enroll devices**
3. Confirm that **Microsoft Intune** is shown as the MDM authority

> **Note**: If you're using Configuration Manager with co-management, your MDM authority will be set to **Configuration Manager**. This module focuses on cloud-native Intune scenarios.

---

### Organize Groups for Device Management

Groups are the foundation of device management in Microsoft Intune. They allow you to target policies, applications, and configurations to specific sets of users or devices.

#### Types of Groups in Microsoft Entra ID

| Group Type | Use | Recommendation for Intune |
|---|---|---|
| **Security groups** | Assign access permissions and deploy Intune policies; can contain users, devices, or both; can be assigned manually or dynamically | ✅ Recommended for device management |
| **Microsoft 365 groups** | Designed for collaboration (email, SharePoint, Teams) | ❌ Not recommended for Intune policy assignments |

#### Recommended Group Structure

| Group Category | Example Groups |
|---|---|
| **Device ownership** | Corporate-owned devices, Personal devices (BYOD) |
| **Device platform** | Windows devices, iOS/iPadOS devices, Android devices, macOS devices |
| **Department or location** | Finance department devices, Sales team devices, Remote workers |
| **Enrollment method** | Autopilot devices, Manually enrolled devices, Bulk enrollment devices |

---

### Dynamic Groups for Automated Management

Dynamic groups automatically add or remove members based on attribute rules. This eliminates manual group maintenance and ensures devices are always correctly targeted.

**To create a dynamic device group:**

1. Navigate to **Microsoft Entra ID** > **Groups** > **New group**
2. Set **Group type** to **Security**
3. Set **Membership type** to **Dynamic Device**
4. Choose **Add dynamic query**
5. Build your rule using attributes like device OS type, ownership, or enrollment profile
6. Validate the query and save the group

> **💡 Tip**: Use dynamic groups whenever possible to reduce administrative overhead. However, dynamic membership evaluation can take **up to 24 hours**, so plan accordingly for time-sensitive scenarios.
<img width="1172" height="677" alt="image" src="https://github.com/user-attachments/assets/4dae8d62-358d-48bc-8563-29b9742a51c1" />

#### Dynamic Group Rule Examples

**Group all Windows devices:**
```
(device.deviceOSType -eq "Windows")
```

**Group by ownership:**
```
(device.deviceOwnership -eq "Company")
```

**Group by enrollment profile:**
```
(device.enrollmentProfileName -eq "Autopilot-Profile-Finance")
```

---

### Configure Enrollment Restrictions

Enrollment restrictions control which devices and users can enroll in Microsoft Intune. They act as gatekeepers, ensuring only authorized devices join your managed environment. This supports Zero Trust's **verify explicitly** principle by preventing unauthorized or non-compliant devices from enrolling.

#### Types of Enrollment Restrictions

| Restriction Type | Purpose | Controls |
|---|---|---|
| **Device type restrictions** | Control which device platforms and OS versions can enroll | Platform selection (Windows, iOS, Android, macOS); minimum/maximum OS versions; ownership type (corporate-owned, personal/BYOD, or both) |
| **Device limit restrictions** | Prevent device sprawl and manage licensing costs | Maximum number of devices a user can enroll |

#### Default Enrollment Restrictions

| Setting | Default Value |
|---|---|
| **Platforms allowed** | All (Windows, iOS, Android, macOS) |
| **Ownership types allowed** | All (corporate-owned and personal/BYOD) |
| **Maximum devices per user** | 15 |
| **OS version restrictions** | None |

#### Create a Device Type Restriction

1. Navigate to **Devices** > **Enrollment restrictions**
2. Select **Create restriction** > **Device type restriction**
3. Configure allowed platforms (Windows, iOS, Android, macOS)
4. Set OS version requirements:
   - Minimum OS version
   - Maximum OS version (optional)
5. Choose ownership types:
   - Corporate-owned only
   - Personal (BYOD) only
   - Both
6. Assign the restriction to user groups
7. Save the restriction

#### Create a Device Limit Restriction

1. Navigate to **Devices** > **Enrollment restrictions**
2. Select **Create restriction** > **Device limit restriction**
3. Set the maximum number of devices per user
4. Assign the restriction to user groups
5. Save the restriction

#### Priority and Targeting

When a user belongs to multiple groups with different restrictions, **Intune applies the restriction with the highest priority** (priority 1 = highest; the default policy always has the lowest priority).

**Best practices:**
- Assign restrictive policies to pilot groups with high priority
- Keep the default policy as a fallback for all users
- Use group assignments to control which users receive which restrictions
- Test restrictions with a small group before broad rollout
- Remember: priority determines which policy applies, not restrictiveness

---

### Configure Enrollment Settings

Beyond restrictions, Microsoft Intune provides additional platform-specific settings that control the user experience during enrollment.

| Platform | Setting | Purpose | Configuration Location |
|---|---|---|---|
| **Windows** | Automatic enrollment | Allows users to enroll Windows devices by joining them to Microsoft Entra ID | Microsoft Entra ID > **Mobility (MDM and MAM)** |
| **iOS/iPadOS/macOS** | Apple Push Notification service (APNs) certificate | Enables communication between Intune and Apple devices | Intune admin center > **Devices** > **Enrollment** > **Apple** |
| **Android** | Managed Google Play | Connects Intune to Google Play for Android Enterprise enrollment | Intune admin center > **Devices** > **Enrollment** > **Android** |

> **Note**: For detailed configuration steps for Apple and Android enrollment, see the [Microsoft Intune documentation](https://learn.microsoft.com/en-us/mem/intune/).

---

### Configure Intune Branding and Notifications

Customizing the Company Portal and user-facing messages helps users understand they're connecting to your organization's resources. You can configure:

- Company name
- Company logo
- Privacy statement URL
- Support contact information
- Theme color

#### Terms and Conditions

To configure terms and conditions:

1. Navigate to **Tenant administration** > **Terms and Conditions**
2. Create a new terms and conditions policy
3. Provide a title, description, and terms document
4. Assign the policy to user groups
5. Publish the policy

Users must accept the terms before completing enrollment.

---

### Tenant Configuration at Contoso

| Configuration Area | What Contoso Implemented |
|---|---|
| **MDM Authority** | Set to Microsoft Intune (cloud-native management) |
| **Organizational Groups** | Created dynamic groups: "Windows-Devices", "iOS-Devices", "Android-Devices"; created assignment groups: "Finance-Dept", "Sales-Team", "Remote-Workers" |
| **Enrollment Restrictions** | Default policy allows all platforms with 5-device limit; created "Corporate-Windows-Only" restriction (priority 1) for Finance requiring Windows 10 19041+ and blocking personal devices |
| **Platform Certificates** | APNs certificate configured for iOS/iPadOS management; Android Enterprise connected to managed Google Play |
| **Company Portal** | Customized with Contoso logo, support email, and privacy statement URL |
| **Terms and Conditions** | Published device usage policy requiring acceptance before enrollment |

---

## 4. Choose and Explain Enrollment Methods

Microsoft Intune supports multiple enrollment methods to accommodate different device platforms, ownership models, and deployment scenarios.

**The enrollment method determines:**
- How much control IT has over the device
- What data is visible to IT administrators
- Which management policies can be applied
- The user experience during enrollment

**The right enrollment method depends on:**
- Device platform (Windows, iOS/iPadOS, Android, macOS)
- Device ownership (corporate-owned vs. personal/BYOD)
- Deployment scale (individual devices vs. bulk provisioning)
- User technical skill level

---

### Windows Enrollment Methods

| Method | Best For | Prerequisites | Key Capabilities |
|---|---|---|---|
| **User-driven enrollment (manual)** | Existing devices, remote workers, small-scale deployments, BYOD | Windows 10/11 Pro/Enterprise/Education; internet connection; Intune license; automatic enrollment configured | Users manually join device to Microsoft Entra ID via Settings app; IT can enforce policies and deploy apps; user remains local admin by default |
| **Windows Autopilot (User-driven)** | New device deployments, remote users needing personalized devices | Windows 10/11 Pro/Enterprise/Education; device registered with Autopilot; deployment profile assigned; network connectivity during OOBE | User signs in during OOBE and becomes primary user; eliminates custom OS images; consistent configuration; supports remote deployment |
| **Windows Autopilot (Pre-provisioning / White Glove)** | Devices needing extensive setup before user receives them | Same as user-driven Autopilot; IT technician access for initial provisioning | IT technician provisions device with apps and settings; ships to user who completes final setup; reduces deployment time for complex configurations |

**Autopilot benefits:**
- Eliminates need for custom OS images
- Reduces IT workload for provisioning
- Provides consistent configuration across devices
- Supports fully remote deployment scenarios

---

### Bulk Enrollment

Bulk enrollment enables IT to enroll multiple Windows devices simultaneously without individual user interaction. Best for:

- Kiosks, digital signage, and shared devices
- Education environments (shared student devices, computer labs)
- Dedicated-purpose devices without primary users
- Large-scale deployments with hundreds or thousands of devices
- Scenarios where devices need identical configuration

| Method | Description | Best Use Case | Key Requirements |
|---|---|---|---|
| **Windows Autopilot (Self-Deploying Mode)** | Pre-registers device hardware IDs; devices auto-provision during first boot without user sign-in | Shared devices, kiosks, digital signage | Windows 10/11 Pro/Enterprise/Education; no imaging required |
| **Configuration Manager Co-management** | Enrolls existing ConfigMgr-managed devices into Intune; enables gradual workload migration | Organizations with existing Configuration Manager infrastructure | Microsoft Entra hybrid join; supports thousands of devices |
| **Windows Configuration Designer (Provisioning Packages)** | Creates `.ppkg` files with device config, apps, enrollment settings; applied via USB/network/OOBE | Air-gapped environments or limited bandwidth locations | Token expires in 180 days; no internet required during setup |
| **Group Policy (Hybrid Joined Devices)** | Automatically enrolls Microsoft Entra hybrid joined devices through GPO | Organizations with existing Active Directory Domain Services | Devices joined to both on-premises AD and Microsoft Entra ID |

**Considerations:**
- Most bulk enrollment methods create devices **without user affinity** (not tied to a specific user)
- Limited policy support compared to user-driven enrollment for user-specific settings
- Choose method based on existing infrastructure (cloud-only vs. hybrid Active Directory)
- Provisioning packages and GPO require additional planning for certificate management and network access

---

### Apple Platforms (iOS/iPadOS/macOS)

Apple devices can be enrolled through:

- **User enrollment** — For BYOD scenarios with work/personal data separation
- **Automated Device Enrollment (ADE)** via Apple Business Manager — For corporate-owned devices

All methods use the **Apple Push Notification service (APNs) certificate** for communication between Intune and Apple devices.

**ADE key features:**
- Zero-touch deployment
- Prevents users from unenrolling corporate-owned devices
- Devices running macOS 11+ automatically receive supervision capabilities for enhanced management control

---

### Android Enrollment Methods

Android Enterprise provides three enrollment types:

| Type | Description | Use Case |
|---|---|---|
| **Work profile** | Work/personal data separation | BYOD scenarios |
| **Fully managed** | Complete IT control over the device | Corporate-owned devices |
| **Dedicated devices** | Kiosks and single-purpose devices | No user affinity required |

> **⚠️ Prerequisite**: Organizations must configure **managed Google Play** integration before enrolling Android devices.

---

### Choose the Right Enrollment Method

Selecting the wrong enrollment method creates security gaps, frustrates users, and increases management overhead. Match your enrollment strategy to three key factors:

| Factor | Consideration |
|---|---|
| **Control vs. privacy** | Corporate-owned devices need automated methods that prevent unenrollment. Personal devices require user-controlled enrollment with work/personal data separation. |
| **Scale and efficiency** | Manual enrollment works for pilot projects but automated methods (Autopilot, ADE) dramatically reduce per-device effort at enterprise scale. |
| **Security requirements** | Compliance-critical environments need full device management with enforced policies. BYOD scenarios often need only app-level protection without device-level control. |

#### Common Pitfalls to Avoid

| Pitfall | Risk |
|---|---|
| Using BYOD enrollment for corporate devices | Removes security controls and allows user unenrollment |
| Forcing full device management on personal devices | Creates privacy concerns and employee resistance |
| Manual provisioning at enterprise scale | Overwhelms IT staff with inconsistent configurations |
| Ignoring remote deployment needs | Leaves remote workers without enrollment options |

---

#### Enrollment Recommendations: Windows Devices

| Scenario | Recommended Method |
|---|---|
| New device purchase for known user | Windows Autopilot (user-driven) |
| Existing device, remote user | User-driven enrollment |
| Shared device in lobby or conference room | Windows Autopilot (self-deploying) or bulk enrollment |
| Large-scale refresh for remote employees | Windows Autopilot (pre-provisioning) |
| Kiosks, digital signage, dedicated-purpose devices | Windows Autopilot (self-deploying) or bulk enrollment |
| Hybrid AD environment with existing infrastructure | Configuration Manager co-management or Group Policy |

#### Enrollment Recommendations: Mobile and macOS

| Platform | BYOD Scenario | Corporate-Owned Scenario |
|---|---|---|
| **iOS/iPadOS** | User enrollment via Company Portal | Automated Device Enrollment (ADE) |
| **Android** | Android Enterprise work profile | Android Enterprise fully managed or dedicated |
| **macOS** | User-approved enrollment | Automated Device Enrollment (ADE) |

---

### Enrollment Strategy at Contoso

| Device Category | Enrollment Method | Rationale |
|---|---|---|
| **New Windows laptops** (Finance, 50 devices) | Windows Autopilot (user-driven) | Pre-registered hardware IDs with OEM; users receive devices at home and self-provision with corporate credentials |
| **Existing Windows desktops** (Office workers, 200 devices) | User-driven enrollment (manual join) | Users manually join via Settings > Accounts; supports remote workers with existing hardware |
| **Conference room devices** (Shared, 15 devices) | Windows Autopilot (self-deploying) | Zero-touch kiosk deployment without user affinity; locked-down single-purpose configuration |
| **Executive mobile devices** (iOS, 25 devices) | Automated Device Enrollment (ADE) via Apple Business Manager | Corporate-owned; supervised management prevents unenrollment; pre-configured with corporate apps |
| **Employee personal phones** (BYOD, optional) | iOS user enrollment / Android work profile | Separates work and personal data; user-controlled enrollment; app-level protection only |
| **Sales team tablets** (Android, 100 devices) | Android Enterprise fully managed | Corporate-owned; complete device control; managed Google Play for approved apps only |

---

## Confirmed Scenarios with Detailed Solutions

The following scenarios are derived directly from the module content and have been validated as applicable real-world implementations.

---

### Scenario 1: Setting Up Intune for a 500-User Organization

**Problem**: A mid-sized company is migrating from on-premises management to Intune and needs to configure roles and licenses before enrolling any devices.

**Solution**:

1. **Assign licenses** — Assign Microsoft 365 E3 (includes Intune) to all 500 users via group-based licensing in the Microsoft Entra admin center:
   - Navigate to **Identity** > **Billing** > **Licenses** > **All products**
   - Select your product, go to **Licensed groups**, and assign to an "All Employees" security group
2. **Set usage locations** — Run a bulk update via PowerShell or Azure AD sync to set the `UsageLocation` property for all users before license assignment
3. **Create an "Intune Admins" security group** — Assign the **Intune Administrator** role to this group; limit **Global Administrator** to 2 break-glass accounts
4. **Configure device settings** — Set "Users may join devices" to **Selected**, targeting an Intune Pilot group; enable MFA for device registration
5. **Verify MDM authority** — Confirm Microsoft Intune is set as the MDM authority in the Intune admin center

**Sources**:
- [Assign licenses to users](https://learn.microsoft.com/en-us/intune/fundamentals/licenses-assign)
- [Microsoft Entra admin center](https://entra.microsoft.com/)
- [Intune admin center](https://intune.microsoft.com)

---

### Scenario 2: BYOD Deployment for Employee Personal Devices

**Problem**: Employees want to use personal iOS and Android phones for work email and apps. IT must protect corporate data without full device control.

**Solution**:

1. **iOS**: Enable user enrollment via the Company Portal app
   - Configure APNs certificate under **Devices** > **Enrollment** > **Apple**
   - Create a device type restriction scoped to **Personal ownership** only for this group
2. **Android**: Configure Android Enterprise work profile
   - Connect Intune to managed Google Play: **Devices** > **Enrollment** > **Android**
   - Assign Android Enterprise work profile enrollment to the BYOD group
3. **Apply App Protection Policies (MAM)** — Protect corporate data within apps without requiring full device management
4. **Create a dynamic group** for BYOD devices:
   ```
   (device.deviceOwnership -eq "Personal")
   ```
5. **Communicate to employees**: Emphasize that work data is in a separate container and IT cannot access personal photos, calls, or apps

**Sources**:
- [Android Enterprise work profile](https://learn.microsoft.com/en-us/mem/intune/enrollment/android-work-profile-enroll)
- [iOS user enrollment](https://learn.microsoft.com/en-us/mem/intune/enrollment/ios-user-enrollment)

---

### Scenario 3: Deploying New Corporate Laptops Remotely with Windows Autopilot

**Problem**: The Finance department is receiving 50 new laptops shipped directly from the OEM to employees' homes. IT needs them fully configured without hands-on imaging.

**Solution**:

1. **Register hardware IDs with Autopilot** — Work with the OEM to upload device hardware IDs to Intune via the hardware hash
2. **Create an Autopilot deployment profile**:
   - Navigate to **Devices** > **Windows** > **Windows enrollment** > **Deployment profiles**
   - Select User-driven mode, OOBE settings, and assign to Finance group
3. **Create a dynamic group** for Autopilot devices:
   ```
   (device.enrollmentProfileName -eq "Autopilot-Profile-Finance")
   ```
4. **Assign apps and policies** to the Finance group before devices arrive
5. **User experience**: User receives laptop, powers on, connects to Wi-Fi, signs in with corporate credentials — device configures itself automatically

**Sources**:
- [Windows Autopilot overview](https://learn.microsoft.com/en-us/autopilot/windows-autopilot)
- [Autopilot deployment profiles](https://learn.microsoft.com/en-us/mem/intune/enrollment/enrollment-autopilot)

---

### Scenario 4: Kiosk and Shared Device Deployment

**Problem**: 15 conference room devices and lobby kiosks need enrollment with no user-specific configuration.

**Solution**:

1. **Pre-register device hardware IDs** with Autopilot
2. **Create a self-deploying Autopilot profile**:
   - Mode: **Self-Deploying**
   - No user authentication required during OOBE
3. **Device-based licensing**: Assign **Microsoft Intune Device subscription** instead of per-user licenses
4. **Create a dedicated device group** and assign a kiosk configuration profile (single-app or multi-app kiosk mode)
5. **Set a device limit restriction** — ensure these devices don't count against per-user device limits

**Sources**:
- [Autopilot self-deploying mode](https://learn.microsoft.com/en-us/autopilot/self-deploying)
- [Kiosk mode configuration](https://learn.microsoft.com/en-us/mem/intune/configuration/kiosk-settings)

---

### Scenario 5: Hybrid Environment — Enrolling Existing AD-Joined Devices

**Problem**: An organization has 200 existing Windows desktops joined to on-premises Active Directory. They want to bring them into Intune without reimaging.

**Solution**:

1. **Configure Microsoft Entra hybrid join** via Microsoft Entra Connect
2. **Enable automatic enrollment via Group Policy**:
   - Deploy the `MDM Enrollment` GPO to the target OU
   - Devices automatically enroll in Intune upon next Group Policy refresh
3. **Alternatively, configure co-management** with Configuration Manager to gradually shift workloads to Intune
4. **Test with a pilot OU** before broad deployment
5. **Validate enrollment** in the Intune admin center under **Devices** > **All devices**

**Sources**:
- [Configure hybrid Microsoft Entra join](https://learn.microsoft.com/en-us/entra/identity/devices/hybrid-join-plan)
- [Group Policy MDM enrollment](https://learn.microsoft.com/en-us/mem/intune/enrollment/windows-enroll)

---

## Related Scenarios and Detailed Solutions

---

### Related Scenario 1: Privileged Identity Management (PIM) for Intune Administrators

**Challenge**: You want to follow Zero Trust's least privilege principle by requiring just-in-time elevation for Intune administrators.

**Solution**:

1. Enable **Microsoft Entra PIM** (requires Entra ID P2 / EMS E5)
2. Configure the Intune Administrator role as **eligible** (not permanent)
3. Administrators request activation when needed, providing business justification
4. Activation is time-limited (e.g., 2 hours) and can require MFA or manager approval
5. All activations are logged for audit

**Sources**:
- [What is Privileged Identity Management?](https://learn.microsoft.com/en-us/entra/id-governance/privileged-identity-management/pim-configure)
- [Configure PIM for Intune Administrator role](https://learn.microsoft.com/en-us/entra/id-governance/privileged-identity-management/pim-how-to-add-role-to-user)

---

### Related Scenario 2: Conditional Access + Device Compliance

**Challenge**: Ensure only compliant, enrolled devices can access Microsoft 365 resources.

**Solution**:

1. Create a **Compliance policy** in Intune (e.g., require BitLocker, minimum OS version)
2. Create a **Conditional Access policy** in Microsoft Entra ID:
   - Target: All users or specific groups
   - Conditions: Require device to be marked as compliant
   - Grant: Allow only if device is compliant
3. Non-compliant devices are blocked from accessing Exchange Online, SharePoint, and Teams

**Sources**:
- [Device compliance policies](https://learn.microsoft.com/en-us/mem/intune/protect/device-compliance-get-started)
- [Conditional Access with Intune](https://learn.microsoft.com/en-us/mem/intune/protect/conditional-access)

---

### Related Scenario 3: Apple Business Manager + ADE for iOS Corporate Fleet

**Challenge**: Deploy 25 supervised iOS devices to executives with pre-installed apps, preventing unenrollment.

**Solution**:

1. Set up **Apple Business Manager (ABM)** and link it to Intune
2. Configure **APNs certificate** in Intune
3. Create an **ADE enrollment profile** with supervision enabled and "Allow users to remove enrollment profile" set to **No**
4. Assign devices in ABM to the Intune MDM server
5. When devices are powered on, they automatically enroll and receive apps and configurations

**Sources**:
- [Apple Business Manager](https://business.apple.com/)
- [ADE for iOS in Intune](https://learn.microsoft.com/en-us/mem/intune/enrollment/device-enrollment-program-enroll-ios)

---

### Related Scenario 4: Provisioning Packages for Air-Gapped or Offline Environments

**Challenge**: Enroll Windows devices in locations with no internet access (factory floor, secure facilities).

**Solution**:

1. Use **Windows Configuration Designer** (from the Microsoft Store) to create a provisioning package (`.ppkg`)
2. Include enrollment settings, Wi-Fi profiles, certificates, and initial app configurations
3. Apply the `.ppkg` via USB drive during OOBE or after Windows setup
4. **Note**: The enrollment token expires after **180 days** — plan package refresh cycles accordingly

**Sources**:
- [Windows Configuration Designer](https://learn.microsoft.com/en-us/windows/configuration/provisioning-packages/provisioning-install-icd)
- [Bulk enrollment with provisioning packages](https://learn.microsoft.com/en-us/mem/intune/enrollment/windows-bulk-enroll)

---

## Tools and Resources Used

| Tool / Resource | Description | Link |
|---|---|---|
| **Microsoft Intune** | Cloud-native MDM and MAM solution | [intune.microsoft.com](https://intune.microsoft.com) |
| **Microsoft Entra ID** | Identity and access management (formerly Azure AD) | [entra.microsoft.com](https://entra.microsoft.com) |
| **Microsoft 365 Admin Center** | License assignment and user management | [admin.microsoft.com](https://admin.microsoft.com) |
| **Apple Business Manager (ABM)** | Zero-touch iOS/macOS device deployment | [business.apple.com](https://business.apple.com) |
| **Apple Push Notification Service (APNs)** | Enables Intune communication with Apple devices | Configured in Intune admin center |
| **Managed Google Play** | Android Enterprise app management integration | Configured in Intune admin center |
| **Windows Autopilot** | Zero-touch Windows device provisioning | [Autopilot docs](https://learn.microsoft.com/en-us/autopilot/windows-autopilot) |
| **Windows Configuration Designer** | Creates provisioning packages for bulk/offline enrollment | [Microsoft Store / Docs](https://learn.microsoft.com/en-us/windows/configuration/provisioning-packages/provisioning-install-icd) |
| **Microsoft Configuration Manager** | On-premises management tool; used in co-management scenarios | [ConfigMgr docs](https://learn.microsoft.com/en-us/mem/configmgr/) |
| **Microsoft Defender for Endpoint** | Advanced threat protection; integrates with Intune compliance | [Defender docs](https://learn.microsoft.com/en-us/defender-endpoint/) |
| **Privileged Identity Management (PIM)** | Just-in-time role elevation for Entra ID | [PIM docs](https://learn.microsoft.com/en-us/entra/id-governance/privileged-identity-management/pim-configure) |
| **Conditional Access** | Policy engine to enforce device compliance for resource access | [CA docs](https://learn.microsoft.com/en-us/entra/identity/conditional-access/) |
| **Microsoft Entra Connect / Cloud Sync** | Directory synchronization for hybrid environments | [Entra Connect docs](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/whatis-azure-ad-connect) |
| **License Assignment documentation** | Step-by-step for assigning Intune licenses | [Licenses assign](https://learn.microsoft.com/en-us/intune/fundamentals/licenses-assign) |

---


