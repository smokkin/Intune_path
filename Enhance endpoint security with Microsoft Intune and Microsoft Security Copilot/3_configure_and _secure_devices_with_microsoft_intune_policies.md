# Configure and Secure Devices with Microsoft Intune Policies

---

## 1. Introduction

Enrolling devices in Microsoft Intune gives you visibility and the ability to manage endpoints, but **enrollment alone doesn't secure your organization**. Without policies, enrolled devices remain unmanaged users can disable encryption, skip operating system updates, and access corporate resources from noncompliant devices. Traditional on-premises tools like Group Policy and manual configuration scripts cannot reach remote devices outside the corporate network and require significant administrative overhead.

Modern organizations face a critical challenge: how do you ensure consistent security standards across devices that work from anywhere? Employees connect from home offices, coffee shops, airports, and client sites. Personal devices (BYOD) access corporate email alongside company-owned laptops. Without centralized policy management, IT teams face an impossible task.

This is where **device configuration profiles** and **compliance policies** become essential:

- **Configuration profiles** deploy and enforce device settings automatically enabling BitLocker encryption, configuring firewall rules, setting password requirements, and deploying certificates regardless of whether the device connects to your corporate network.
- **Compliance policies** define security requirements that devices must meet before accessing corporate resources ensuring antivirus is active, disks are encrypted, operating systems are updated, and jailbroken devices are blocked.

Microsoft Intune provides cloud-native policy management operating entirely over the internet. Once a device enrolls, it receives policies automatically and reports compliance status in real time.

### Zero Trust Alignment

Configuration and compliance policies work together to support **Zero Trust security principles**:

| Zero Trust Principle | Policy Implementation |
|---|---|
| **Verify explicitly** | Compliance policies verify device security posture before granting access |
| **Use least privilege access** | Configuration profiles enforce security baselines and minimize attack surface |
| **Assume breach** | Automated remediation responds to compliance failures without manual intervention |

Combined with Conditional Access, these policies ensure only compliant, properly configured devices can access corporate data regardless of where users work.

---

### Contoso Scenario: From Enrollment to Policy

> **Context:** In previous modules, Contoso's IT team successfully configured Microsoft Entra ID for automatic enrollment, enrolled Windows devices through Microsoft Entra join, and validated device management. Now, the team needs to **configure device settings and enforce security standards** across all enrolled devices.

**Contoso's requirements:**
- **Standardize device settings**: Configure Windows settings like BitLocker encryption, Windows Defender Firewall, and password policies
- **Enforce compliance**: Ensure devices meet security requirements (antivirus enabled, disk encrypted, OS up to date)
- **Target specific groups**: Apply different policies to different departments (sales team gets VPN settings, finance team gets stricter encryption)
- **Automate remediation**: Fix common compliance issues automatically instead of relying on manual intervention

---

### What You Learn

In this module, you complete the essential policy configuration steps to secure and manage enrolled devices:

- Create and assign device configuration profiles to standardize device settings across Windows, iOS/iPadOS, and Android platforms
- Define and apply compliance policies that enforce security requirements and identify noncompliant devices
- Target policies precisely using dynamic groups, assignment filters, and applicability rules
- Validate that policies apply successfully and monitor compliance status across your device fleet
- Automate remediation for common compliance issues using PowerShell scripts and compliance actions

---

### Main Goal

By the end of this module, you can deploy and enforce security policies across your enrolled device fleet. You understand how to configure device settings, define compliance requirements, target policies effectively, and automate responses to compliance failures. Your devices transition from simply enrolled to **fully managed and compliant** with organizational security standards.

---

## 2. Create and Assign Configuration Profiles

Device configuration profiles allow you to manage settings on enrolled devices without requiring Group Policy or manual configuration. Configuration profiles define what settings should be applied to devices, and Intune enforces those settings automatically.

---

### What Are Configuration Profiles?

Configuration profiles are collections of device settings created in Intune and deployed to groups of devices or users. These profiles control device behavior, security settings, features, and functionality.

**Common use cases:**
- Configure Wi-Fi and VPN connections
- Enable BitLocker drive encryption
- Configure Windows Firewall rules
- Set password requirements and screen lock settings
- Configure email and certificate settings
- Customize device restrictions (block camera, removable storage, etc.)
- Deploy trusted certificates and SCEP/PKCS profiles

**Benefits:**
- **Centralized management** — Configure thousands of devices from a single console
- **Consistency** — Ensure all devices have the same settings
- **Automation** — Settings apply automatically when devices enroll or check in
- **Flexibility** — Different profiles for different groups (sales, finance, executives)
- **Compliance** — Enforce security standards across the organization

---

### Profile Types by Platform

#### Windows: Settings Catalog

The **Settings catalog** is the modern approach for configuring Windows devices. It provides a searchable list of thousands of settings in a user-friendly interface.

**Advantages:**
- Browse and search for specific settings by name
- See setting descriptions and supported OS versions
- No need to know CSP (Configuration Service Provider) paths
- Supports the latest Windows features and settings
- Easier to create and maintain than custom profiles

**When to use:**
- For all new Windows 10/11 configuration profiles
- When you need granular control over specific settings
- When you want to find settings without knowing the CSP path

#### iOS/iPadOS: Device Restrictions

The **Device restrictions** profile is the primary method for configuring iOS and iPadOS device settings.

**Common settings categories:**

| Category | Examples |
|---|---|
| **Password** | Passcode requirements, Touch ID/Face ID settings |
| **Built-in Apps** | Block or allow Safari, Camera, App Store, iTunes, FaceTime |
| **Cloud and Storage** | iCloud backup, Keychain, Photo Stream restrictions |
| **Connected Devices** | Bluetooth, USB accessories, AirPrint |
| **Wireless** | Wi-Fi, cellular data, personal hotspot restrictions |

#### Android: Device Restrictions

The **Device restrictions** profile is the primary method for configuring Android device settings. Coverage varies by enrollment type (fully managed, corporate-owned work profile, personally owned work profile).

**Common settings categories:**

| Category | Examples |
|---|---|
| **Password** | Password requirements, biometric authentication |
| **Work Profile** | Work profile settings, app permissions, notifications (critical for BYOD security) |
| **Security** | Encryption, developer options, unknown sources |
| **Connectivity** | Bluetooth, Wi-Fi, USB, mobile networks |

> **Key consideration:** Personally owned work profiles provide work profile settings only, maintaining user privacy while securing corporate data.

#### Templates

**Templates** are prebuilt profile types for common configuration scenarios.

| Template | Use Case |
|---|---|
| **Device restrictions** | Block features like camera, Bluetooth, removable storage |
| **Email** | Configure email accounts (Exchange, Gmail, Outlook) |
| **Wi-Fi** | Deploy Wi-Fi connection profiles |
| **VPN** | Configure VPN connections (IKEv2, L2TP, PPTP, automatic VPN) |
| **Trusted certificate** | Deploy root and intermediate CA certificates |
| **SCEP certificate / PKCS certificate** | Deploy client authentication certificates |
| **Endpoint protection** | Configure antivirus, firewall, and encryption settings |
| **Identity protection** | Configure Windows Hello for Business |

**When to use:** For common scenarios (Wi-Fi, VPN, email), when you prefer a guided wizard experience, or for backwards compatibility with older profiles.

#### Custom Profiles (OMA-URI)

**Custom profiles** allow you to configure settings not available in the Settings catalog or templates by specifying OMA-URI settings directly.

**When to use:**
- For advanced settings not yet in the Settings catalog
- When you need to configure CSPs directly
- For settings documented in Microsoft docs but not exposed in the UI

> **Note:** Microsoft recommends using the Settings catalog for new profiles whenever possible. Custom profiles should only be used for settings not available in the catalog.

---

### Create a Configuration Profile (Step by Step)

#### Step 1: Start Creating a Profile

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com)
2. Navigate to **Devices** > **Configuration**
3. Select **Create** > **New policy**
4. Choose:
   - **Platform**: Windows 10 and later
   - **Profile type**: Settings catalog
5. Select **Create**

> **Note:** For iOS/iPadOS, select **Platform: iOS/iPadOS** and **Profile type: Templates > Device restrictions**. For Android, select **Platform: Android Enterprise** and **Profile type: Templates > Device restrictions**, then choose your enrollment type.

#### Step 2: Provide Basic Information

1. On the **Basics** page, enter:
   - **Name**: Descriptive name (e.g., `Windows - BitLocker Encryption`)
   - **Description**: Optional but recommended (e.g., `Enables BitLocker with 256-bit XTS-AES encryption for all fixed drives`)
2. Select **Next**

#### Step 3: Configure Settings

**Windows Settings Catalog approach — key design decisions:**

| Decision | Detail |
|---|---|
| **Encryption method choice** (XTS-AES 256-bit vs 128-bit) | Stronger encryption (256-bit) protects data better but may impact older hardware performance. Most organizations choose 256-bit for new devices and 128-bit for legacy hardware. |
| **Group related settings** | BitLocker isn't one setting it's device encryption requirements, encryption methods for different drive types, and recovery key storage. Group related settings in one profile for easier troubleshooting. |
| **Search vs. browse** | Searching finds settings across categories ("password" returns results from BitLocker, Windows Hello, Account Protection). Browsing by category helps discover related settings you didn't know existed. |

**iOS/iPadOS common security settings:**
- Password requirements: Alphanumeric, minimum 8 characters, Touch ID/Face ID
- Built-in apps: Block Camera if needed for data security
- iCloud: Configure backup and sync restrictions

**Android common security settings:**
- Work profile password: Numeric minimum 6 digits
- Work profile restrictions: Block copy/paste between profiles, require encryption
- Device experience: Block screen capture in work profile

For detailed platform-specific configuration steps, see:
- [Configure iOS/iPadOS device settings](https://learn.microsoft.com/en-us/mem/intune/configuration/device-restrictions-ios)
- [Configure Android Enterprise device settings](https://learn.microsoft.com/en-us/mem/intune/configuration/device-restrictions-android-enterprise)

#### Step 4: Configure Assignments

**User vs. Device groups — why it matters:**

| Target Type | When to Use | Why |
|---|---|---|
| **Device groups** (BitLocker, firewall, certificates) | Security must be consistent regardless of who logs in | Settings persist for all users of a shared device |
| **User groups** (email, OneDrive, app configurations) | Settings are personal to the user's work experience | Settings follow the user across devices |

> **⚠️ Common Mistake:** Assigning device-level security to user groups means settings only apply when that specific user logs in. On shared devices, other users won't get the security settings.

**Exclusion groups** — critical for phased rollouts. Exclude pilot groups or known incompatible devices (e.g., exclude Windows 10 devices from a Windows 11-specific profile).

**Applicability rules (Windows only)** — filter by OS version or edition:
- Deploy Windows 11-only features without impacting Windows 10 devices
- Require BitLocker on Enterprise edition (where it's available) but not Pro
- Prevent settings from failing gracefully on incompatible devices

> **Note:** iOS/iPadOS and Android don't support applicability rules. Use assignment filters checking OS version properties instead.

**Assignment filters for advanced targeting:**

| Targeting Mechanism | Best For |
|---|---|
| **Dynamic groups** | Stable, reusable collections (all Windows 11 devices, all corporate-owned phones) |
| **Filters** | Policy-specific conditions (apply this VPN profile only to Sales laptops with 512 GB+ storage) |

**Filter rule example syntax (OData):**
```
(device.deviceTotalStorage -ge 256000000000)
```
This targets devices with 256 GB+ storage. Filters evaluate in **real-time** at policy application — no 24-hour wait like dynamic groups.

**Include vs. Exclude filter modes:**
- **Include** — "Only apply if filter matches" (allow list approach)
- **Exclude** — "Don't apply if filter matches" (block list approach)

**Combined example:** Assign to "All Devices" group + Include filter for "Windows 11 Enterprise only" → Every Windows 11 Enterprise device gets the policy automatically.

#### Step 5: Review and Create

1. On the **Review + create** page, verify all settings
2. Select **Create** to deploy the profile

The profile will apply to targeted devices at their next check-in (default **8-hour interval**, or when users manually sync).

---

### Manage and Monitor Configuration Profiles

#### Understanding Deployment Status

> **Why monitoring matters:** Configuration profiles fail silently — users don't see error messages, devices just don't get the settings. Without monitoring, you might believe BitLocker is deployed organization-wide when 30% of devices haven't received it.

| Status | Meaning | Common Causes |
|---|---|---|
| **Succeeded** | Setting applied successfully | Device received the profile and successfully configured the setting |
| **Failed** | Setting couldn't be applied | OS version too old, conflicting profiles, or setting dependencies not met (e.g., requiring TPM that doesn't exist) |
| **Pending** | Profile sent but device hasn't reported results yet | Device hasn't checked in (8-hour default interval), or setting requires a reboot that hasn't happened |
| **Conflict** | Multiple profiles configure the same setting with different values | Windows can't apply both "Screen timeout: 5 minutes" and "Screen timeout: 15 minutes" simultaneously |

**Three levels of status reporting:**

| Level | Use When |
|---|---|
| **Device status** | "Which devices need attention?" |
| **User status** | Troubleshooting user-targeted profiles like email |
| **Per-setting status** | Troubleshooting — tells you WHAT failed, not just that something failed |

> **💡 Tip:** Use Copilot in Intune prompts such as `Show me configuration profiles with the highest number of errors` or `Explain why the firewall profile failed on some devices` to accelerate root-cause analysis. Treat Copilot responses as advisory — verify with detailed reports before acting.

---

### Configuration Profile Best Practices

- **Use descriptive names** — Include platform, setting type, and target group (e.g., `Windows - BitLocker - All Devices`)
- **Start with Settings catalog** — Use Settings catalog for new profiles instead of custom OMA-URI settings
- **Test with pilot groups** — Assign profiles to test groups before deploying to all devices
- **Keep profiles focused** — Create separate profiles for different setting categories (one for BitLocker, one for Wi-Fi, etc.)
- **Use filters for advanced targeting** — Leverage assignment filters for granular control based on device properties
- **Monitor profile status** — Regularly check deployment status and address failures promptly
- **Document profile purpose** — Add clear descriptions explaining what each profile does and why
- **Avoid conflicts** — Ensure multiple profiles don't configure the same setting with different values
- **Use applicability rules** — Target specific OS versions or editions to avoid unsupported settings
- **Layer policies strategically** — Apply broad policies to all devices, then specific policies to subgroups

---

### Contoso's Configuration Profiles

| Profile | Platform | Key Settings | Assignment |
|---|---|---|---|
| BitLocker Encryption | Windows | XTS-AES 256-bit, OS + fixed drives | All Windows Devices |
| Wi-Fi Corporate | Windows | WPA2-Enterprise, certificate auth | All Windows Devices |
| VPN Configuration | Windows | IKEv2, vpn.contoso.com | Sales Team |
| Firewall Rules | Windows | Enable all profiles, block inbound | Finance Team + filter (Win11 Enterprise) |
| Passcode Requirements | iOS/iPadOS | Alphanumeric, 8 chars, Touch ID/Face ID | All iOS Devices |
| Work Profile Security | Android | 6-digit numeric, encryption, no screenshots | All Android Devices |

---

## 3. Create and Assign Compliance Policies

While configuration profiles manage device settings, compliance policies define **security requirements** that devices must meet. Compliance policies answer the question: *"Is this device secure enough to access corporate resources?"*

---

### What Are Compliance Policies?

Compliance policies are rules that define security and configuration requirements devices must meet to be considered compliant. These policies evaluate device health and security status, then mark devices as compliant or noncompliant.

**Common compliance requirements:**
- Antivirus must be enabled and up to date
- Disk encryption must be enabled (BitLocker on Windows)
- Device password or PIN must meet complexity requirements
- Operating system version must be within supported range
- Device must not be jailbroken or rooted
- Firewall must be enabled
- Device threat level must be below a threshold (integration with Microsoft Defender)

---

### Compliance vs. Configuration Profiles

| Feature | Compliance Policies | Configuration Profiles |
|---|---|---|
| **Purpose** | Evaluate and report device health | Configure device settings |
| **Action** | Mark device as compliant or noncompliant | Apply settings to device |
| **Enforcement** | Works with Conditional Access to block/allow access | Direct enforcement of settings |
| **User impact** | Can block access to corporate resources | Changes device behavior |
| **Remediation** | User must fix the issue (or use remediation scripts) | Settings are applied automatically |

**Example pairing:**
- **Configuration profile**: Sets BitLocker encryption settings (algorithm, key length, etc.)
- **Compliance policy**: Checks if BitLocker is enabled (yes/no evaluation)

> **💡 Naming tip to avoid confusion:**
> - Configuration profile: `Windows - BitLocker Configuration` (applies the settings)
> - Compliance policy: `Windows - BitLocker Required` (checks if it's enabled)

---

### Compliance Requirements by Platform

| Category | Windows | iOS/iPadOS | Android |
|---|---|---|---|
| **Device Health** | Require BitLocker; Require Secure Boot; Require code integrity | Block jailbroken devices | Block rooted devices; Require device at or under Device Threat Level |
| **Device Properties** | Min/Max OS version; Valid OS builds; Require encryption of data storage | Min/Max OS version | Min OS version; Min security patch level (YYYY-MM-DD) |
| **Password Requirements** | Require password; Block simple passwords; Password type (alphanumeric, numeric); Min length; Max inactivity timeout; Password expiration days; Prevent password reuse | Require password; Block simple passwords; Password type; Min length; Max inactivity timeout; Optional Touch ID/Face ID | Require password; Password type; Min length; Require encryption |
| **Security Features** | Require firewall; Require antivirus; Require antispyware; Defender security intelligence up-to-date | — | Google Play Protect SafetyNet attestation |
| **Threat Protection** | Microsoft Defender for Endpoint; Device risk score threshold (Low/Medium/High) | — | Google Play Protect integration |

**Key platform differences:**
- **Windows** — Focuses on enterprise security stack (BitLocker, Defender, Secure Boot, code integrity)
- **iOS/iPadOS** — Emphasizes Apple's hardware-backed security (jailbreak detection, biometric authentication via Secure Enclave)
- **Android** — Validates work profile isolation, Google Play Protect status, and security patch currency

---

### Actions for Noncompliance

> **Why graduated actions matter:** Immediately blocking noncompliant devices creates support burden. Users can't work, they panic-call the helpdesk, and IT spends time on preventable issues.

**Recommended graduated approach:**

| Timeline | Action | Rationale |
|---|---|---|
| **Immediately** | Mark device noncompliant | Sent to Entra ID for Conditional Access evaluation |
| **After 1 day** | Send email explaining the issue and how to fix it | Allows automated remediation scripts to run first |
| **After 3–7 days** | Send push notification via Company Portal | Escalate awareness |
| **After 7–14 days** | Remote lock device OR block corporate app access | Forces user action without destroying data; BYOD-friendly option |
| **After 30 days** | Retire device | Reserved for abandoned devices, separated employees, or persistent security risks |

> **💡 Make notifications actionable:** Customize templates with `{{DeviceName}}` and `{{UserName}}` variables. Include specific fix instructions like "Enable BitLocker via Settings > Update & Security > Device encryption" instead of generic "fix your device" messages.

**Assignment strategy:**
- Assign compliance policies to **device groups** (not user groups) — compliance evaluates the device's security posture, not the user's behavior
- Use **exclusion groups** for: test devices during policy design, legacy devices that are approved exceptions, and pilot groups during phased rollout

---

### View Compliance Status

**Key compliance states explained:**

| State | Meaning | Action Required |
|---|---|---|
| **Compliant** | Device meets all requirements | Trust this device for corporate resource access |
| **Not compliant** | Device fails one or more checks | Drill into per-setting status to understand severity — one failed setting = noncompliant |
| **Not evaluated** | No policies assigned or device hasn't checked in | **Dangerous** — often means forgotten/abandoned devices still on the network |
| **In grace period** | Noncompliant but within configured grace period | Users have time to fix issues before access restrictions apply |
| **Conflict** | Multiple policies make contradictory requirements | Indicates policy design problems — should not occur |

**Three levels of compliance reporting:**

| View | When to Use |
|---|---|
| **Organization-wide** | Executive reporting — aggregate health metrics (e.g., "92% of devices are compliant") |
| **Device-level** | Troubleshooting why a specific user can't access resources |
| **Per-policy** | Understanding adoption rate after rolling out a new policy |

---

### Integrate Compliance with Conditional Access

> **Key insight:** Compliance policies evaluate security posture, but **Conditional Access enforces consequences**. Without Conditional Access, devices can be noncompliant with no impact.

**How integration works:**

1. Device checks in to Intune (every 8 hours)
2. Compliance policies evaluate device state
3. Intune sends compliance status to Microsoft Entra ID
4. User attempts to access corporate resource (email, SharePoint, Teams)
5. Conditional Access policy checks Entra ID for device compliance status
6. Access granted (compliant) or blocked (noncompliant)

**Important:** Compliance evaluation (Intune) is separate from access enforcement (Conditional Access). This means:
- Compliance policies work without Conditional Access (evaluation only, no blocking)
- You can create policies, test them, and tune thresholds **before** enabling enforcement
- One Conditional Access policy can use compliance status from multiple Intune policies

---

### Common Compliance Scenarios

| Scenario | Compliance Requirements | Actions for Noncompliance | Supporting Profile |
|---|---|---|---|
| **Require BitLocker on all corporate Windows devices** | Require BitLocker: Yes; Assigned to "All Windows Devices" | 1. Mark device noncompliant immediately 2. Send email after 1 day 3. Block access via Conditional Access | Configuration profile: Use Settings catalog to configure BitLocker settings; assign to same group |
| **Enforce Windows 10/11 version currency** | Min OS version: `10.0.19041.0` (Windows 10 20H1+); Assigned to "All Windows Devices" | 1. Mark noncompliant immediately 2. Send email after 3 days 3. Block access after 14 days | Feature update profile: Deploy Windows 10/11 feature updates automatically; assign to same group |
| **Require stronger security for finance team** | All baseline requirements + Require code integrity: Yes; Require Secure Boot: Yes; Device threat level: Secured; Min password length: 12; Assigned to "Finance Team Devices" | Same as baseline policy | Inherits baseline configuration profiles |

---

### Compliance Policy Best Practices

- **Start with basic requirements** — Begin with fundamental security requirements (antivirus, encryption, firewall)
- **Use grace periods** — Give users time to fix compliance issues before blocking access
- **Send notifications** — Always notify users when devices become noncompliant and explain how to fix issues
- **Test with pilot groups** — Assign policies to test groups first to avoid unexpected lockouts
- **Monitor compliance regularly** — Review compliance dashboards weekly to identify trends
- **Document compliance requirements** — Maintain clear documentation of what's required and why
- **Align with security policies** — Ensure compliance requirements match your organization's security policies
- **Use remediation scripts** — Automate fixes for common compliance issues (covered in Unit 6)
- **Layer policies** — Use multiple policies for different scenarios (basic for all devices, stricter for finance team)
- **Integrate with Conditional Access** — Use compliance as a condition for accessing corporate resources

---

### Contoso's Compliance Policies

| Policy | Platform | Key Requirements | Actions | Assignment |
|---|---|---|---|---|
| Corporate Security Baseline | Windows | BitLocker, OS ≥19041, firewall, antivirus, 8-char password | Email (1 day), notification (3 days), block access | All Windows Devices |
| Finance Enhanced | Windows | Baseline + code integrity, 12-char password, 10-min timeout | Same as baseline | Finance Team Devices |
| Corporate Baseline | iOS/iPadOS | Block jailbreak, OS ≥15.0, alphanumeric 8 chars, 5-min timeout | Email (1 day), block access (3 days) | All iOS Devices |
| Corporate Baseline | Android | Block root, OS ≥10, patch ≥2024-01, numeric 6 chars, encryption | Email (1 day), block access (3 days) | All Android Devices |

---

## 4. Target Policies with Dynamic Groups

Assigning policies to static groups works well for many scenarios, but what if you need to target devices dynamically based on their properties? **Dynamic groups** and **assignment filters** provide powerful ways to automatically target policies based on device attributes.

---

### What Are Dynamic Groups?

**Dynamic groups** are Microsoft Entra ID groups where membership is automatically determined by rules based on device or user properties. Instead of manually adding members, you define a rule, and Azure evaluates devices or users against that rule.

**Benefits:**
- **Automation** — Devices are automatically added or removed based on properties
- **Accuracy** — Group membership always reflects current device state
- **Scalability** — No manual maintenance required as your environment grows
- **Consistency** — Same rules apply uniformly across all devices

**Common use cases:**
- Group all Windows 11 devices
- Group devices in a specific department
- Group devices by manufacturer (Dell, HP, Lenovo)
- Group devices enrolled within the last 30 days
- Group devices by ownership (corporate vs. personal)

**Creating dynamic groups** — In the Microsoft Entra admin center, select **Dynamic Device** as the membership type and define a membership rule using:
- **Rule builder (GUI)** — For simple rules using dropdown selectors
- **Rule syntax (advanced)** — For complex conditions with multiple criteria

> **⚠️ Important:** Always use **Validate Rules (Preview)** before creating a group to confirm which devices currently match. This prevents deploying policies to thousands of unintended devices especially critical for sensitive policies like device wipe or BitLocker encryption.

> **Timing:** Dynamic group membership evaluates every 24 hours or when device properties change. New dynamic groups can take up to **32 hours** for full policy deployment (24 hours for membership evaluation + 8 hours for Intune check-in). Plan accordingly for time-sensitive policy changes.

---

### Common Dynamic Group Rules for Devices

| Scenario | Dynamic Group Rule |
|---|---|
| All Windows devices | `(device.deviceOSType -eq "Windows")` |
| All Windows 11 devices | `(device.deviceOSVersion -startsWith "10.0.22")` |
| Devices by manufacturer (Dell) | `(device.deviceManufacturer -eq "Dell Inc.")` |
| Corporate-owned devices | `(device.deviceOwnership -eq "Company")` |
| Devices with names containing "SALES" | `(device.displayName -contains "SALES")` |
| Devices enrolled within the last 30 days | `(device.enrollmentProfileName -ne null) -and (device.approximateLastSignInDateTime -ge "2025-10-11")` |
| Devices in a specific Entra ID organizational unit | `(device.devicePhysicalIds -any (_ -contains "[OrderID]:ContosoSales"))` |
| Complex rule (Windows 11 AND Corporate-owned) | `(device.deviceOSVersion -startsWith "10.0.22") -and (device.deviceOwnership -eq "Company")` |

---

### What Are Assignment Filters?

**Assignment filters** provide additional targeting control at the policy assignment level within Intune separate from group membership. They evaluate against Intune-specific device properties not available in Microsoft Entra dynamic groups (e.g., `device.deviceTotalStorage`, `device.model`, `device.manufacturer`).

**Creating assignment filters** — In the Intune admin center: **Devices** > **Filters** > **Create**. Provide a name, description, choose the platform, and define rules using Property/Operator/Value syntax. Intune shows a preview of matching devices before you create the filter.

---

### Dynamic Groups vs. Assignment Filters

| Feature | Dynamic Groups | Assignment Filters |
|---|---|---|
| **Scope** | Microsoft Entra ID (organization-wide) | Intune policy assignment (policy-specific) |
| **Reusability** | Group can be used across services (Entra, Intune, SharePoint, etc.) | Filter is Intune-specific |
| **Properties** | Limited to Entra device properties | Access to more Intune device properties |
| **Performance** | Membership updates every 24 hours | Evaluated in real-time at policy application |
| **Use case** | Broad categorization (all Windows devices) | Granular targeting (devices with 256 GB+ storage) |

**When to use dynamic groups:**
- Broad device categorization (all Windows, all iOS, all corporate-owned)
- Groups used across multiple services (not just Intune)
- Department-based grouping

**When to use assignment filters:**
- Granular targeting based on Intune-specific properties
- Policy-specific exceptions or refinements
- Targeting based on device inventory (storage, model, BIOS version)

**Include vs. Exclude filter modes:**
- **Include mode** — Policy only applies to devices matching the filter (allow list approach generally clearer for auditing)
- **Exclude mode** — Policy applies to everyone except devices matching the filter (block list approach)

> **💡 Strategic pattern:** Create broad dynamic groups ("All Windows Devices"), then use filters to refine policy targeting ("Include only devices with 256 GB+ storage"). This is easier to maintain than creating many narrowly scoped dynamic groups with near-identical rules.

---

### Common Assignment Filter Properties

| Category | Property | Description | Example Value |
|---|---|---|---|
| **Device hardware** | `device.manufacturer` | Device manufacturer | `"Dell Inc."`, `"HP"` |
| | `device.model` | Device model | `"Latitude 5520"`, `"Surface Pro 9"` |
| | `device.deviceTotalStorage` | Total storage in bytes | `256000000000` (256 GB) |
| | `device.deviceTotalMemory` | Total RAM in bytes | `16000000000` (16 GB) |
| **Device software** | `device.osVersion` | Full OS version | `"10.0.22621.1"` |
| | `device.operatingSystemEdition` | OS edition | `"Professional"`, `"Enterprise"`, `"Education"` |
| **Device enrollment** | `device.enrollmentProfileName` | Autopilot profile name | `"Standard Corporate Profile"` |
| | `device.deviceOwnership` | Device ownership type | `"Corporate"`, `"Personal"` |
| **Device identity** | `device.deviceName` | Device name | `"CONTOSO-LAP-001"` |
| | `device.serialNumber` | Device serial number | `"ABC123XYZ456"` |

---

### Combine Dynamic Groups and Filters

For the most flexible targeting, combine dynamic groups and assignment filters.

**Example: Deploy advanced security settings to high-end finance team devices**

- **Dynamic group:** `Finance Team Windows Devices`
  ```
  (user.department -eq "Finance") -and (device.deviceOSType -eq "Windows")
  ```

- **Assignment filter:** `High-End Devices`
  ```
  (device.deviceTotalStorage -ge 512000000000) -and (device.deviceTotalMemory -ge 16000000000)
  ```

- **Policy assignment:**
  - Profile: `Windows - Advanced Security Settings`
  - Assigned to: `Finance Team Windows Devices` (dynamic group)
  - Filter: **Include filtered devices** → `High-End Devices`

**Result:** Only high-end Windows devices (512 GB+ storage, 16 GB+ RAM) in the Finance department receive the advanced security profile.

---

### Best Practices for Dynamic Groups and Filters

- **Test rules before deployment** — Use validation and preview features to confirm rules target the intended devices
- **Keep rules simple** — Complex rules are harder to troubleshoot and maintain
- **Document rule logic** — Add clear descriptions explaining what the group or filter targets and why
- **Monitor group membership** — Regularly review dynamic group membership to ensure rules work as expected
- **Use descriptive names** — e.g., `Windows 11 Corporate Devices` vs. `Group 1`
- **Combine strategically** — Use dynamic groups for broad categories, then refine with filters for granular targeting
- **Consider performance** — Dynamic groups update every 24 hours; filters are evaluated in real-time
- **Avoid overlapping filters** — Ensure filters don't conflict with group-based targeting
- **Update rules as needed** — As your environment changes, update rules to reflect new requirements
- **Leverage Intune properties** — Use Intune-specific properties in filters that aren't available in dynamic group rules

---

### Contoso's Dynamic Groups and Filters

| Type | Name | Rule | Use |
|---|---|---|---|
| Dynamic Group | All Windows 11 Devices | `device.deviceOSVersion -startsWith "10.0.22"` | Windows 11 configuration profiles |
| Dynamic Group | Sales Team Devices | `user.department -eq "Sales" -and device.deviceOSType -eq "Windows"` | VPN configuration |
| Dynamic Group | Corporate-Owned Windows | `device.deviceOwnership -eq "Company" -and device.deviceOSType -eq "Windows"` | BitLocker compliance policies |
| Filter | High-Storage (256 GB+) | `device.deviceTotalStorage -ge 256000000000` | Refine BitLocker targeting |
| Filter | Dell Devices | `device.manufacturer -eq "Dell Inc."` | Dell BIOS configuration |

---

## 5. Exercise: Assign and Validate Device Policies

In this exercise, you create a configuration profile and compliance policy, assign them to a device group, and validate that the policies are applied correctly. This exercise focuses on Windows devices.

---

### Prerequisites

Before starting, ensure you have:
- A Windows 10/11 device enrolled in Microsoft Intune
- Administrative access to the Microsoft Intune admin center
- The device is powered on and connected to the internet

> **Note:** For iOS/iPadOS and Android devices, the policy creation and assignment process follows similar steps using platform-specific profile templates and compliance settings. Key differences include: passcode requirements for iOS/iPadOS, work profile settings for Android, and platform-specific security requirements like jailbreak detection (iOS) or root detection (Android).

---

### Task 1: Create a Configuration Profile

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com)
2. Navigate to **Devices** > **Configuration**
3. Select **Create** > **New policy**
4. Configure the policy:
   - **Platform**: Windows 10 and later
   - **Profile type**: Settings catalog
   - Select **Create**
5. On the **Basics** page:
   - **Name**: `Exercise - Screen Lock Settings`
   - **Description**: `Configures screen lock timeout for Windows devices`
   - Select **Next**
6. On the **Configuration settings** page:
   - Select **Add settings**
   - In the search box, type `screen timeout`
   - Expand **Control Panel** > **Personalization**
   - Select the checkbox for **Screen Timeout While Locked**
   - Select **Add**
7. Configure the setting value:
   - **Screen Timeout While Locked**: `900` (15 minutes in seconds)
   - Select **Next**
8. On the **Assignments** page:
   - Select **Add groups**
   - Search for and select a group containing your enrolled device (or create a test group)
   - Select **Next**
9. Skip **Applicability Rules** by selecting **Next**
10. On the **Review + create** page:
    - Review the configuration
    - Select **Create**

> **Note:** For iOS/iPadOS, select Device restrictions profile and configure passcode settings. For Android, select Device restrictions and configure work profile password settings.

---

### Task 2: Create a Compliance Policy

1. In the Intune admin center, navigate to **Devices** > **Compliance**
2. Select **Create policy**
3. Choose **Platform**: Windows 10 and later, then select **Create**
4. On the **Basics** page:
   - **Name**: `Exercise - Basic Security Requirements`
   - **Description**: `Requires firewall and antivirus to be enabled`
   - Select **Next**
5. On the **Compliance settings** page, expand **System Security** and configure:
   - **Firewall**: Require
   - **Antivirus**: Require
   - **Microsoft Defender Antimalware**: Require
   - Select **Next**
6. On the **Actions for noncompliance** page:
   - The default action "Mark device noncompliant" (Immediately) is already configured
   - Select **Add** to add an additional action:
     - **Action**: Send email to end user
     - **Schedule**: After 1 day
     - Select **OK**
   - Select **Next**
7. On the **Assignments** page:
   - Select **Add groups**
   - Select the same group you used for the configuration profile
   - Select **Next**
8. On the **Review + create** page:
   - Review the settings
   - Select **Create**

> **Note:** For iOS/iPadOS, configure jailbreak detection and passcode requirements. For Android, configure root detection, work profile password, and encryption requirements.

---

### Task 3: Trigger a Device Sync

To speed up policy application, manually trigger a sync on your enrolled Windows device.

1. On your enrolled Windows device, open **Settings**
2. Navigate to **Accounts** > **Access work or school**
3. Select your work or school account
4. Select **Info**
5. Scroll down and select **Sync**
6. Wait for "Sync is in progress..." to appear
7. Wait 2–3 minutes for the sync to complete

> **Note:** For iOS/iPadOS: Open Company Portal app > Devices > Select device > Check Status. For Android: Open Company Portal or Intune app > Devices > Select device > Check device settings or Sync.

> **💡 Tip:** You can also trigger a sync from the Intune admin center:
> 1. Navigate to **Devices** > **All devices**
> 2. Select your device
> 3. Select **Sync** from the toolbar
> 4. Select **Yes** to confirm

---

### Task 4: Validate Configuration Profile Application

#### On the Device

1. Wait 5–10 minutes after syncing for the policy to apply
2. Verify the screen timeout setting was applied:
   - The setting is enforced in the background
   - Lock the device (Windows + L) and observe the screen timeout behavior

#### In the Intune Admin Center

1. Navigate to **Devices** > **Configuration**
2. Select your configuration profile (`Exercise - Screen Lock Settings`)
3. Review the **Overview** page:
   - Check **Device status** to see how many devices have the profile applied
   - Look for your device showing as **Succeeded**
4. Select **Device status** to see the detailed list
5. Locate your device and verify:
   - **Status**: Success (green checkmark)
   - **Last check-in**: Recent timestamp
6. Select your device to view **Per-setting status** showing successful configuration of all settings

> **Note:** If the policy shows "Pending" or hasn't applied yet, wait a few more minutes and refresh the page. Policy application can take **5–15 minutes** after a sync.

---

### Task 5: Validate Compliance Policy Evaluation

1. Navigate to **Devices** > **All devices**
2. Select your enrolled device
3. Scroll down to the **Device compliance** section
4. Review the compliance status:
   - ✅ **Compliant** — Device meets all requirements
   - ❌ **Not compliant** — Device fails one or more requirements
   - ⬤ **Not evaluated** — Compliance hasn't been evaluated yet
5. Select the **Exercise - Basic Security Requirements** policy
6. View **Per-setting status** to see compliance evaluation for each configured setting

#### On the Device

7. Open **Settings** > **Accounts** > **Access work or school**
8. Select your work account and choose **Info**
9. Scroll to **Device compliance** to see the compliance status

> **Note:** For iOS/iPadOS and Android, open the Company Portal app > Devices > Select device to review compliance status.

---

### Task 6: Review Policy Monitoring Dashboards

**Configuration profile monitoring:**

1. Navigate to **Devices** > **Configuration**
2. Review the overview dashboard:
   - **Policy assignment status**: Pie chart showing succeeded, failed, pending, and conflict statuses
   - **Assignment failures**: List of profiles with errors
3. Select your `Exercise - Screen Lock Settings` profile
4. Review the **Overview** page:
   - **Device status**: Succeeded, failed, or pending count
   - **User status**: User-level assignment status (if applicable)
   - **Per-setting status**: Status for individual settings within the profile

**Compliance policy monitoring:**

1. Navigate to **Devices** > **Compliance**
2. Review the **Overview** dashboard:
   - **Device compliance status**: Pie chart showing compliant, not compliant, in grace period, and not evaluated
   - **Noncompliant devices**: List of devices failing compliance
3. Select your `Exercise - Basic Security Requirements` policy
4. Review the **Overview** page:
   - **Device status**: List of devices with policy assigned and their compliance status
5. If any devices show as noncompliant:
   - Select the device
   - Review **Per-setting status** to see which settings failed
   - Take corrective action (enable firewall, update antivirus, etc.)

---

### Exercise Summary

After completing this exercise, verify the following:

- ✅ Configuration profile created and assigned to device group
- ✅ Compliance policy created and assigned to device group
- ✅ Device synced with Intune (Last check-in is recent)
- ✅ Configuration profile shows "Success" status for your device
- ✅ Compliance policy evaluated your device (Compliant or Not compliant status displayed)
- ✅ Per-setting status visible for both configuration and compliance policies
- ✅ Policy monitoring dashboards reviewed

> **💡 Troubleshooting:** If policies don't apply within 15 minutes, verify device group membership and check **Per-setting status** for errors. For compliance showing "Not evaluated," wait 30–60 minutes as compliance evaluation takes longer than profile application.

---

## 6. Automate Remediation for Noncompliance

When devices become noncompliant, manually fixing issues on each device doesn't scale. **Remediation scripts** (also called Proactive Remediations) allow you to automatically detect and fix common compliance issues using PowerShell scripts.

---

### What Are Remediation Scripts?

Remediation scripts are PowerShell scripts that run on Windows devices to detect and automatically fix issues. Each remediation consists of two scripts:

- **Detection script** — Checks if an issue exists on the device
- **Remediation script** — Fixes the issue if the detection script finds it

**Common use cases:**
- Re-enable Windows Defender Firewall if users disable it
- Clear temporary files when disk space is low
- Fix registry settings that users modified
- Restart stopped services (like Windows Update)
- Remove unauthorized software
- Fix group membership issues

**Benefits:**
- **Automation** — Fix issues without IT intervention
- **Scale** — Deploy to thousands of devices
- **Compliance** — Keep devices compliant automatically
- **Reporting** — Track detection and remediation success rates

---

### How Remediation Scripts Work

1. **Detection script runs first** — Returns **exit 0** (no issue, stop) or **exit 1** (issue found, trigger remediation)
2. **If exit 1**, the **remediation script runs immediately** to fix the issue
3. Results are reported back to Intune

> **Why the two-script design:** This prevents unnecessary changes. Detection runs frequently (potentially thousands of times), but remediation only executes when needed minimizing system impact and reducing risk of unintended modifications.

> **Important:** Remediation scripts are ideal for issues that users can accidentally break (firewall disabled, services stopped) but **NOT** for intentional policy configurations use configuration profiles instead for settings that should always apply.

---

### Example: Ensure Windows Defender Firewall Is Enabled

#### Detection Script

```powershell
# Detection script: Check if Windows Defender Firewall is enabled
# Exit 0 = Compliant (firewall is enabled)
# Exit 1 = Noncompliant (firewall is disabled, needs remediation)

try {
    $firewallProfiles = Get-NetFirewallProfile -Profile Domain,Private,Public
    $disabledProfiles = $firewallProfiles | Where-Object { $_.Enabled -eq $false }
  
    if ($disabledProfiles) {
        # Firewall is disabled on one or more profiles
        Write-Output "Firewall is disabled on: $($disabledProfiles.Name -join ', ')"
        exit 1
    } else {
        # Firewall is enabled on all profiles
        Write-Output "Firewall is enabled on all profiles"
        exit 0
    }
} catch {
    Write-Error "Error checking firewall status: $_"
    exit 1
}
```

> **Why check all three profiles:** Windows has three firewall profiles (Domain, Private, Public). Users can disable one profile while leaving others enabled. Checking all three ensures complete protection.

#### Remediation Script

```powershell
# Remediation script: Enable Windows Defender Firewall
# Exit 0 = Remediation successful
# Exit 1 = Remediation failed

try {
    # Enable firewall on all profiles
    Set-NetFirewallProfile -Profile Domain,Private,Public -Enabled True
  
    # Verify firewall is now enabled
    $firewallProfiles = Get-NetFirewallProfile -Profile Domain,Private,Public
    $disabledProfiles = $firewallProfiles | Where-Object { $_.Enabled -eq $false }
  
    if ($disabledProfiles) {
        Write-Error "Failed to enable firewall on: $($disabledProfiles.Name -join ', ')"
        exit 1
    } else {
        Write-Output "Firewall successfully enabled on all profiles"
        exit 0
    }
} catch {
    Write-Error "Error enabling firewall: $_"
    exit 1
}
```

> **Why verify after remediation:** Always confirm the fix worked before returning exit 0. If `Set-NetFirewallProfile` silently fails (e.g., due to a group policy conflict), the verification catch will return exit 1, triggering an alert in Intune.

---

### Deploy Remediation Scripts to Intune

Navigate to **Devices** > **Scripts** > **Remediations** in the Intune admin center. Upload both the detection and remediation PowerShell files, configure execution settings, assign to device groups, and set a schedule.

**Execution context — Run as:**

| Context | Use For | Why |
|---|---|---|
| **System (SYSTEM account)** | Administrative tasks requiring elevated permissions (firewall, services, registry, disk cleanup) | Most remediation scripts fix system-level issues |
| **User context** | Per-user settings (HKEY_CURRENT_USER registry, user profile files) | Required for user-specific settings |

**Script signature check:**
- **Enforce signature check** (production) — Requires scripts signed with a code signing certificate (security best practice)
- **Don't enforce** (testing) — Allows unsigned scripts for development and testing

**64-bit PowerShell:**
- **Yes (recommended)** — Runs in 64-bit PowerShell (access to full system, all registry keys)
- **No** — Runs in 32-bit PowerShell (limited access, registry redirection issues)

> **Why 64-bit matters:** Many system settings only exist in the 64-bit registry. Running remediation scripts in 32-bit PowerShell means you're checking and fixing a limited view of the system.

**Schedule strategy:**

| Frequency | Use For | Consideration |
|---|---|---|
| **Daily** | Critical security issues (firewall, antivirus, Windows Update service) | Immediate remediation prevents compliance violations |
| **Weekly** | Maintenance tasks (disk cleanup, temp file removal) | Daily checks create unnecessary system load |
| **Custom** | Specialized scenarios (monthly certificate renewal checks, quarterly software updates) | Balance security needs against system performance impact |

---

### Monitor Remediation Script Execution

Navigate to **Devices** > **Scripts** > **Remediations** to monitor execution.

| Status | Meaning | Significance |
|---|---|---|
| **Detection succeeded, no issue** | Detection returned exit 0 (device is compliant, no action taken) | Confirms devices maintain correct configuration without IT intervention |
| **Detection succeeded, issue detected** | Detection returned exit 1, but remediation hasn't run yet | Device may have been offline during remediation window |
| **Remediation succeeded** | Issue was detected and remediation script successfully fixed it (exit 0) | Goal state — automatic self-healing without helpdesk tickets |
| **Remediation failed** | Remediation script returned exit 1 (couldn't fix the issue) | **Critical** — indicates environmental issues (GPO conflict, permissions, hardware failure) requiring investigation |

---

### Additional Remediation Script Examples

#### Example 1: Clear Temp Files When Disk Space Is Low

**Detection:**
```powershell
# Check if C: drive has less than 10 GB free
$drive = Get-PSDrive -Name C
$freeSpaceGB = [math]::Round($drive.Free / 1GB, 2)

if ($freeSpaceGB -lt 10) {
    Write-Output "Low disk space: $freeSpaceGB GB free"
    exit 1
} else {
    Write-Output "Sufficient disk space: $freeSpaceGB GB free"
    exit 0
}
```

**Remediation:**
```powershell
# Clear temp files
Remove-Item -Path "$env:TEMP\*" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item -Path "C:\Windows\Temp\*" -Recurse -Force -ErrorAction SilentlyContinue
Write-Output "Temp files cleared"
exit 0
```

---

#### Example 2: Restart Windows Update Service If Stopped

**Detection:**
```powershell
# Check if Windows Update service is running
$service = Get-Service -Name wuauserv
if ($service.Status -ne "Running") {
    Write-Output "Windows Update service is stopped"
    exit 1
} else {
    Write-Output "Windows Update service is running"
    exit 0
}
```

**Remediation:**
```powershell
# Start Windows Update service
Start-Service -Name wuauserv
$service = Get-Service -Name wuauserv
if ($service.Status -eq "Running") {
    Write-Output "Windows Update service started successfully"
    exit 0
} else {
    Write-Error "Failed to start Windows Update service"
    exit 1
}
```

---

#### Example 3: Fix Incorrect DNS Settings

**Detection:**
```powershell
# Check if DNS is set to corporate DNS servers
$expectedDNS = @("10.0.0.10", "10.0.0.11")
$adapter = Get-NetAdapter | Where-Object {$_.Status -eq "Up"} | Select-Object -First 1
$currentDNS = (Get-DnsClientServerAddress -InterfaceIndex $adapter.ifIndex -AddressFamily IPv4).ServerAddresses

if (($currentDNS | Sort-Object) -ne ($expectedDNS | Sort-Object)) {
    Write-Output "DNS servers incorrect: $($currentDNS -join ', ')"
    exit 1
} else {
    Write-Output "DNS servers correct"
    exit 0
}
```

**Remediation:**
```powershell
# Set DNS to corporate DNS servers
$adapter = Get-NetAdapter | Where-Object {$_.Status -eq "Up"} | Select-Object -First 1
Set-DnsClientServerAddress -InterfaceIndex $adapter.ifIndex -ServerAddresses "10.0.0.10","10.0.0.11"
Write-Output "DNS servers set to 10.0.0.10, 10.0.0.11"
exit 0
```

---

### Integrate Remediation with Compliance Policies

Remediation scripts complement compliance policies to create a **self-healing compliance system**:

1. **Compliance policy detects issue** — Device marked noncompliant (e.g., firewall disabled)
2. **Remediation script fixes issue** — Firewall is automatically re-enabled
3. **Next compliance check** — Device becomes compliant again
4. **Conditional Access allows access** — User regains access to resources

---

### Best Practices for Remediation Scripts

- **Test scripts thoroughly** — Test detection and remediation scripts on pilot devices before wide deployment
- **Use exit codes correctly** — Always return exit 0 for success and exit 1 for failure
- **Log output** — Use `Write-Output` for informational messages and `Write-Error` for errors
- **Handle errors gracefully** — Use try-catch blocks to handle unexpected errors
- **Run as SYSTEM** — Most remediation scripts should run as system for full permissions
- **Verify remediation** — After fixing an issue, verify the fix was successful before exiting
- **Keep scripts simple** — Focus on single issues per script for easier troubleshooting
- **Sign scripts in production** — Use code signing certificates for production deployments
- **Schedule appropriately** — Daily for critical issues, weekly for less urgent maintenance
- **Monitor regularly** — Review remediation reports weekly to identify trends and failures

---

### Contoso's Remediation Scripts

| Script | Purpose | Frequency | Assignment |
|---|---|---|---|
| Windows Defender Firewall | Ensure firewall enabled | Daily | All Windows Devices |
| Disk Space Cleanup | Clear temp files (<10 GB free) | Weekly | All Windows Devices |
| Windows Update Service | Ensure service running | Daily | All Windows Devices |

---

## Confirmed Scenarios with Detailed Solutions

---

### Scenario 1: Deploy BitLocker Encryption via Configuration Profile

**Problem:** You need to ensure all corporate Windows devices have BitLocker encryption enabled with XTS-AES 256-bit encryption.

**Solution:**

1. In the Intune admin center, navigate to **Devices** > **Configuration** > **Create** > **New policy**
2. Platform: **Windows 10 and later** / Profile type: **Settings catalog** > **Create**
3. Name: `Windows - BitLocker Encryption`
4. In **Configuration settings**, select **Add settings** and search for `BitLocker`:
   - **BitLocker**: Require device encryption
   - **BitLocker system drive encryption method**: XTS-AES 256-bit
   - **BitLocker fixed drive encryption method**: XTS-AES 256-bit
   - **Choose how BitLocker-protected fixed drives can be recovered**: Configure recovery key escrow to Microsoft Entra ID
5. On **Assignments**, assign to a **device group** (e.g., "All Windows Devices") — NOT a user group
6. Deploy the companion **compliance policy** to verify BitLocker is actually enabled (separate policy: `Windows - BitLocker Required`)
7. Monitor deployment: **Devices** > **Configuration** > Select profile > **Per-setting status**

**Sources:**
- [BitLocker settings in Intune](https://learn.microsoft.com/en-us/mem/intune/protect/encrypt-devices)
- [Windows Settings catalog](https://learn.microsoft.com/en-us/mem/intune/configuration/settings-catalog)

---

### Scenario 2: Block Corporate Resource Access for Noncompliant Devices

**Problem:** Devices that don't meet compliance requirements (antivirus off, outdated OS, no encryption) can still access Exchange Online and SharePoint.

**Solution:**

1. **Create a compliance policy** in Intune that checks:
   - Require antivirus: Yes
   - Require firewall: Yes
   - Minimum OS version: `10.0.19041.0`
   - Require BitLocker: Yes
   - Actions: Mark noncompliant immediately; email after 1 day
2. **Create a Conditional Access policy** in Microsoft Entra admin center:
   - Users/Groups: All users
   - Cloud apps: All cloud apps (or specific: Exchange Online, SharePoint)
   - Conditions: Check "Require device to be marked as compliant"
   - Grant: Require compliant device
3. **Enable in report-only mode first** to preview impact before enforcing
4. After validating impact, switch to **Enforce** mode

**Sources:**
- [Conditional Access with Intune](https://learn.microsoft.com/en-us/mem/intune/protect/conditional-access)
- [Require compliant device with Conditional Access](https://learn.microsoft.com/en-us/entra/identity/conditional-access/howto-conditional-access-policy-compliant-device)

---

### Scenario 3: Apply Stricter Policies to the Finance Department

**Problem:** The Finance team handles sensitive data and needs stronger security requirements than standard employees (longer passwords, code integrity, Secure Boot).

**Solution:**

1. **Create a dynamic group** for Finance Windows devices:
   ```
   (user.department -eq "Finance") -and (device.deviceOSType -eq "Windows")
   ```
2. **Create a Finance-specific configuration profile:**
   - Name: `Windows - Finance Enhanced Security`
   - Settings: Minimum password length 12, max inactivity 10 minutes, enable code integrity
   - Assign to `Finance Windows Devices` group
3. **Create a Finance-specific compliance policy:**
   - Name: `Windows - Finance Compliance`
   - Requirements: All baseline + Require Secure Boot: Yes; Require code integrity: Yes; Min password length: 12; Device threat level: Secured
   - Assign to `Finance Windows Devices` group
4. Ensure the Finance policy **priority is higher** than the baseline policy in the policy stack

**Sources:**
- [Create device compliance policies](https://learn.microsoft.com/en-us/mem/intune/protect/create-compliance-policy)

---

### Scenario 4: Automatically Re-enable Firewall When Disabled by Users

**Problem:** Users in the Sales team occasionally disable their Windows Defender Firewall, causing compliance failures and helpdesk tickets.

**Solution:**

1. Deploy the **Firewall detection + remediation script** from Unit 6 (fully documented with code above)
2. In the Intune admin center, navigate to **Devices** > **Scripts** > **Remediations** > **Create**
3. Upload the detection and remediation scripts:
   - Run as SYSTEM context
   - Run in 64-bit PowerShell
   - Enforce signature check: No (testing) / Yes (production)
   - Schedule: **Daily**
4. Assign to: `Sales Team Devices` group (or All Windows Devices)
5. **Companion compliance policy** still marks devices noncompliant if the firewall check fails — the remediation fixes it before the next compliance cycle

**Sources:**
- [Use Intune remediation scripts](https://learn.microsoft.com/en-us/mem/intune/fundamentals/remediations)

---

### Scenario 5: Target a VPN Profile Only to Sales Laptops

**Problem:** You want to deploy a VPN profile only to Sales department devices, but only to laptops with 256 GB+ storage (not thin clients or kiosks).

**Solution:**

1. **Create a dynamic group**: `Sales Team Windows Devices`
   ```
   (user.department -eq "Sales") -and (device.deviceOSType -eq "Windows")
   ```
2. **Create an assignment filter**: `High-Storage Devices (256 GB+)`
   ```
   (device.deviceTotalStorage -ge 256000000000)
   ```
3. **Create the VPN configuration profile**:
   - Platform: Windows 10 and later
   - Profile type: Templates > VPN
   - Configure connection: IKEv2, `vpn.contoso.com`
   - Assignments: Add `Sales Team Windows Devices` group
   - Filter: Include filtered devices → `High-Storage Devices (256 GB+)`
4. **Validate** deployment in **Devices** > **Configuration** > Select profile > **Device status**

**Sources:**
- [Use assignment filters in Intune](https://learn.microsoft.com/en-us/mem/intune/fundamentals/filters)
- [Configure VPN settings in Intune](https://learn.microsoft.com/en-us/mem/intune/configuration/vpn-settings-configure)

---

## Related Scenarios and Detailed Solutions

---

### Related Scenario 1: Deploy Wi-Fi Profile with Certificate Authentication

**Challenge:** Corporate Wi-Fi uses WPA2-Enterprise with certificate-based authentication. Devices need the certificate AND Wi-Fi profile deployed automatically.

**Solution:**

1. First, deploy a **Trusted Certificate** profile:
   - Platform: Windows 10 and later
   - Profile type: Templates > Trusted certificate
   - Upload your root CA certificate
   - Assign to "All Windows Devices"
2. Deploy a **SCEP Certificate** profile:
   - Platform: Windows 10 and later
   - Profile type: Templates > SCEP certificate
   - Configure SCEP server URL and certificate properties
   - Assign to "All Windows Devices"
3. Deploy the **Wi-Fi profile**:
   - Platform: Windows 10 and later
   - Profile type: Templates > Wi-Fi
   - Security type: WPA2-Enterprise
   - Authentication method: Certificate
   - Reference the SCEP certificate profile
   - Assign to "All Windows Devices"

> **Important:** Deploy in order — trusted cert first, then SCEP, then Wi-Fi. Dependencies require the certificate chain before the Wi-Fi profile applies.

**Sources:**
- [Configure Wi-Fi settings in Intune](https://learn.microsoft.com/en-us/mem/intune/configuration/wi-fi-settings-configure)
- [SCEP certificate profiles in Intune](https://learn.microsoft.com/en-us/mem/intune/protect/certificates-scep-configure)

---

### Related Scenario 2: Resolve Profile Conflicts Between Two Overlapping Policies

**Challenge:** Two configuration profiles both configure the screen timeout setting with different values — one says 5 minutes, another says 15 minutes. Devices show "Conflict" status.

**Solution:**

1. Navigate to **Devices** > **Configuration**
2. Review **Per-setting status** for both profiles — both will list the conflicting devices
3. Use **Copilot in Intune** prompt: `List settings with conflicts and explain resolutions` for a quick summary
4. **Resolution options:**
   - **Merge profiles** — Combine both into a single profile with one authoritative screen timeout value
   - **Adjust scope** — Use applicability rules or assignment filters to ensure only one profile applies per device
   - **Remove the redundant setting** — Delete the screen timeout setting from one profile
5. After resolving, trigger a manual sync on affected devices and verify **Per-setting status** shows "Succeeded"

**Sources:**
- [Troubleshoot configuration profiles in Intune](https://learn.microsoft.com/en-us/mem/intune/configuration/troubleshoot-policies-in-microsoft-intune)

---

### Related Scenario 3: iOS BYOD — App Protection Without Full Device Compliance

**Challenge:** Employees want to use personal iPhones for work email. You need to protect corporate data without enrolling the device into full MDM.

**Solution:**

1. Configure **App Protection Policies (MAM)** for Microsoft Outlook and Teams:
   - Navigate to **Apps** > **App protection policies** > **Create policy** > **iOS/iPadOS**
   - Require PIN for app access, block copy/paste to non-managed apps, wipe app data on jailbreak detection
2. Configure **Conditional Access** to allow access from unmanaged devices only via approved apps with app protection policies:
   - Grant: Require app protection policy
3. This provides app-level protection **without** requiring Intune enrollment or a device compliance policy

**Sources:**
- [App protection policies overview](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy)
- [Require app protection policy with Conditional Access](https://learn.microsoft.com/en-us/entra/identity/conditional-access/howto-policy-approved-app-or-app-protection)

---

### Related Scenario 4: Windows Feature Update Policy for OS Version Compliance

**Challenge:** Compliance policy requires minimum OS version `10.0.19041.0`, but older devices haven't updated and are marked noncompliant.

**Solution:**

1. Deploy a **Feature Update policy** in Intune:
   - Navigate to **Devices** > **Windows updates** > **Feature updates** > **Create profile**
   - Select the target Windows 10/11 version (e.g., Windows 11 23H2)
   - Assign to "All Windows Devices" or a subset
2. Set a **deadline for mandatory installation** (e.g., 7 days)
3. Monitor update rollout via **Devices** > **Windows updates** > **Windows feature updates** > Select policy
4. As devices update and check in, compliance status transitions from "Not compliant" to "Compliant"

**Sources:**
- [Windows feature update management in Intune](https://learn.microsoft.com/en-us/mem/intune/protect/windows-10-feature-updates)

---

## Tools and Resources Used

| Tool / Resource | Description | Link |
|---|---|---|
| **Microsoft Intune admin center** | Primary portal for managing policies, profiles, compliance, and remediation scripts | [intune.microsoft.com](https://intune.microsoft.com) |
| **Microsoft Entra admin center** | Identity management and Conditional Access policies | [entra.microsoft.com](https://entra.microsoft.com) |
| **Settings catalog** | Modern Windows policy configuration with searchable settings | Intune admin center > Devices > Configuration |
| **Copilot in Intune** | AI assistant for summarizing policy status and diagnosing conflicts | Available in Intune admin center profile/policy views |
| **Windows Settings catalog** | Full reference for all configurable Windows settings | [Settings catalog docs](https://learn.microsoft.com/en-us/mem/intune/configuration/settings-catalog) |
| **Intune Remediation Scripts** (Proactive Remediations) | PowerShell-based automated detection and fix scripts | Intune admin center > Devices > Scripts > Remediations |
| **`Get-NetFirewallProfile`** | PowerShell cmdlet to check Windows Defender Firewall status across all profiles | Part of Windows built-in PowerShell modules |
| **`Set-NetFirewallProfile`** | PowerShell cmdlet to enable/configure Windows Defender Firewall profiles | Part of Windows built-in PowerShell modules |
| **`Get-PSDrive`** | PowerShell cmdlet to check disk space | Part of Windows built-in PowerShell modules |
| **`Get-Service` / `Start-Service`** | PowerShell cmdlets to check and manage Windows services | Part of Windows built-in PowerShell modules |
| **`Get-DnsClientServerAddress` / `Set-DnsClientServerAddress`** | PowerShell cmdlets to check and configure DNS settings | Part of Windows built-in PowerShell modules |
| **Conditional Access** | Microsoft Entra ID policy engine that enforces compliance-based access control | [CA docs](https://learn.microsoft.com/en-us/entra/identity/conditional-access/) |
| **Dynamic groups** | Microsoft Entra ID groups with auto-membership based on device/user properties | [Dynamic groups docs](https://learn.microsoft.com/en-us/entra/identity/users/groups-create-rule) |
| **Assignment filters** | Intune-specific targeting mechanism based on Intune device properties | [Filters docs](https://learn.microsoft.com/en-us/mem/intune/fundamentals/filters) |
| **Device restrictions — iOS/iPadOS** | Platform-specific configuration settings for iOS/iPadOS | [iOS device restrictions](https://learn.microsoft.com/en-us/mem/intune/configuration/device-restrictions-ios) |
| **Device restrictions — Android Enterprise** | Platform-specific configuration settings for Android Enterprise | [Android Enterprise device restrictions](https://learn.microsoft.com/en-us/mem/intune/configuration/device-restrictions-android-enterprise) |
| **BitLocker encryption with Intune** | Full guide for deploying BitLocker via Intune | [Encrypt devices](https://learn.microsoft.com/en-us/mem/intune/protect/encrypt-devices) |
| **App protection policies (MAM)** | App-level data protection without full device enrollment | [App protection policies](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy) |
| **Windows feature updates** | OS update management via Intune | [Feature updates](https://learn.microsoft.com/en-us/mem/intune/protect/windows-10-feature-updates) |
| **VPN settings in Intune** | Configure VPN connection profiles across platforms | [VPN settings](https://learn.microsoft.com/en-us/mem/intune/configuration/vpn-settings-configure) |
| **Wi-Fi settings in Intune** | Configure Wi-Fi connection profiles across platforms | [Wi-Fi settings](https://learn.microsoft.com/en-us/mem/intune/configuration/wi-fi-settings-configure) |
| **Troubleshoot policies in Intune** | Diagnostic guidance for configuration profile and compliance issues | [Troubleshoot policies](https://learn.microsoft.com/en-us/mem/intune/configuration/troubleshoot-policies-in-microsoft-intune) |
| **SCEP certificate profiles** | Deploy client authentication certificates via SCEP | [SCEP certificates](https://learn.microsoft.com/en-us/mem/intune/protect/certificates-scep-configure) |

---
