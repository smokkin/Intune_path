# Protect Data and Control Access with Microsoft Intune and Conditional Access

> `covers how Microsoft Intune **app protection policies (MAM)** and Microsoft Entra **Conditional Access** work together to protect organizational data and enforce zero-trust access control.`

---

## 1. Introduction

Contoso's devices are enrolled and secured with configuration and compliance policies. The next challenge is **protecting corporate data** on both managed and unmanaged devices and **controlling which devices** can access sensitive resources like email and corporate documents.

This module covers how to protect organizational data using **app protection policies** (also called Mobile Application Management or MAM policies) and control resource access using **Microsoft Entra Conditional Access** policies. These capabilities work together to provide comprehensive data protection and zero-trust access control.

### 1.1 Contoso's Scenario

Contoso needs to address several data protection and access control requirements:

- **Protect data on personal devices**: Employees use personal mobile devices to access work email and documents. Contoso needs to prevent corporate data from being copied to personal apps without enrolling these devices in Intune.
- **Enforce device compliance for access**: Only compliant, corporate-managed devices should access sensitive SharePoint sites and financial applications.
- **Require multi-factor authentication**: High-risk sign-ins and access from untrusted locations should require additional authentication steps.
- **Block access from noncompliant devices**: Devices that fail compliance checks should be automatically blocked from accessing corporate resources until remediated.

### 1.2 In This Module

This module covers how to protect data and control access using two key technologies:

1. **App protection policies (MAM)**: Secure organizational data within apps on managed and unmanaged devices by controlling data leakage, encryption, and app-level security.
2. **Conditional Access policies**: Control who can access corporate resources based on identity, device compliance, location, risk level, and application sensitivity.

You learn to configure these policies, integrate them with device compliance, and monitor their effectiveness in protecting Contoso's data.

### 1.3 Learning Objectives

By the end of this module, you're able to:

- Create and assign app protection policies to protect data on iOS, Android, and Windows devices
- Configure Conditional Access policies that enforce compliance and authentication requirements
- Integrate device compliance status with Conditional Access for automated access control
- Monitor app protection policy effectiveness and user adoption
- Troubleshoot common data protection and access control issues

---

## 2. App Protection Policies (MAM)

App protection policies (also called **Mobile Application Management** or **MAM** policies) protect organizational data at the application level **without requiring device enrollment**. They control how corporate data is accessed, shared, and stored within apps, making them ideal for bring-your-own-device (BYOD) scenarios and unmanaged devices.

App protection policies are available for **iOS/iPadOS**, **Android**, and **Windows** devices, though **Windows MAM has more limited capabilities** and is best suited for contractor-owned devices or shared workstations where full device enrollment isn't feasible.

### 2.1 What Are App Protection Policies?

App protection policies define rules that govern organizational data within specific applications. Unlike device-level configuration policies that require device enrollment (Mobile Device Management or MDM), app protection policies work with or without enrollment, focusing on securing data within apps rather than controlling the entire device.

**Key characteristics:**

- **App-level protection**: Secure data within specific apps (Outlook, Teams, OneDrive, Office) without managing the entire device
- **Work without enrollment**: Protect data on unmanaged personal devices where users don't want corporate device management
- **Identity-driven**: Policies apply when users sign in with their corporate identity (Microsoft Entra account)
- **Data isolation**: Separate corporate data from personal data, preventing data leakage between work and personal apps
- **Platform support**: Available for iOS/iPadOS, Android, and Windows (MAM for Windows)

### 2.2 App Protection Policy Settings

App protection policies include comprehensive controls organized into several categories.

#### Data protection settings

Control how organizational data is transferred and stored:

- **Data transfer**: Prevent copying/pasting corporate data to personal apps, restrict "Open in" operations, and control data backup
- **Encryption**: Require app-level encryption for organizational data at rest
- **Screenshot prevention**: Block screenshots and screen capture of corporate data (iOS/iPadOS and Android)
- **Approved keyboards**: Restrict which third-party keyboards can access corporate data (iOS/iPadOS and Android)
- **Backup protection**: Block organizational data backup to iTunes, iCloud (iOS), or Android backup services
- **Clipboard protection**: Control copy/paste operations between protected and unprotected apps (all platforms)

**Platform-specific capabilities:**

- **iOS/iPadOS**: Open-In management controls data sharing via Share Sheet, Universal Links control, biometric authentication (Touch ID/Face ID)
- **Android**: SafetyNet attestation for device integrity, work profile encryption, Google Play Protect integration
- **Windows**: Network boundary enforcement (corporate network vs. personal), file protection based on organizational identity, limited to Microsoft apps

#### Access requirements

Define authentication and security requirements for app access:

- **PIN for access**: Require app-level PIN or biometric authentication to access corporate data
- **Corporate credentials**: Require Microsoft Entra sign-in credentials to access the app
- **Minimum OS version**: Block app access on devices running outdated operating systems
- **Jailbreak/root detection**: Block access from compromised devices
- **Max PIN attempts**: Lock or wipe app data after repeated failed PIN attempts

#### Conditional launch

Automatically respond to device conditions by warning users or blocking access. Conditions vary by platform based on security capabilities.

**Key conditional launch conditions:**

| Condition                    | iOS/iPadOS | Android | Windows | Example Use Case                               |
| ----------------------------- | ---------- | ------- | ------- | ----------------------------------------------- |
| **Min OS version**            | ✓          | ✓       | ✓       | Block iOS <15.0, Android <10.0, Windows <19041  |
| **Jailbreak/Root detection**  | ✓          | ✓       | –       | Block access from compromised devices           |
| **SafetyNet/Play Protect**    | –          | ✓       | –       | Verify device integrity (Android only)          |
| **Threat level**              | ✓          | ✓       | ✓       | Block if Microsoft Defender detects high risk   |
| **Offline grace period**      | ✓          | ✓       | ✓       | Wipe data if device offline for 90 days         |
| **Disabled account**          | ✓          | ✓       | ✓       | Remove data when user leaves organization       |
| **Google Play Services**      | –          | ✓       | –       | Ensure services available (Android only)        |

**Actions available**: Warn (user notification), Block access (app access prevented), Wipe data (organizational data removed).

**Why conditional launch matters**: These conditions create an automatic response system — devices failing security checks lose access immediately without IT intervention. For example, when a user jailbreaks their iPhone to install unauthorized apps, conditional launch automatically blocks corporate email access until the device is restored.

> **Note**
> Windows app protection policies have fewer conditional launch options compared to mobile platforms. Consider using device compliance policies integrated with Conditional Access for comprehensive Windows device health checks.

#### Health checks

Validate device and app health before allowing access:

- **Max OS version**: Warn users running pre-release or beta OS versions
- **SafetyNet attestation** (Android): Verify device integrity using Google Play Protect
- **Require Microsoft Intune app protection policy**: Ensure the Intune app wrapper or SDK is active

### 2.3 Create App Protection Policies (General Steps)

To create an app protection policy in Microsoft Intune:

1. **Navigate to app protection policies**: Intune admin center > Apps > App protection policies > Create policy > Select platform (iOS/iPadOS, Android, or Windows)
2. **Configure basic settings**:
   - **Name**: Descriptive policy name (e.g., "iOS App Protection - Finance Department" or "Windows MAM - Contractors")
   - **Description**: Document purpose and scope
   - **Enrollment state** (Windows only): Choose whether to protect enrolled devices, unenrolled devices, or both
3. **Select target apps**: Choose which apps receive protection:
   - **Microsoft apps**: Outlook, Teams, OneDrive, Word, Excel, PowerPoint, OneNote, Microsoft Edge, SharePoint, Planner
   - **Public store apps**: Additional apps that support Intune SDK (availability varies by platform)
   - **Custom apps**: Line-of-business apps integrated with Intune SDK
   - **Windows limitation**: Only Microsoft apps are supported
4. **Configure data protection**:
   - Send org data to other apps: **Policy managed apps**
   - Receive data from other apps: **All apps** or **Policy managed apps**
   - Save copies of org data: **Block**
   - Restrict cut, copy, and paste: **Policy managed apps with paste in**
   - Encrypt org data: **Require** (or **Yes** for Windows)
   - Platform-specific settings:
     - **iOS**: Prevent iTunes and iCloud backups, block third-party keyboards
     - **Android**: Block screen capture and Google Assistant, specify approved keyboards
     - **Windows**: Network boundary enforcement settings
5. **Configure access requirements**:
   - PIN for access: **Require** (iOS/Android only; Windows doesn't support app-level PIN)
   - PIN type: **Numeric** or **Passcode**
   - Biometric authentication: **Allow** Touch ID/Face ID (iOS) or Fingerprint (Android)
   - Corporate credentials for access: **Require**
   - Recheck access requirements after (minutes): **30**
6. **Configure conditional launch**:
   - Min OS version: **15.0** (iOS), **10.0** (Android), or **10.0.19041** (Windows) — Action: **Block access**
   - Device integrity checks:
     - **iOS**: Block jailbroken devices
     - **Android**: Block rooted devices, SafetyNet device attestation
     - **Windows**: Limited device health checks
   - Max allowed device threat level: **Medium** (Block access)
   - Offline grace period: **90 days** (Wipe data for iOS/Android, Block access for Windows)
7. **Assign to groups**: Target user groups who need protection (e.g., "All Users," "Finance Department," or "Contractors")
8. **Review + create**: Validate settings and deploy the policy

> **Note**
> Windows app protection policies have fewer capabilities than iOS and Android. Windows MAM doesn't support app-level PIN, has limited conditional launch options, and only works with Microsoft apps. For comprehensive Windows device management, use device enrollment (MDM) combined with configuration and compliance policies.

### 2.4 MAM for Windows Capabilities

Windows app protection policies (MAM for Windows) extend data protection to Windows 10/11 devices without requiring full device enrollment. This capability is particularly useful for contractor-owned devices or shared workstations.

**Key features for Windows:**

- **Application-level protection**: Protect data in Microsoft Edge, Office apps, and OneDrive
- **Windows Information Protection (WIP) successor**: MAM for Windows replaces the deprecated WIP feature
- **Clipboard protection**: Control copy/paste operations between protected and unprotected apps
- **Network boundary enforcement**: Protect data based on network location and domain membership
- **Unmanaged device support**: Secure data on personal Windows devices without MDM enrollment

**Limitations compared to iOS/Android:**

- Fewer supported apps (primarily Microsoft apps)
- No screenshot protection
- No biometric authentication options (PIN not supported)
- Limited conditional launch conditions
- Requires Windows 10/11 Pro or Enterprise

**When to use Windows MAM:**

- Contractor or temporary worker devices you can't enroll
- Shared workstations in kiosks or common areas
- Partner organization devices accessing your resources
- Personal devices where users resist full device enrollment

**When to use MDM instead:**

- Corporate-owned Windows devices
- Devices requiring comprehensive security controls
- Devices needing application deployment and updates
- Devices requiring compliance policy enforcement

### 2.5 App Protection Policies for Managed Devices

While app protection policies are designed for unmanaged devices, they also provide an additional layer of protection for managed (MDM-enrolled) devices. This "defense in depth" approach ensures data protection even if a device becomes unenrolled or falls out of compliance.

**Benefits of combining MAM + MDM:**

- **Layered protection**: Device-level controls (MDM) plus app-level controls (MAM)
- **Enrollment gap coverage**: Protect data immediately after device enrollment begins but before MDM policies apply
- **Compliance gaps**: Maintain app-level protection even if device temporarily loses compliance
- **User-owned apps**: Protect corporate data in sideloaded or personal store apps on corporate devices

### 2.6 Monitor App Protection Policies

Monitor policy effectiveness and user adoption in the Intune admin center.
<img width="1148" height="617" alt="image" src="https://github.com/user-attachments/assets/af2abcde-4a82-41d5-9a92-67b00c8a843b" />

#### App protection status report

View overall policy deployment and user adoption:

- **Protected users**: Count of users with at least one protected app
- **Platform distribution**: iOS, Android, and Windows user breakdown
- **Flagged users**: Users with jailbroken/rooted devices or other violations
- **User status**: Detailed per-user app protection status

**Navigation**: Intune admin center > Apps > Monitor > App protection status

#### App configuration per policy

Drill into specific policy effectiveness:

- **User check-ins**: Number of users actively using protected apps
- **App check-ins**: Number of app instances receiving the policy
- **Platform breakdown**: iOS, Android, Windows distribution
- **Last check-in**: Most recent policy application timestamp

**Navigation**: Intune admin center > Apps > App protection policies > Select policy > Properties > Assignments

#### User app protection status

View individual user compliance with app protection requirements:

- **Protected apps**: List of apps receiving protection on user's devices
- **Check-in date**: Last time the app checked policy status
- **Device platform**: iOS, Android, Windows
- **Policy compliance**: Whether user meets all access requirements

**Navigation**: Intune admin center > Apps > Monitor > User status > Select user

### 2.7 Troubleshoot App Protection Policies

| Issue                               | Cause                               | Resolution                                          |
| ------------------------------------ | ------------------------------------ | ----------------------------------------------------- |
| Policy not applying                  | App not MAM-enabled                  | Verify app supports Intune SDK or app wrapping        |
| User can't access app                | Conditional launch blocking          | Check device health, OS version, and threat level     |
| Data transfer blocked unexpectedly   | Policy too restrictive               | Review "Send org data to other apps" setting          |
| PIN not prompting                    | Access requirements not configured   | Enable "PIN for access" in policy settings            |
| App data not encrypted               | Encryption disabled                  | Set "Encrypt org data" to Require                      |

**Diagnostic tools:**

- **Intune Company Portal app**: View applied policies and compliance status on the device
- **Microsoft Intune app** (Android): Access diagnostic logs for MAM issues
- **Intune Troubleshooting**: Intune admin center > Troubleshooting + support > Select user > App protection policies
- **App protection logs**: Review detailed policy application events in Intune logs

### 2.8 Best Practices for App Protection Policies

These practices support Zero Trust's **least privilege access** principle by limiting app permissions and data access to only what's necessary, while **assuming breach** through robust data encryption and device integrity checks.

**Platform-specific best practices:**

- **iOS/iPadOS**: Enable biometric authentication (Touch ID/Face ID) for better user experience, test third-party App Store apps for Intune SDK support, monitor jailbreak detection, and leverage Open-In management to control data flow without over-restricting productivity
- **Android**: Use SafetyNet attestation to validate device integrity, test across different Android Enterprise enrollment types, monitor root detection, and plan for manufacturer-specific behaviors (Samsung, Google, OnePlus devices may handle policies differently)
- **Windows**: Understand MAM limitations compared to mobile platforms, use for specific scenarios (contractor devices, shared workstations), prefer MDM for corporate-owned devices, and communicate network requirements clearly to users

**Universal best practices (all platforms):**

1. **Start with targeted pilots**: Deploy to a small group before rolling out organization-wide
2. **Use warning actions first**: Set conditional launch to "Warn" before "Block" to assess impact
3. **Layer MAM and MDM**: Apply app protection to both managed and unmanaged devices for defense in depth
4. **Monitor flagged users**: Regularly review compromised devices and policy violations
5. **Educate users**: Communicate why app protection is necessary and how it protects their personal privacy
6. **Test data transfer scenarios**: Validate that legitimate workflows (copy/paste, open in) work as expected
7. **Plan for offline scenarios**: Set reasonable offline grace periods (90 days typical)
8. **Update policies regularly**: Add new apps as they're adopted and adjust settings based on usage patterns

---

## 3. Conditional Access Policies

Microsoft Entra Conditional Access acts as the **policy enforcement engine** for zero-trust security. It evaluates signals from identity, device, location, and application context to make real-time access control decisions. When integrated with Intune device compliance, Conditional Access ensures only secure, compliant devices can access organizational resources.

### 3.1 What Is Conditional Access?

Conditional Access policies answer the question: **"Should this user be allowed to access this resource from this device under these conditions?"**

These policies evaluate multiple signals before granting access:

| Signal Type     | Evaluation Question                   | Examples                                              |
| ---------------- | --------------------------------------- | -------------------------------------------------------- |
| **Identity**     | Who is requesting access?               | User, group, directory role                               |
| **Device**       | Is the device compliant and trusted?    | Compliance status, platform, managed state                |
| **Location**     | Where is the request coming from?       | Trusted network, country/region, IP address                |
| **Risk**         | Is this request risky?                  | Entra ID Protection risk scores, anomalous behavior         |
| **Application**  | What resource is being accessed?        | Microsoft 365, SaaS apps, line-of-business apps              |

Based on these signals, Conditional Access can enforce different actions:

| Access Control Action              | Description                                               | Use Case                                    |
| ------------------------------------ | ------------------------------------------------------------ | ---------------------------------------------- |
| **Allow access**                     | Grant access with no additional requirements                  | Trusted location, low-risk sign-in              |
| **Block access**                     | Deny access completely                                        | High-risk location, legacy authentication       |
| **Require MFA**                      | Demand additional authentication proof                        | All sign-ins, high-risk scenarios                |
| **Require compliant device**         | Allow access only from Intune-managed compliant devices        | Email and document access                        |
| **Require hybrid joined device**     | Require domain-joined devices synchronized to Entra ID          | On-premises resource access                       |
| **Require approved client app**      | Allow access only through managed apps                          | BYOD with app protection policies                  |

### 3.2 Conditional Access Policy Structure

Every Conditional Access policy has two main components.

#### 1. Assignments (Conditions)

Define **WHO** is targeted and **WHEN** the policy applies.

**Users and groups:**

- Include/exclude specific users, groups, or directory roles
- Example: Include "All Users", Exclude "IT Administrators" and "Break Glass Accounts"

**Cloud apps or actions:**

- Specific apps (Exchange Online, SharePoint, Teams)
- All cloud apps
- User actions (register security info, register/join devices)

**Conditions:**

| Condition Type           | Options                                                       | Notes                                    |
| -------------------------- | ----------------------------------------------------------------- | -------------------------------------------- |
| **Sign-in risk**           | High, medium, low                                                  | Requires Microsoft Entra ID Protection         |
| **Device platforms**       | iOS, Android, Windows, macOS, Linux                                 | Target specific operating systems              |
| **Locations**              | Trusted networks, specific countries/regions, all locations         | Define named locations in Entra ID              |
| **Client apps**            | Browser, mobile apps, desktop clients, legacy auth                  | Control authentication protocols                |
| **Filter for devices**     | Device name, model, ownership, compliance                           | Advanced property-based filtering                |

#### 2. Access controls (Actions)

Define **WHAT** happens when conditions are met.

**Grant controls** (allow access with requirements):

- Require multifactor authentication
- Require device to be marked as compliant (Intune compliance)
- Require Microsoft Entra hybrid joined device
- Require approved client app (app protection policies)
- Require app protection policy (MAM)
- Require password change

**Session controls** (ongoing monitoring):

- Use app-enforced restrictions (limited SharePoint/Exchange access)
- Use Conditional Access App Control (Microsoft Defender for Cloud Apps monitoring)
- Sign-in frequency (force re-authentication after time period)
- Persistent browser session (keep user signed in)
  <img width="1311" height="926" alt="image" src="https://github.com/user-attachments/assets/cad2ebd9-0691-4b31-a1b3-e48cbfc6dfa8" />

### 3.3 Integrate Device Compliance with Conditional Access

The most powerful integration for Intune administrators is requiring device compliance for resource access. This ensures only devices meeting your security standards can access corporate data.

#### How compliance integration works

1. **Device enrolls in Intune**: User enrolls device or device is automatically enrolled via Autopilot
2. **Compliance policy evaluates device**: Intune checks BitLocker, firewall, antivirus, OS version, etc.
3. **Compliance status sent to Microsoft Entra ID**: Intune reports compliant/noncompliant status to Entra ID
4. **User attempts resource access**: User tries to access Exchange, SharePoint, or other protected resource
5. **Conditional Access evaluates compliance**: Policy checks if device is marked compliant in Entra ID
6. **Access decision**: Grant access (compliant) or block access (noncompliant)

This creates a **continuous compliance enforcement loop**: devices must remain compliant to maintain access. If a device falls out of compliance (e.g., antivirus disabled), access is automatically revoked.

#### Create a compliance-based Conditional Access policy

**Navigation**: Microsoft Entra admin center > **Protection** > **Conditional Access** > **Policies** > **New policy**

| Configuration Area                       | Setting                                                                                | Recommendation                                                               | Rationale                                                                                                  |
| ------------------------------------------ | ---------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Assignments – Users**                    | Include "All users", Exclude "Break Glass Accounts"                                       | Always exclude emergency access accounts                                          | Prevents complete organizational lockout if compliance system fails                                          |
| **Assignments – Cloud apps**                | Select specific apps (Exchange Online, SharePoint) OR "All cloud apps"                     | Start with email/docs (high data sensitivity), expand to all apps later            | Strategic approach: protect sensitive data first, then broader enforcement                                     |
| **Assignments – Device platforms**          | Include all platforms (Android, iOS, Windows, macOS) where compliance policies exist        | Target all platforms where users access resources                                  | Users access resources from multiple devices; inconsistent enforcement creates gaps                              |
| **Access controls – Grant**                 | "Require device to be marked as compliant"                                                  | Enforce that only devices meeting security standards can access data                | Ensures BitLocker, antivirus, OS version, and other security controls are active                                  |
| **Access controls – Multiple controls**     | "Require one" (compliance OR MFA) vs. "Require all" (compliance AND MFA)                    | "Require all" for sensitive data (Finance), "Require one" for broader apps          | Balance security depth with user productivity                                                                     |
| **Enforcement mode**                        | Report-only mode → Test for 1-2 weeks → On (enforce)                                       | Always test with report-only first                                                | Identifies how many users would be blocked; allows compliance remediation before enforcement to avoid mass lockouts and help desk overflow |

> **Common mistake**: Enabling enforcement immediately without report-only testing causes mass user lockouts and help desk overflow.

### 3.4 Test with Report-only Mode

Before enforcing new Conditional Access policies, use **Report-only mode** to assess impact without blocking users:

- Policies in report-only mode evaluate conditions but don't enforce access controls
- View simulation results: Conditional Access > Insights and reporting > Conditional Access insights
- Review affected users and sign-ins before enabling enforcement
- Identify potential issues (e.g., users with noncompliant devices who need remediation time)
<img width="1699" height="1379" alt="image" src="https://github.com/user-attachments/assets/9d7f3826-cdd1-4db1-8ae3-2ef824f4e617" />

### 3.5 Common Conditional Access Policy Scenarios

| Scenario                              | Users                                  | Cloud Apps                          | Conditions                                              | Access Control                                        | Use Case                                          |
| --------------------------------------- | ----------------------------------------- | -------------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------ |
| **MFA for all users**                    | All users (exclude break glass)            | All cloud apps                          | Any                                                            | Grant: Require MFA                                              | Baseline security for all access                          |
| **Compliant devices for email/docs**     | All users                                  | Exchange Online, SharePoint Online       | Platforms: iOS, Android, Windows, macOS                          | Grant: Require compliant device                                   | Prevent access from unmanaged devices                       |
| **Block untrusted locations**            | All users (exclude emergency)              | All cloud apps                          | Locations: High-risk countries/regions                            | Block access                                                      | Data residency compliance, geofencing                       |
| **Require approved apps**                | All users                                  | Exchange Online, SharePoint Online       | Platforms: iOS, Android; Client apps: Mobile/desktop               | Grant: Require approved app + app protection policy               | BYOD with app-level protection                              |
| **High-risk sign-ins require MFA**       | All users                                  | All cloud apps                          | Sign-in risk: High, Medium                                          | Grant: Require MFA                                                | Adaptive authentication for suspicious activity              |
| **Block legacy authentication**          | All users (exclude service accounts)        | All cloud apps                          | Client apps: Exchange ActiveSync, legacy protocols                  | Block access                                                      | Eliminate legacy auth attack vectors                          |

### 3.6 Monitor Conditional Access Policies

#### Sign-in logs

Review every authentication attempt and Conditional Access decision.

**Navigation**: Microsoft Entra admin center > Monitoring > Sign-in logs
<img width="979" height="708" alt="image" src="https://github.com/user-attachments/assets/731f5d4f-07d3-4526-81ac-c23c5731a1c8" />

#### Conditional Access insights

Analyze policy impact and usage patterns.

**Navigation**: Microsoft Entra admin center > Protection > Conditional Access > Insights and reporting
<img width="1672" height="1235" alt="image" src="https://github.com/user-attachments/assets/44b52609-ffe4-40b2-88fc-4bedb65a3bfa" />

#### Monitor device compliance status

Track device compliance tied to Conditional Access.

**Navigation**: Intune admin center > Devices > Monitor > Noncompliant devices

| Metric                          | Description                                                          |
| ---------------------------------- | -------------------------------------------------------------------------- |
| **Compliance status**              | Compliant vs. noncompliant device count                                     |
| **Noncompliance reasons**          | Which settings cause failures (BitLocker, antivirus, OS version)             |
| **Conditional Access blocks**      | How many access attempts blocked due to noncompliance                          |

This data helps identify compliance gaps that impact user productivity.

Microsoft also provides **pre-built policy templates** for common security scenarios, to help quickly deploy industry-standard policies.

### 3.7 Troubleshoot Conditional Access

| Issue                              | Cause                                          | Resolution                                                  |
| ------------------------------------ | -------------------------------------------------- | ---------------------------------------------------------------- |
| User blocked unexpectedly            | Device not marked compliant                          | Verify device enrolled and compliance policy applied               |
| MFA prompt every sign-in             | Session controls too strict                          | Adjust sign-in frequency or enable persistent session                |
| Policy not applying                  | User/device exclusion                                | Review policy assignments and exclusions                            |
| Compliant device still blocked       | Compliance status not synced                         | Force device sync in Company Portal app                              |
| Break glass account locked out       | Emergency access account included in policy           | Always exclude break glass accounts from all CA policies              |
<img width="1093" height="150" alt="image" src="https://github.com/user-attachments/assets/370b0671-8cfb-4b68-936f-5f25a79086cf" />
<img width="1547" height="1199" alt="image" src="https://github.com/user-attachments/assets/a0cce9c7-8853-4b72-9102-f2b089ad8036" />

### 3.8 Best Practices for Conditional Access

These practices reinforce Zero Trust's core principles: **verify explicitly** (continuous authentication), **use least privilege access** (grant minimum required permissions), and **assume breach** (monitor and respond to threats).

- **Always exclude break glass accounts**: Maintain emergency access accounts excluded from all policies (supports recovery while maintaining security)
- **Start with report-only mode**: Test new policies before enforcement to avoid unexpected lockouts (*verify explicitly* — validate impact before blocking)
- **Use named locations**: Define trusted networks for location-based policies (*verify explicitly* — geography as a trust signal)
- **Layer policies incrementally**: Start with MFA, then add compliance requirements, then app restrictions (*verify explicitly* — progressive trust verification)
- **Monitor sign-in logs regularly**: Identify blocked users and compliance gaps (*assume breach* — continuous monitoring for anomalies)
- **Document policy intent**: Use clear names and descriptions for each policy (operational excellence and audit compliance)
- **Plan for compliance grace periods**: Allow time for devices to sync and remediate issues (balance security with user productivity)
- **Communicate with users**: Explain why they're blocked and how to regain access (user enablement reduces help desk load)
- **Test on pilot groups**: Deploy to small groups before organization-wide rollout (risk mitigation through controlled deployment)
- **Review policies quarterly**: Remove unused policies and adjust based on evolving security needs (*assume breach* — adapt defenses to emerging threats)

---

## 4. Exercise: Protect Data and Control Access with Policies

In this exercise, you configure app protection policies to secure organizational data on mobile devices and create Conditional Access policies that require device compliance for resource access. This hands-on practice demonstrates how Contoso protects corporate data and enforces zero-trust access control.

> This exercise has platform pivots: **Windows**, **iOS/iPadOS**, and **Android**. All variants are included below.

### 4.1 Exercise Overview

In this exercise, you complete the following tasks:

1. Create an app protection policy for mobile devices
2. Assign app protection policies to user groups
3. Create a Conditional Access policy requiring device compliance
4. Test Conditional Access policy in report-only mode
5. Monitor app protection and Conditional Access effectiveness

**Estimated time to complete**: 13 minutes

### 4.2 Prerequisites

**Windows pivot:**

- Device compliance policies configured in Intune
- Administrative access to Microsoft Intune admin center
- Administrative access to Microsoft Entra admin center
- At least one enrolled and compliant Windows device
- Test user account with appropriate licenses (Microsoft 365 E5 or equivalent)

**iOS/iPadOS pivot:**

- Device compliance policies configured in Intune (optional for app protection)
- Administrative access to Microsoft Intune admin center
- Administrative access to Microsoft Entra admin center
- An iOS or iPadOS device (can be personal/BYOD — enrollment not required for app protection)
- Test user account with appropriate licenses (Microsoft 365 E5 or equivalent)

**Android pivot:**

- Device compliance policies configured in Intune (optional for app protection)
- Administrative access to Microsoft Intune admin center
- Administrative access to Microsoft Entra admin center
- An Android device (can be personal/BYOD — enrollment not required for app protection)
- Test user account with appropriate licenses (Microsoft 365 E5 or equivalent)

### 4.3 Task 1: Create an App Protection Policy

Configure app-level data protection for mobile devices. This protects corporate data in apps without requiring device enrollment.

#### iOS/iPadOS steps

1. Sign in to the **Microsoft Intune admin center** (`https://intune.microsoft.com`)
2. Navigate to **Apps** > **App protection policies**
3. Select **+ Create policy** > **iOS/iPadOS**
4. **Basics** tab:
   - Name: `iOS App Protection - Corporate Data`
   - Description: `Protects organizational data in Microsoft apps on iOS devices, managed and unmanaged`
   - Select **Next**
5. **Apps** tab — Select target apps:
   - Select **Select public apps**
   - Search for and add: Microsoft Outlook, Microsoft Teams, Microsoft OneDrive, Microsoft Word, Microsoft Excel, Microsoft PowerPoint, Microsoft Edge
   - Select **Next**
6. **Data protection** tab — Configure data controls:
   - **Data Transfer**:
     - Send org data to other apps: **Policy managed apps**
     - Receive data from other apps: **All apps**
     - Save copies of org data: **Block**
     - Allow user to save copies to selected services: **OneDrive for Business, SharePoint**
     - Restrict cut, copy, and paste between other apps: **Policy managed apps with paste in**
     - Cut and copy character limit for any app: **0** (no limit)
   - **Encryption**:
     - Encrypt org data: **Require**
   - **Functionality**:
     - Sync app with native contacts app: **Block**
     - Printing org data: **Block**
     - Restrict web content transfer with other apps: **Microsoft Edge**
     - Org data notifications: **Block org data**
   - Select **Next**
7. **Access requirements** tab — Configure authentication:
   - PIN for access: **Require**
   - PIN type: **Numeric**
   - Simple PIN: **Block**
   - PIN minimum length: **6**
   - PIN reset after number of days: **90**
   - Touch ID instead of PIN for access (iOS 8+): **Allow**
   - Face ID instead of PIN for access (iOS 11+): **Allow**
   - Override biometrics with PIN after timeout: **Require**
   - Timeout (minutes of inactivity): **30**
   - Corporate credentials for access: **Require**
   - Recheck the access requirements after (minutes of inactivity): **30**
   - Select **Next**
8. **Conditional launch** tab — Configure device conditions:
   - Min OS version < **15.0**: Block access
   - Jailbroken/rooted devices: Block access
   - Max allowed device threat level ≥ Medium: Block access
   - Offline grace period > 90 days: Wipe data
   - Disabled account: Block access
   - Select **Next**
9. **Assignments** tab:
   - Include: **All Users** (or select specific groups for Contoso)
   - Exclude: (leave empty for this exercise)
   - Select **Next**
10. **Review + create** tab: Review all settings, then select **Create**
<img width="1248" height="637" alt="image" src="https://github.com/user-attachments/assets/85d97b89-ed8b-43cd-af62-6b17db6140f4" />

**Result**: An app protection policy that protects corporate data on iOS devices without requiring device enrollment.

#### Android steps

1. Sign in to the **Microsoft Intune admin center** (`https://intune.microsoft.com`)
2. Navigate to **Apps** > **App protection policies**
3. Select **+ Create policy** > **Android**
4. **Basics** tab:
   - Name: `Android App Protection - Corporate Data`
   - Description: `Protects organizational data in Microsoft apps on Android devices, managed and unmanaged`
   - Select **Next**
5. **Apps** tab:
   - Select **Select public apps**
   - Search for and add: Microsoft Outlook, Microsoft Teams, Microsoft OneDrive, Microsoft Word, Microsoft Excel, Microsoft PowerPoint, Microsoft Edge
   - Select **Next**
6. **Data protection** tab — Configure data controls:
   - **Data Transfer**:
     - Send org data to other apps: **Policy managed apps**
     - Receive data from other apps: **All apps**
     - Save copies of org data: **Block**
     - Allow user to save copies to selected services: **OneDrive for Business, SharePoint**
     - Restrict cut, copy, and paste: **Policy managed apps with paste in**
   - **Encryption**:
     - Encrypt org data: **Require**
   - **Functionality**:
     - Sync app with native contacts app: **Block**
     - Printing org data: **Block**
     - Restrict web content transfer: **Microsoft Edge**
     - Org data notifications: **Block org data**
   - **Android-specific**:
     - Screen capture and Google Assistant: **Block**
   - Select **Next**
7. **Access requirements** tab:
   - PIN for access: **Require**
   - PIN type: **Numeric**
   - Simple PIN: **Block**
   - PIN minimum length: **6**
   - Biometric instead of PIN for access (Android 6.0+): **Allow**
   - Override biometrics with PIN after timeout: **Require**
   - Timeout: **30** minutes
   - Corporate credentials for access: **Require**
   - Recheck access requirements after: **30** minutes of inactivity
   - Select **Next**
8. **Conditional launch** tab:
   - Min OS version < **10.0**: Block access
   - Jailbroken/rooted devices: Block access
   - Max allowed device threat level ≥ Medium: Block access
   - Offline grace period > 90 days: Wipe data
   - Disabled account: Block access
   - **Android-specific**: SafetyNet device attestation: **Basic integrity and certified devices** (Block access)
   - Select **Next**
9. **Assignments** tab:
   - Include: **All Users**
   - Select **Next**
10. **Review + create**: Review settings and select **Create**
<img width="1148" height="637" alt="image" src="https://github.com/user-attachments/assets/9bdebe2d-1c5f-444b-8cf1-72f469d80723" />

**Result**: An app protection policy that protects corporate data on Android devices without requiring device enrollment.

#### Windows pivot

App protection policies are primarily designed for mobile platforms (iOS and Android). For Windows devices, use:

1. **Windows Information Protection (WIP)** policies for app-level data protection
2. **Configuration profiles** for device-level security settings
3. **Compliance policies** combined with Conditional Access for access control

For this exercise on Windows, **skip to Task 3** to configure Conditional Access policies that enforce device compliance.

### 4.4 Task 2: Verify App Protection Policy Assignments

> Applies to the iOS/iPadOS and Android pivots. For Windows devices, skip this task and proceed to Task 3.

Confirm policies are targeting the correct users and applications.

1. In **App protection policies**, select your app protection policy (iOS or Android)
2. Select **Properties** > **Assignments**
3. Verify:
   - **All Users** or your selected groups are included
   - No unintended exclusions
   - Protected app count: **7 apps**
4. Navigate to **Monitor** > **App protection status**
5. Review the dashboard:
   - **Total protected users**: Should show count of licensed users
   - **Platform breakdown**: Shows users by platform
   - **Flagged users**: Should be **0** initially (no jailbroken/rooted devices detected yet)

### 4.5 Task 3: Create a Conditional Access Policy Requiring Device Compliance

Configure zero-trust access control that blocks noncompliant devices from accessing corporate resources. **This applies to all platforms.**

1. Sign in to the **Microsoft Entra admin center** (`https://entra.microsoft.com`)
2. Navigate to **Protection** > **Conditional Access** > **Policies**
3. Select **+ New policy**
4. **Name**: `Require compliant device for Exchange and SharePoint`
5. **Assignments** section — **Users**:
   - Select **0 users and groups selected**
   - **Include** tab: Select **All users**
   - **Exclude** tab: Select **Users and groups** > Select your **break glass / emergency access account**
   - Select **Select**
6. **Target resources**:
   - Select **No target resources selected**
   - **Select what this policy applies to**: **Cloud apps**
   - **Include** tab: **Select apps**
   - Search for and select: **Office 365 Exchange Online**, **Office 365 SharePoint Online**
   - Select **Select**
7. **Conditions**:
   - **Device platforms**:
     - Configure: **Yes**
     - **Include** tab: **Select device platforms** > Check: Android, iOS, Windows, macOS
     - Select **Done**
   - **Locations**:
     - Configure: **Yes**
     - **Include** tab: **Any location**
     - **Exclude** tab: (leave empty for this exercise)
     - Select **Done**
8. **Access controls** section — **Grant**:
   - Select **0 controls selected**
   - Select **Grant access**
   - Check: **Require device to be marked as compliant**
   - For multiple controls: **Require one of the selected controls** (or **Require all** if combining with MFA)
   - Select **Select**
9. **Enable policy**:
   - **Report-only** (test first — recommended for production)
   - Or **On** (enforce immediately — for this exercise)
10. Select **Create**
<img width="1511" height="926" alt="image" src="https://github.com/user-attachments/assets/4ea6eb60-46c3-4798-8d3d-af05f3ae809b" />

**Result**: Users must have compliant devices to access Exchange Online and SharePoint Online. Noncompliant devices are automatically blocked.

### 4.6 Task 4: Test Conditional Access Policy with What If Tool

Simulate policy impact before full enforcement using the What If tool.

1. In **Conditional Access** > **Policies**, select **What If**
2. Configure the simulation:
   - **User or workload identity**: Select a test user, e.g. `user@contoso.com`
   - **Cloud apps, actions, or authentication context**: Select **Office 365 Exchange Online**
   - **Device platform**: Select **Windows**
   - **Device state (Preview)**: Select **Device marked compliant** (simulate compliant device)
3. Select **What If**
4. Review results:
   - **Policies that will apply**: Should show "Require compliant device for Exchange and SharePoint"
   - **Evaluation result**: **Access granted** (because device is compliant)
5. Test noncompliant scenario:
   - Change **Device state**: **Not marked compliant**
   - Select **What If** again
   - **Evaluation result**: **Access blocked** (policy denies noncompliant device)
<img width="1531" height="1077" alt="image" src="https://github.com/user-attachments/assets/eb475068-28c1-4398-ab38-062b28924e33" />

**Result**: The What If tool confirms the policy correctly grants access to compliant devices and blocks noncompliant devices.

### 4.7 Task 5: Monitor Conditional Access and App Protection Effectiveness

Review policy enforcement and identify any access issues.

1. **Monitor Conditional Access sign-ins**:
   - In Microsoft Entra admin center, navigate to **Monitoring** > **Sign-in logs**
   - Filter by:
     - **Conditional Access**: Success or Failure
     - **Application**: Office 365 Exchange Online or SharePoint
   - Review recent sign-in attempts: user, device platform and compliance status, Conditional Access result (Success, Block, Require compliance), failure reason (if blocked)
2. **Monitor device compliance (impacts Conditional Access)**:
   - Navigate to **Devices** > **Monitor** > **Noncompliant devices**
   - Review: count of noncompliant devices blocked by Conditional Access, compliance policy settings causing failures, devices requiring remediation
3. **Review app protection policy status**:
   - In Intune admin center, navigate to **Apps** > **Monitor** > **App protection status**
   - Review: protected users, platform distribution, flagged users
4. **Check user-specific app protection status**:
   - In Intune admin center, navigate to **Apps** > **Monitor** > **User status**
   - Select a test user
   - Review: list of protected apps installed on user's devices, last check-in date for each app, device platform for each app instance

**Verification checklist:**

- [ ] App protection policies show in **App protection status** dashboard
- [ ] Protected apps appear in user status monitoring
- [ ] Conditional Access policy appears in **Sign-in logs** evaluations
- [ ] Compliant devices successfully access Exchange and SharePoint
- [ ] Noncompliant devices are blocked (if tested)
- [ ] No unexpected user lockouts or access issues

### 4.8 Exercise Summary

You've configured data protection and access control for Contoso's environment.

**What you accomplished:**

- Created app protection policy securing Microsoft apps on mobile devices (iOS/iPadOS and Android)
- Deployed app protection policy to users
- Created Conditional Access policy requiring device compliance for Exchange and SharePoint
- Tested policy impact using What If simulation tool
- Monitored app protection and Conditional Access effectiveness

#### Contoso's data protection posture

| Security Layer              | Capability                                                                                                                       | Impact                                                  |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------ |
| **App-level protection**       | Corporate data in Microsoft apps encrypted and protected from leakage to personal apps (mobile platforms)                              | Data secured on 500+ BYOD devices                               |
| **BYOD support**                | Personal mobile devices access work apps without full device enrollment through app protection policies                                | Zero user resistance to security policies                        |
| **Zero-trust access**           | Only compliant, managed devices can access sensitive email and documents across all platforms                                          | 100% compliance enforcement for Exchange/SharePoint                |
| **Continuous compliance**       | Devices must maintain compliance to keep access; noncompliant devices automatically blocked                                              | 23 noncompliant devices blocked in first week                     |
| **Risk-based access**           | Conditional Access evaluates device, location, and identity signals before granting access                                              | Real-time access decisions based on context                        |
| **Platform flexibility**        | Windows devices use MDM-based compliance enforcement, mobile devices benefit from both app protection and compliance policies            | Consistent security across heterogeneous environment                |

---

## 5. Tools and Resources Used

The following tools, portals, and concepts are referenced in the source material:

- **Microsoft Intune admin center** — `https://intune.microsoft.com` (app protection policies, device compliance, monitoring)
- **Microsoft Entra admin center** — `https://entra.microsoft.com` (Conditional Access, sign-in logs, identity protection)
- **Microsoft Entra ID / Microsoft Entra ID Protection** — identity platform; provides sign-in risk signals used as Conditional Access conditions
- **Microsoft Intune Company Portal app** — used to view applied policies, compliance status, and force device sync
- **Microsoft Intune app (Android)** — used to access diagnostic logs for MAM issues
- **Windows Information Protection (WIP)** — legacy app-level data protection feature for Windows; **deprecated**, superseded by MAM for Windows
- **Windows Autopilot** — referenced as an automatic device enrollment method
- **Microsoft 365 apps** referenced as protectable/target apps: Outlook, Teams, OneDrive, Word, Excel, PowerPoint, OneNote, Microsoft Edge, SharePoint, Planner
- **Microsoft Defender for Cloud Apps** — referenced under Conditional Access session controls ("Conditional Access App Control")
- **Microsoft Defender** — referenced as the source of device "threat level" signals used in conditional launch
- **Office 365 Exchange Online** and **Office 365 SharePoint Online** — target cloud apps used in the exercise
- **Conditional Access What If tool** — built into the Microsoft Entra admin center, used for policy impact simulation
- **Conditional Access policy templates** — pre-built templates in the Microsoft Entra admin center for common scenarios
- **BitLocker** — referenced as a compliance check item (Windows disk encryption)
- **Google Play Protect / SafetyNet attestation** — Android device integrity verification referenced in conditional launch and health checks
- **Microsoft 365 E5 (or equivalent) licensing** — required test user license referenced in exercise prerequisites

No external libraries, SDKs (beyond "Intune SDK" referenced generically), APIs, or third-party tools were specified with version numbers or installation commands in the source. See [Source Notes](#8-source-notes).

---

## 6. Confirmed and Worked Scenarios (Applied Solutions)

These scenarios are directly confirmed by the walkthrough steps in the source module (Unit 4: Exercise).

### Scenario A — Protect corporate data on BYOD iOS devices without enrollment

**Problem**: Employees use personal iPhones/iPads to access Outlook, Teams, and OneDrive, but the organization cannot mandate full MDM enrollment of personal devices.

**Confirmed solution** (from source):

1. Create an **iOS/iPadOS app protection policy** in Intune (Apps > App protection policies > Create policy > iOS/iPadOS).
2. Target Microsoft apps (Outlook, Teams, OneDrive, Word, Excel, PowerPoint, Edge).
3. Configure data protection: block saving copies of org data outside policy-managed apps, require encryption, restrict cut/copy/paste to "Policy managed apps with paste in," block printing and native contacts sync.
4. Require a 6-digit PIN (or Touch ID/Face ID) with a 30-minute recheck interval.
5. Set conditional launch to block devices below iOS 15.0, jailbroken devices, and devices with a "Medium" or higher threat level; wipe data after a 90-day offline grace period.
6. Assign the policy to **All Users** (or a target group).
7. Verify via **Monitor > App protection status** that protected users and a flagged-user count of 0 appear correctly.

**Outcome (per source)**: Corporate data is encrypted and isolated from personal apps without requiring device enrollment.

**Applicable official source**: [Microsoft Intune app protection policies overview](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy)

### Scenario B — Protect corporate data on BYOD Android devices without enrollment

**Problem**: Same as Scenario A, but for Android devices, with the added need to verify device integrity (rooted devices, tampered OS).

**Confirmed solution** (from source):

1. Create an **Android app protection policy** (Apps > App protection policies > Create policy > Android).
2. Target the same Microsoft 365 apps.
3. Configure data protection identically to iOS, plus Android-specific: block screen capture and Google Assistant.
4. Require a 6-digit PIN or biometric, 30-minute recheck.
5. Conditional launch: block OS versions below 10.0, block rooted devices, require SafetyNet "Basic integrity and certified devices" attestation, block at Medium+ threat level, 90-day offline wipe.
6. Assign to All Users and verify via the App protection status dashboard.

**Outcome (per source)**: Same as iOS — encrypted, isolated corporate data without enrollment, with added device-integrity verification via SafetyNet/Play Protect.

**Applicable official source**: [Android app protection policy settings](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy-settings-android)

### Scenario C — Block noncompliant devices from Exchange and SharePoint (all platforms)

**Problem**: Sensitive resources (email, SharePoint documents) must be accessible only from devices that meet compliance standards (BitLocker, antivirus, OS version, etc.), regardless of platform.

**Confirmed solution** (from source):

1. In the Microsoft Entra admin center, create a new Conditional Access policy named, e.g., `Require compliant device for Exchange and SharePoint`.
2. Assign to **All users**, excluding the break-glass/emergency access account.
3. Target cloud apps: **Office 365 Exchange Online** and **Office 365 SharePoint Online**.
4. Set device platform conditions to include Android, iOS, Windows, and macOS, with location set to **Any location**.
5. Under Grant access controls, check **Require device to be marked as compliant**; choose "Require one" or "Require all" depending on whether MFA is also required.
6. Enable in **Report-only** mode first (recommended), then switch to **On** after validating impact.
7. Validate with the **What If** tool: simulate a compliant device (access granted) and a noncompliant device (access blocked).
8. Monitor via **Sign-in logs** and **Devices > Monitor > Noncompliant devices**.

**Outcome (per source)**: Compliant devices are granted access; noncompliant devices are automatically blocked. The source notes a hypothetical example metric of "23 noncompliant devices blocked in first week" for Contoso.

**Applicable official sources**:
- [Conditional Access: Require compliant or hybrid Microsoft Entra joined device](https://learn.microsoft.com/en-us/entra/identity/conditional-access/policy-compliant-device)
- [Conditional Access What If tool](https://learn.microsoft.com/en-us/entra/identity/conditional-access/troubleshoot-conditional-access-what-if)

### Scenario D — Windows devices: when MAM is skipped in favor of MDM + Conditional Access

**Problem**: The exercise's app protection policy steps (Task 1/Task 2) target iOS and Android only; Windows is explicitly excluded from those tasks in the source.

**Confirmed solution** (from source): For Windows, the module directs administrators to:

1. Use **Windows Information Protection (WIP)** for app-level protection (noted as deprecated, superseded by Windows MAM) — or rely on compliance + Conditional Access instead.
2. Use **configuration profiles** for device-level security settings.
3. Use **compliance policies combined with Conditional Access** (Scenario C above) as the primary access-control mechanism.
4. Skip directly to Task 3 (Conditional Access policy) for Windows devices.

**Outcome (per source)**: Windows data protection and access control are achieved primarily through MDM-based compliance enforcement and Conditional Access rather than MAM.

**Applicable official source**: [Windows Information Protection (WIP) overview](https://learn.microsoft.com/en-us/windows/security/operating-system-security/data-protection/windows-information-protection/protect-enterprise-data-using-wip)

---

## 7. Related Scenarios and Solutions

These scenarios extend naturally from the source content but are **not** literally walked through step-by-step in the source; they are standard, well-documented Microsoft patterns related to the same features. They are marked as related/extended rather than "confirmed from source."

### Related Scenario 1 — Requiring both MFA and compliant device for highly sensitive apps (e.g., Finance)

**Solution outline**: Create a Conditional Access policy scoped to the Finance app(s)/group, with Grant controls set to **Require all the selected controls**, selecting both **Require multifactor authentication** and **Require device to be marked as compliant**. The source's table directly recommends "Require all" for sensitive data such as Finance, contrasted with "Require one" for broader apps.

**Applicable source**: [Common Conditional Access policies](https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-conditional-access-policy-common)

### Related Scenario 2 — Blocking legacy authentication organization-wide

**Solution outline**: Create a Conditional Access policy targeting **All cloud apps**, with a condition on **Client apps** set to legacy authentication protocols (e.g., Exchange ActiveSync, other legacy clients), and a **Block access** grant control. This matches the "Block legacy authentication" row in the source's scenario table.

**Applicable source**: [Block legacy authentication with Conditional Access](https://learn.microsoft.com/en-us/entra/identity/conditional-access/policy-old-block-legacy-authentication)

### Related Scenario 3 — Geofencing / blocking access from high-risk countries

**Solution outline**: Define **named locations** in Microsoft Entra ID representing high-risk countries/regions, then create a Conditional Access policy targeting all cloud apps with a **Locations** condition including those named locations and a **Block access** grant control, excluding emergency accounts.

**Applicable source**: [Conditional Access: Block access by location](https://learn.microsoft.com/en-us/entra/identity/conditional-access/howto-conditional-access-policy-location)

### Related Scenario 4 — Adaptive MFA based on sign-in risk

**Solution outline**: With Microsoft Entra ID Protection enabled, create a Conditional Access policy with a **Sign-in risk** condition (High, Medium) and a **Require MFA** grant control, so only risky sign-ins are challenged rather than every sign-in — reducing user friction while maintaining security. This matches the source's "High-risk sign-ins require MFA" scenario.

**Applicable source**: [Conditional Access: Sign-in risk-based Conditional Access](https://learn.microsoft.com/en-us/entra/id-protection/concept-identity-protection-policies)

### Related Scenario 5 — Recovering from an over-restrictive app protection policy that blocks legitimate workflows

**Solution outline**: If users report blocked copy/paste or "Open in" workflows after deploying an app protection policy, review the **Data protection** tab settings — specifically "Send org data to other apps" and "Restrict cut, copy, and paste" — and relax from "Block" to "Policy managed apps with paste in" or similar, following the source's troubleshooting table entry ("Data transfer blocked unexpectedly → Policy too restrictive → Review 'Send org data to other apps' setting"). Always pilot changes with a test group before wide rollout per the source's best practices.

**Applicable source**: [Troubleshoot app protection policies](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policies-faq)

### Related Scenario 6 — Recovering a locked-out break-glass account

**Solution outline**: If an emergency access (break-glass) account is accidentally included in a Conditional Access policy and becomes locked out, an administrator with access to an unaffected privileged account should edit the policy to add the break-glass account to the **Exclude** list under Users, per the source's troubleshooting guidance ("Break glass account locked out → Emergency access account included in policy → Always exclude break glass accounts from all CA policies"). Organizations should maintain at least one break-glass account permanently excluded from all Conditional Access policies and monitored separately.

**Applicable source**: [Manage emergency access accounts in Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/security-emergency-access)

---
