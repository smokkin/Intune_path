# Enroll and Validate Devices in Microsoft Intune

---

## 1. Introduction

You've prepared your Microsoft Intune tenant, configured roles and licensing, and learned about different enrollment methods. Now it's time to put that knowledge into practice by enrolling actual devices and validating that they're correctly managed.

Device enrollment is the moment when an endpoint transitions from **unmanaged to managed**, giving your organization the ability to deploy policies, secure data, and monitor compliance. But enrollment is just the beginning you also need to verify that devices successfully joined Microsoft Entra ID, confirm management connectivity, and troubleshoot any issues that arise.

---

### Scenario: Contoso's Enrollment Pilot

> **Context:** You're the IT administrator at Contoso Corporation. You've prepared the tenant and configured enrollment settings. Leadership has approved a pilot program to enroll **50 Windows devices** from the Finance department.

**Your tasks for this module include:**
- Enrolling your first Windows device to understand the user experience
- Validating that the device successfully joined Microsoft Entra ID and enrolled in Intune
- Configuring enrollment restrictions to control which devices Finance users can enroll
- Troubleshooting enrollment issues that pilot users might encounter

---

### What You Learn

In this hands-on module, you:

- **Enroll a Windows device** using the manual enrollment method
- **Validate device status** by checking join type, management connectivity, and policy application
- **Configure enrollment restrictions** to control platform types and device ownership
- **Troubleshoot common issues** such as failed enrollment, sync problems, and certificate errors

---

### Main Goal

By the end of this module, you have practical experience enrolling devices in Microsoft Intune and the skills to validate successful enrollment and resolve common issues. You're confident performing enrollments at scale and supporting users through the enrollment process.

---

## 2. Exercise: Enroll a Windows Device

In this exercise, you enroll a Windows device in Microsoft Intune using Microsoft Entra join with automatic MDM enrollment. This is the recommended method for corporate-owned devices.

> **Note:** This exercise focuses on Windows device enrollment. For iOS/iPadOS and Android enrollment procedures, see:
> - [Enroll iOS/iPadOS devices in Intune](https://learn.microsoft.com/en-us/mem/intune/enrollment/ios-enroll)
> - [Enroll Android devices in Intune](https://learn.microsoft.com/en-us/mem/intune/enrollment/android-enroll)

> **⚠️ Important:** This exercise requires a Windows 10 or Windows 11 device that is **not already joined** to Microsoft Entra ID or enrolled in Intune. You also need a Microsoft Entra user account with an Intune license assigned.

---

### Prerequisites

Before you begin, ensure you have:

- A Windows 10 (version 1809 or later) or Windows 11 device
- Local administrator rights on the device
- The device connected to the internet
- A Microsoft Entra user account with an Intune license
- The user account email address and password

> **Note:** If you don't have a physical device available, you can use a virtual machine running Windows 10 or 11 in Azure, Hyper-V, or another hypervisor.

---

### Task 1: Verify Automatic Enrollment Is Configured

Before enrolling devices, verify that automatic MDM enrollment is enabled in Microsoft Entra ID.

1. Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com) with your administrative credentials.
2. Navigate to **Devices** > **Overview** > **Device settings**.
3. Scroll down to **Enterprise applications** and select **Microsoft Intune**.
4. On the **Mobility (MDM and MAM)** page, verify:
   - **MDM user scope** is set to **All** or **Some** (with your test user included)
   - **MDM terms of use URL**, **MDM discovery URL**, and **MDM compliance URL** are all populated
5. If automatic enrollment isn't enabled:
   - Set **MDM user scope** to **All** (for testing) or **Some** (select specific groups)
   - Ensure the URLs are populated (they auto-populate when you select Intune)
   - Select **Save**
     <img width="1192" height="870" alt="image" src="https://github.com/user-attachments/assets/4a352e68-437c-41d3-acc2-9e679bbc1547" />


> **💡 Tip:** In production environments, start with **Some** and assign specific pilot groups before expanding to **All** users.

---

### Task 2: Prepare the Windows Device

Ensure the device is not already joined to Microsoft Entra ID.

1. On the Windows device, press **Windows key + I** to open **Settings**.
2. Navigate to **Accounts** > **Access work or school**.
3. Check if any work or school accounts are already connected:
   - If you see **"Connected to [organization name]'s Microsoft Entra ID"** — the device is already joined. Use a different device or remove it first.
   - If you see **"Connected to [organization name]"** — disconnect it by selecting the connection and choosing **Disconnect**.
4. Verify the device isn't already domain-joined:
   - Press **Windows key + Pause** or go to **Settings** > **System** > **About**
   - Under **Device specifications**, confirm **Domain** shows "Not available" or the device is in a WORKGROUP
   - If the device is domain-joined, use a different device or unenroll from the existing domain first.

---

### Task 3: Enroll the Windows Device

Perform the enrollment by joining the device to Microsoft Entra ID, which automatically triggers Intune enrollment.

1. In **Settings** > **Accounts** > **Access work or school**, select **Connect**.
2. On the **Sign in** page, enter your Microsoft Entra user account email (e.g., `user@contoso.com`) and select **Next**.
3. Enter the password and select **Sign in**.
4. On the **Make sure this is your organization** page:
   - Verify the organization name and domain match your Intune tenant
   - Select **Join**
5. On the **You're all set!** page:
   - Review the confirmation message
   - Select **Done**
6. The device begins enrolling in Microsoft Intune automatically. You may see a brief notification: **"Setting up...your device for management."**

> **Note:** The enrollment process typically completes in **1–2 minutes**. Policies and apps begin deploying shortly after enrollment completes.

---

### Task 4: Verify Local Enrollment Status

After enrollment, verify the device successfully joined Microsoft Entra ID and enrolled in Intune.

1. Stay in **Settings** > **Accounts** > **Access work or school**.
2. You should now see:
   - Connection name: **Connected to [your organization]'s Entra ID**
   - Organization: Your tenant name
   - Account: Your user email address
3. Select the work or school account connection to expand it.
4. Select **Info** to view enrollment details:
   - **Managed by**: Should show your organization name
   - **Sync status**: Should show "Sync in progress" or "Last sync" with a recent timestamp
   - **Areas managed by [organization]**: Should show "Connectivity," "Configuration," and other areas
5. To trigger an immediate sync (to pull the latest policies), select **Sync** on the Info page.

> **💡 Tip:** Users can return to this page at any time to manually sync their device or view compliance status.

---

### Task 5: Verify Enrollment in the Intune Admin Center

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com) with administrative credentials.
2. Navigate to **Devices** > **All devices**.
3. Locate the device you just enrolled (use the search box if needed).
4. Select the device name to open its properties page.
5. On the device overview page, verify:

   | Field | Expected Value |
   |---|---|
   | **Managed by** | MDM (Mobile Device Management) |
   | **Ownership** | Corporate or Personal (depending on enrollment method) |
   | **Compliance status** | "Not evaluated" initially → transitions to Compliant or Not compliant |
   | **Last check-in** | Recent enrollment time |
   | **Microsoft Entra registered** | Yes |
   | **Intune registered** | Yes |
   | **OS** | Your device's operating system |
   | **Primary user** | Enrolled user's name |

6. Navigate to **Hardware** (under **Monitor**) to view device specifications:
   - Total storage, free storage, manufacturer, model, OS version
   - Confirms that Intune is collecting device inventory
7. Navigate to **Discovered apps** (under **Monitor**):
   - May not populate immediately — Intune collects app inventory during regular sync intervals
8. Navigate to **Device configuration** (under **Monitor**) to see configuration profile assignments:
   - Initially, no profiles may be assigned
   - Profiles appear as you create and deploy configuration and compliance policies

> **⚠️ Important:** It can take up to **8 hours** for a device to fully populate in the Intune portal with complete inventory details. However, basic enrollment and management capabilities are available immediately.

---

### Task 6: Check Microsoft Entra Device Status

1. Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com).
2. Navigate to **Devices** > **All devices**.
3. Locate the enrolled device and select its name.
4. Verify the following properties:

   | Field | Expected Value |
   |---|---|
   | **Enabled** | Yes |
   | **OS** | Windows |
   | **Join type** | Microsoft Entra joined |
   | **MDM** | Microsoft Intune |
   | **Owner** | The user who enrolled the device |
   | **Registered** | Recent enrollment date/time |

5. Scroll down to view:
   - **Compliance**: Device compliance status as reported by Intune
   - **BitLocker keys**: May show BitLocker recovery keys for Windows devices (if BitLocker is enabled)

---

### Exercise Troubleshooting Tips

If enrollment didn't complete successfully, see [Unit 5 — Troubleshoot Device Enrollment](#5-troubleshoot-device-enrollment). Common issues include:

- Automatic enrollment not configured in Microsoft Entra ID
- User doesn't have an Intune license assigned
- Network connectivity issues preventing cloud communication
- Device already joined to another organization's Entra ID

---

### Exercise Summary

After completing this exercise, you should see:

- Device listed in **Settings** > **Access work or school** as connected
- Device appearing in **Microsoft Intune admin center** under **All devices**
- Device registered in **Microsoft Entra ID** with join type "Microsoft Entra joined"
- MDM management status showing "Microsoft Intune"
- Device inventory details populating in the Intune portal

---

## 3. Validate Device Join and Management

After enrolling a device, it's critical to validate that enrollment completed successfully and the device is properly managed. Validation ensures that devices can receive policies, users can access corporate resources, and IT can manage the device remotely.

---

### Understanding Device Join Types

Windows devices can connect to Microsoft Entra ID in different ways. The join type determines what level of management is available.

| Join Type | What It Is | When to Use | Management Capabilities |
|---|---|---|---|
| **Microsoft Entra joined** | Device joined directly to Microsoft Entra ID; primary identity is organizational; user signs in with Microsoft Entra credentials; fully managed by Intune | Corporate-owned devices; cloud-first organizations; devices not needing on-premises AD resources | Full device management and configuration; Conditional Access; BitLocker key escrow to Microsoft Entra ID; remote wipe and retire |
| **Microsoft Entra registered** | Personal device with work account added; primary identity is local (Microsoft or local account); work/school account added for resource access; typically MAM only | BYOD scenarios; personal devices accessing email and Office apps; when full device management is not needed | MAM policies; Conditional Access to apps; limited inventory; no device-level policy or full MDM configuration |
| **Hybrid Microsoft Entra joined** | Device joined to on-premises AD and Microsoft Entra ID; primary identity is AD domain account; synced via Microsoft Entra Connect | Orgs with on-premises AD; gradual cloud migration; devices needing both on-premises and cloud access | Co-management (Intune + Configuration Manager); Conditional Access; full MDM and GPO support |

> **⚠️ Important:** This module focuses on **Microsoft Entra joined** devices, which is the recommended join type for cloud-native organizations using Intune.

---

### Validating Device Join Status on the Device

#### Method 1: Use Settings (Simplest for End Users)

1. Open **Settings** > **Accounts** > **Access work or school**
2. Look for the connection:
   - **"Connected to [organization]'s Azure AD"** = Microsoft Entra joined (UI may still show "Azure AD")
   - **"Connected to [organization]"** = Microsoft Entra registered
3. Select the connection and choose **Info** to see:
   - Organization name
   - Sync status
   - Managed by (should show your organization)

#### Method 2: Use System Information (About)

1. Open **Settings** > **System** > **About**
2. Scroll to **Device specifications**
3. Check the **Azure AD joined** field:
   - **"Yes"** = Device is Microsoft Entra joined
   - **"No"** = Device isn't Microsoft Entra joined (might be registered or domain-joined)

#### Method 3: Use `dsregcmd` (Most Detailed — For IT Pros)

The `dsregcmd` command-line tool provides detailed device registration status.

1. Open **Command Prompt** or **PowerShell** as Administrator.
2. Run:

```cmd
dsregcmd /status
```

3. Review the output:

**For Microsoft Entra joined devices:**
```
Device State
-------------
AzureAdJoined : YES
EnterpriseJoined : NO
DomainJoined : NO
```

**Check MDM enrollment:**
```
+----------------------------------------------------------------------+
| Tenant details
+----------------------------------------------------------------------+
MdmUrl : https://enrollment.manage.microsoft.com/enrollmentserver/discovery.svc
TenantId : <your-tenant-id>
MdmComplianceUrl : https://portal.manage.microsoft.com/?portalAction=Compliance
```

**Key fields to check:**

| Field | Expected Value |
|---|---|
| **AzureAdJoined** | YES |
| **MDMUrl** | Points to Microsoft Intune enrollment endpoint |
| **DeviceId** | Unique identifier for the device in Microsoft Entra ID |
| **Thumbprint** | Certificate thumbprint for device authentication |

> **💡 Tip:** Save the `dsregcmd` output to a file for troubleshooting:
> ```cmd
> dsregcmd /status > C:\devicestatus.txt
> ```

---

### Validating Management Connectivity

#### Check Last Sync Time

On the device:

1. Go to **Settings** > **Accounts** > **Access work or school**
2. Select the work or school account
3. Select **Info**
4. Check **Last sync** timestamp:
   - Should be within the last 24 hours (default sync interval is 8 hours)
   - If older than 24 hours, the device may not be communicating with Intune

#### Manually Trigger a Sync

1. In the **Info** page, select **Sync**
2. Wait for the "Sync is in progress" message
3. Confirm **Last sync** updates to the current time

#### Verify Sync in Intune Admin Center

1. Navigate to **Devices** > **All devices**
2. Select the enrolled device
3. Check **Last check-in** timestamp:
   - Should match the device's last sync time (within a few minutes)
   - If significantly out of date, the device isn't reaching Intune

---

### Check Device Compliance Status

#### View Compliance on the Device

1. Go to **Settings** > **Accounts** > **Access work or school**
2. Select the work or school account and choose **Info**
3. Look for **Device compliance**:
   - **Compliant** — Device meets all requirements
   - **Not compliant** — Device fails one or more compliance checks
   - **Not evaluated** — Compliance hasn't been assessed yet (typical immediately after enrollment)

#### View Compliance in Intune Admin Center

1. Navigate to **Devices** > **All devices**
2. Locate the device and check the **Compliance** column:
   - ✅ **Compliant** (green checkmark)
   - ❌ **Not compliant** (red X)
   - ⬤ **Not evaluated** (gray circle)
   - ⚠️ **Grace period** (yellow warning)
3. Select the device and go to **Device compliance** (under **Monitor**) to see:
   - Which compliance policies are assigned
   - Which policy settings passed or failed
   - Actions taken for non-compliance

> **Note:** A newly enrolled device may show "Not evaluated" until compliance policies are assigned and evaluated. Compliance policies define device requirements (password strength, encryption, OS version) and are configured separately.

---

### Verify Policy Application

#### In Intune Admin Center

1. Navigate to **Devices** > **All devices** and select the device
2. Go to **Device configuration** (under **Monitor**)
3. View assigned policies:

   | Status | Meaning |
   |---|---|
   | ✅ **Success** | Policy applied successfully |
   | ⚠️ **Conflict** | Multiple policies with conflicting settings |
   | ❌ **Error** | Policy failed to apply |
   | 🕐 **Pending** | Policy not yet applied |

4. Select a specific policy to see:
   - Settings status (which settings succeeded or failed)
   - Error messages for failed settings
   - Last update time
     <img width="1250" height="616" alt="image" src="https://github.com/user-attachments/assets/27766ce6-8033-40d4-8437-51b565734a98" />

#### On the Device — Event Viewer

Device configuration events are logged in Windows Event Viewer:

1. Press **Windows key + R** and run `eventvwr.msc`
2. Navigate to:
   ```
   Applications and Services Logs > Microsoft > Windows > DeviceManagement-Enterprise-Diagnostics-Provider
   ```
3. Review **Admin** and **Operational** logs for:
   - Policy application events
   - Sync events
   - Error messages

> **💡 Tip:** Filter the Event Viewer by:
> - **Event ID 400** — Successful policy application
> - **Event ID 401** — Policy failures

---

### Check Device Inventory

#### View Inventory in Intune Admin Center

1. Navigate to **Devices** > **All devices** and select the device
2. Go to **Hardware** (under **Monitor**):
   - Serial number, manufacturer, model
   - Total storage and free storage
   - Battery health (for laptops)
   - IMEI and phone number (for mobile devices)
3. Go to **Discovered apps** (under **Monitor**):
   - List of applications installed on the device
   - Version numbers and publisher information

> **Note:** Discovered apps may take up to **7 days** to populate after initial enrollment, as Intune collects app inventory on a weekly schedule.

---

### Common Validation Issues

| Symptom | Cause | Resolution |
|---|---|---|
| Device shows "Microsoft Entra registered" instead of "Microsoft Entra joined" | Device was enrolled with a local account instead of joining during enrollment | Re-enroll using the "Join this device to Microsoft Entra ID" option |
| MDM URLs missing in `dsregcmd` output | Automatic enrollment isn't configured in Microsoft Entra ID | Navigate to **Microsoft Entra admin center** > **Mobility (MDM and MAM)** > **Microsoft Intune** > **Configure** |
| Device not appearing in Intune after enrollment | Propagation delay, missing license, or automatic enrollment disabled | Wait 15–30 minutes, refresh the portal, verify the user's Intune license, and check automatic enrollment settings |
| Last check-in is several days old | Device may be offline or unable to reach Intune endpoints | Check network connectivity and firewall rules; manually trigger a sync on the device |

---

### Validation Summary at Contoso

| Validation Area | What Contoso Confirmed |
|---|---|
| **Device Join** | All enrolled devices are Microsoft Entra joined (not just registered) |
| **Policy Sync** | Devices communicate with Intune and sync policies regularly |
| **Help Desk Procedures** | Clear procedures established to verify device status and troubleshoot issues |
| **User Self-Service** | Users can verify enrollment status in Settings without IT support |

---

## 4. Apply Platform and Ownership Restrictions

Not all devices should have access to corporate resources. Enrollment restrictions allow you to control which device platforms, operating system versions, manufacturers, and ownership types can be enrolled in Intune.

These restrictions help you:
- Enforce corporate device standards
- Prevent unsupported devices from accessing resources
- Separate corporate-owned from personal devices
- Comply with security and regulatory requirements

---

### Types of Enrollment Restrictions

#### Device Type Restrictions

Control which platforms and OS versions can enroll:
- Windows
- Android
- iOS/iPadOS
- macOS

For each platform, you can:
- Allow or block enrollment
- Specify minimum and maximum OS versions
- Block personally owned devices (for some platforms)

#### Device Limit Restrictions

Control how many devices each user can enroll:
- Set a maximum number of devices per user (default is **15**)
- Prevent users from enrolling more devices after reaching the limit
- Force users to retire old devices before enrolling new ones

---

### Restriction Policies and Priority

#### Default Policy

- Every Intune tenant has a default device type restriction and a default device limit restriction
- Default policies apply to all users unless overridden by a targeted policy
- Default policies have the **lowest priority** and **cannot be deleted** (but can be edited)

#### Targeted Policies

- You can create additional restriction policies and assign them to specific Microsoft Entra groups
- Targeted policies override the default policy for users in those groups
- If a user is in multiple groups with different restrictions, the **policy with the highest priority** applies

#### Priority Order

- Policies are evaluated in priority order (**1 is highest priority**)
- When a user enrolls a device, Intune checks all restriction policies assigned to that user
- The policy with the highest priority (lowest number) that applies to the user is enforced

---

### Configure Device Type Restrictions

#### Create a Device Type Restriction Policy

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com)
2. Navigate to **Devices** > **Enrollment**
3. Select **Enrollment device platform restrictions** (under **Device platform restrictions** section)
4. Select **Create restriction** > **Device type restriction**
5. Provide a name and optional description for the policy

#### Configure Windows Platform Restrictions

**Example configuration for corporate Windows devices only:**

1. Set **Allow Windows enrollment** to **Yes**
2. Set **Minimum OS version** to `10.0.19041.0` (Windows 10 20H1)
3. Set **Block personally owned devices** to **Yes**
4. Leave **Maximum OS version** blank to allow the latest Windows versions

> **💡 Tip:** For Windows, personal vs. corporate ownership is determined by Autopilot registration, manual marking in Intune, or Microsoft Entra device settings.

#### Mobile Platform Enrollment Restrictions (Brief Notes)

- **iOS/iPadOS**: Corporate-owned devices are identified through Apple Automated Device Enrollment (ADE)
- **Android**: Work profiles recommended for BYOD scenarios
- For detailed mobile platform restriction configurations, see [Set enrollment restrictions in Microsoft Intune](https://learn.microsoft.com/en-us/mem/intune/enrollment/enrollment-restrictions-set)

#### Assign the Policy to Groups

1. On the **Assignments** page, select **Add groups**
2. Search for and select the Microsoft Entra groups that should receive the restriction
3. Select **Select** to assign the policy

> **⚠️ Important:** Enrollment restrictions are assigned to **users**, not devices. When a user attempts to enroll a device, Intune checks the restrictions assigned to that user's groups.

#### Set Policy Priority

1. Navigate to **Devices** > **Enrollment** > **Enrollment device platform restrictions**
2. Select the **Device type restrictions** tab
3. Use the **…** menu next to each policy to **Move up** or **Move down** in priority
4. The policy at the top (**Priority 1**) has the highest priority

> **💡 Tip:** Assign your most restrictive policies to specific groups with the highest priority, and leave the default policy with more lenient settings as a fallback.

---

### Configure Device Limit Restrictions

#### Create a Device Limit Restriction Policy

1. In the Intune admin center, navigate to **Devices** > **Enrollment** > **Enrollment device platform restrictions**
2. Select **Create restriction** > **Device limit restriction**
3. Provide a name and description
4. Set the **Device limit** (number of devices each user can enroll, between **1 and 15**)
5. Assign the policy to user groups
6. Set the priority order if you have multiple device limit policies
   <img width="808" height="270" alt="image" src="https://github.com/user-attachments/assets/093b17a5-ebe7-4eb7-8125-74da6291a598" />

> **Note:** The device limit applies **across all device platforms**. If a user has enrolled 5 devices (2 Windows, 2 iOS, 1 Android) and the limit is 5, they can't enroll any more devices until they retire at least one.

---

### Block Personally Owned Devices

#### What Is Ownership?

Intune tracks device ownership with two values:

| Value | Description |
|---|---|
| **Corporate** | Device is owned by the organization |
| **Personal** | Device is owned by the user (BYOD) |

#### How Ownership Is Determined

**Corporate-owned devices are identified by:**
- Devices enrolled through Apple Automated Device Enrollment (ADE)
- Devices enrolled through Windows Autopilot
- Devices manually marked as corporate-owned in Intune or Entra ID
- Devices imported via CSV with serial numbers or IMEI

**Personal devices are:**
- Any device not meeting the corporate-owned criteria above
- Devices enrolled by a user via Settings > Access work or school (by default)

#### Configure the Block

In a device type restriction policy:

1. Select a platform (Windows, iOS/iPadOS, Android, macOS)
2. Set **Block personally owned devices** to **Yes**
3. Assign the policy and set priority

**Impact:**
- Users attempting to enroll a personally owned device see an error message
- Only devices marked as corporate-owned can enroll

> **⚠️ Warning:** Blocking personally owned devices can prevent legitimate enrollments if you haven't properly pre-registered devices as corporate-owned. Ensure corporate devices are imported via Autopilot, Apple ADE, or manual CSV import **before** enforcing this restriction.

---

### Restrict by Manufacturer (Android)

For Android devices, you can restrict enrollment by device manufacturer.

#### Allow List
- Only devices from the specified manufacturers can enroll
- All other manufacturers are blocked
- **Example:** Allow only Samsung and Google devices

#### Block List
- Devices from the specified manufacturers are blocked
- All other manufacturers are allowed
- **Example:** Block devices from unknown or untrusted manufacturers

> **💡 Tip:** Use allow lists to enforce corporate device standards (e.g., only Samsung Galaxy devices for Android) or block lists to prevent known problematic devices from enrolling.

---

### Common Restriction Scenarios

#### Scenario 1: Block iOS Enrollment for All Users Except Executives

- **Default policy**: Block iOS/iPadOS enrollment
- **Targeted policy** (Priority 1):
  - Allow iOS/iPadOS enrollment
  - Assign to "Executives" group

**Result:** Only users in the Executives group can enroll iOS devices.

---

#### Scenario 2: Allow Only Corporate-Owned Windows Devices

- **Device type restriction policy**:
  - Allow Windows enrollment
  - Block personally owned devices: Yes
  - Assign to "All users"
- Pre-register all corporate devices via Windows Autopilot

**Result:** Only Windows devices registered in Autopilot can be enrolled.

---

#### Scenario 3: Limit Sales Team to 3 Devices Each

- **Device limit restriction policy**:
  - Device limit: 3
  - Assign to "Sales team" group
  - Set priority 1
- **Default policy**: Device limit 15

**Result:** Sales team members can enroll up to 3 devices; other users can enroll up to 15.

---

#### Scenario 4: Block Android Device Administrator, Allow Only Android Enterprise

- **Device type restriction policy**:
  - Block Android device administrator enrollment
  - Allow Android Enterprise work profile enrollment
  - Assign to "All users"

**Result:** Users must enroll Android devices using work profiles (modern management), not legacy device administrator mode.

---

### Monitor and Troubleshoot Enrollment Restrictions

#### View Enrollment Failures

1. In the Intune admin center, navigate to **Devices** > **Monitor** > **Enrollment failures**
2. Review the list of failed enrollments
3. Look for failures caused by **Enrollment restriction**
4. Review the error details to see which restriction policy blocked the enrollment
   <img width="1517" height="704" alt="image" src="https://github.com/user-attachments/assets/deea96ed-6511-45d7-804f-93d2a6947c48" />

#### User Experience When Blocked

| Platform | Error Message |
|---|---|
| **Windows** | "This device can't be enrolled. Please contact your IT administrator." (in Settings > Accounts > Access work or school) |
| **iOS/iPadOS** | "This device can't be enrolled because it doesn't meet your organization's requirements." (in Company Portal after attempting to install the management profile) |
| **Android** | "Your device isn't allowed to enroll." (in Company Portal during work profile setup) |

> **💡 Tip:** Customize the Company Portal app with your IT support contact information so users know who to reach when enrollment is blocked.

#### Common Restriction Issues

| Issue | Cause | Solution |
|---|---|---|
| Corporate device is blocked as personally owned | Device wasn't pre-registered as corporate-owned | Import the device serial number or IMEI via **Intune > Devices > Enroll devices > Corporate device identifiers** |
| User exceeds device limit but has fewer devices than the limit | User has retired devices that still temporarily count toward the limit | Wait 24 hours for retired devices to be removed from the count, or manually delete devices from Intune |
| Enrollment blocked for user even though no restrictive policies are assigned | Default policy blocks the platform, and no targeted policy overrides it | Modify the default policy or create a targeted policy with higher priority |

---

### Best Practices for Enrollment Restrictions

- **Start lenient, then tighten** — Begin with permissive default policies and apply restrictions only to specific groups as needed
- **Pre-register corporate devices** — Before blocking personally owned devices, ensure all corporate devices are imported via Autopilot, Apple ADE, or CSV
- **Communicate with users** — Inform users about device requirements and enrollment restrictions before they attempt to enroll
- **Monitor enrollment failures** — Regularly review the enrollment failures report to identify blocked enrollments that may be legitimate
- **Use priority wisely** — Assign highest priority (Priority 1) to the most specific policies (e.g., executive exceptions) and lower priority to broader policies
- **Test restrictions** — Use a test user account in a test group to verify enrollment restrictions work as expected before deploying to production
- **Document exceptions** — Maintain a list of groups and users with special enrollment restrictions for audit and compliance purposes

---

### Restrictions at Contoso

| Device Category | Restrictions | Rationale |
|---|---|---|
| **Corporate Windows** | Windows 10 19041+, block personal, Autopilot pre-registration | Maintain control over corporate devices |
| **Mobile iOS/iPadOS** | Allow corporate and personal | Enable flexible BYOD |
| **Mobile Android** | Work profile (personal only), block device administrator, block macOS | Support BYOD with work/personal separation |
| **Device Limits** | Standard users: 3, IT staff: 5, Executives: 15 | Prevent excessive enrollments |

---

## 5. Troubleshoot Device Enrollment

Device enrollment doesn't always go smoothly. Network issues, configuration errors, permission problems, and certificate issues can all prevent successful enrollment. When enrollment fails, you need to quickly diagnose and resolve the issue to get users up and running.

---

### Common Enrollment Failure Scenarios

#### Issue 1: Enrollment Blocked by Restrictions

**Symptom:**
- User receives error: "This device can't be enrolled" or "Your device doesn't meet requirements"
- Enrollment fails immediately without device appearing in Intune

**Cause:**
- Enrollment restriction policy blocks the device platform, OS version, or ownership type
- User has exceeded the device limit

**Detailed Resolution Steps:**

1. Navigate to **Devices** > **Monitor** > **Enrollment failures** in Intune admin center
2. Locate the failed enrollment attempt for the user
3. Check the failure reason: "Enrollment restriction"
4. Review the user's assigned restriction policies:
   - **Devices** > **Enrollment** > **Enrollment device platform restrictions**
   - Identify which policy blocked the enrollment
5. Choose one of the following:
   - **Modify the restriction policy** to allow the device
   - **Create an exception policy** with higher priority for specific users
   - **Pre-register the device as corporate-owned** if personally owned devices are blocked

> **💡 Tip:** To quickly identify which restriction policy is blocking enrollment, sign in to the Intune admin center with the user's account and attempt enrollment. The error message indicates the restriction in effect.

---

#### Issue 2: User Lacks Licenses or Permissions

**Symptom:**
- Enrollment starts but fails during registration
- Error message: "You are not licensed to use Microsoft Intune" or "Access denied"

**Cause:**
- User doesn't have an Intune license assigned
- User's account is not in a group that has automatic enrollment enabled
- User doesn't have permissions to enroll devices

**Resolution:**

1. Navigate to **Microsoft Entra admin center** > **Users** > **All users**
2. Select the user and go to **Licenses**
3. Verify the user has one of:
   - Microsoft Intune Plan 1 (standalone)
   - Microsoft 365 E3, E5, F1, or F3
   - Enterprise Mobility + Security (EMS) E3 or E5
4. If no license is assigned:
   - Select **Assignments** and assign an appropriate license
   - Wait **15–30 minutes** for license propagation
5. Verify automatic enrollment:
   - **Microsoft Entra admin center** > **Mobility (MDM and WIP)** > **Microsoft Intune**
   - Confirm that the user's group is in the **MDM user scope**

---

#### Issue 3: Network Connectivity Issues

**Symptom:**
- Enrollment hangs or times out
- Error message: "Can't reach the server" or "Something went wrong"

**Resolution:**

> **⚠️ Important:** Verify internet connectivity and ensure required Intune endpoints are accessible:
> - `login.microsoftonline.com`
> - `enrollment.manage.microsoft.com`
> - `portal.manage.microsoft.com`

Use PowerShell to test connectivity:

```powershell
Test-NetConnection -ComputerName enrollment.manage.microsoft.com -Port 443
```

For firewall and proxy configuration, see [Network requirements for Microsoft Intune](https://learn.microsoft.com/en-us/mem/intune/fundamentals/intune-endpoints).

---

#### Issue 4: Certificate Errors

**Symptom:**
- Enrollment fails with a certificate error
- Error codes: `0x80180014`, `0x80090011`, or similar certificate-related codes

**Resolution:**

> **⚠️ Important — Most common cause:** Incorrect device date and time causes certificate validation failures.
> 1. Verify **Settings** > **Time & Language** > **Date & Time** is correct and set to synchronize automatically
> 2. Restart the device and retry enrollment
> 3. For advanced certificate troubleshooting, see [Troubleshoot Windows enrollment errors](https://learn.microsoft.com/en-us/mem/intune/enrollment/troubleshoot-windows-enrollment-errors)

---

#### Issue 5: Automatic Enrollment Not Triggered

**Symptom:**
- User successfully joins device to Microsoft Entra ID, but device doesn't appear in Intune
- No MDM enrollment happens automatically

**Resolution:**

> **⚠️ Important:**
> 1. Verify automatic enrollment is configured in **Microsoft Entra admin center** > **Devices** > **Device settings** > **Microsoft Intune**
> 2. Ensure **MDM user scope** is set to **All** or includes the user's group
> 3. Use `dsregcmd /status` to check if **MDMUrl** is populated. If blank, automatic enrollment isn't configured
> 4. Manually trigger sync via **Settings** > **Accounts** > **Access work or school** > **Info** > **Sync**

---

### Use Diagnostic Tools

The Intune **Troubleshooting + support** blade provides a centralized view of enrollment issues and error codes.

1. In the Intune admin center, navigate to **Troubleshooting + support** > **Troubleshoot**
2. Search for the user experiencing enrollment issues
3. Review the **Enrollment failures** section for error codes and recommended resolutions

For detailed diagnostic procedures including Windows Event Viewer logs and Company Portal diagnostics, see [Troubleshoot device enrollment in Microsoft Intune](https://learn.microsoft.com/en-us/mem/intune/enrollment/troubleshoot-device-enrollment-in-intune).
<img width="1156" height="590" alt="image" src="https://github.com/user-attachments/assets/2b030056-cd50-4754-bf06-c67a1fe53a84" />

---

### Common Error Codes and Resolutions

| Error Code | Description | Resolution |
|---|---|---|
| **0x80180014** | The server could not process the request because the certificate is invalid. | Verify device date and time are correct; restart the device; clear cached certificates and retry enrollment |
| **0x801c03ea** | The user is not authorized to enroll. | Verify the user has an Intune license assigned; check that automatic enrollment is enabled for the user's group; ensure user account is not disabled or locked |
| **0x80180026** | Enrollment failed due to a network connectivity issue. | Verify internet connectivity; check firewall and proxy settings; test access to Intune endpoints (`enrollment.manage.microsoft.com`) |
| **0x80180002b** | The device is already enrolled in MDM. | Unenroll the device: **Settings** > **Accounts** > **Access work or school** > Select account > **Disconnect**; re-enroll with the correct user account |
| **0xcaa9001f** | The user declined the enrollment prompt. | User canceled enrollment by closing the prompt; restart enrollment process and ensure user completes all prompts |

---

### Resolve Device Sync Issues

#### Device Not in Sync (Last Check-In Is Old)

**Symptom:**
- Device shows "Last check-in" timestamp older than 24 hours in the Intune admin center
- Policies not updating on the device

**Resolution:**

1. Manually trigger sync: **Settings** > **Accounts** > **Access work or school** > **Info** > **Sync**
2. Verify MDMUrl is populated using `dsregcmd /status`. If blank, re-enroll the device
3. Restart the **Microsoft Intune Management Extension** service if needed

For detailed sync troubleshooting, see [Troubleshoot Windows enrollment errors](https://learn.microsoft.com/en-us/mem/intune/enrollment/troubleshoot-windows-enrollment-errors).

---

#### Sync Completes but Policies Don't Apply

**Symptom:**
- Device syncs successfully (recent check-in timestamp)
- Assigned policies show "Pending" or "Error" status

**Resolution:**

1. Navigate to **Devices** > **All devices** > Select device > **Device configuration** to review policy status
2. For errors, select the policy to view per-setting details and correct the configuration
3. Note: "Pending" policies can take up to **8 hours** for the next sync cycle

---

### Best Practices for Troubleshooting

- Use **Troubleshooting + support** > **Troubleshoot** in the Intune admin center for a quick enrollment status overview
- Use `dsregcmd /status` for detailed device registration and MDM enrollment diagnostics
- Monitor **Devices** > **Monitor** > **Enrollment failures** regularly to identify trends
- Pre-register corporate devices via Autopilot to avoid ownership issues

For comprehensive troubleshooting guidance, see [Troubleshoot device enrollment in Microsoft Intune](https://learn.microsoft.com/en-us/mem/intune/enrollment/troubleshoot-device-enrollment-in-intune).

---

### Troubleshoot at Contoso

| Problem | Troubleshooting Steps | Resolution |
|---|---|---|
| "This device can't be enrolled" error | 1. Reviewed enrollment failures 2. Found "Enrollment restriction" error 3. Checked restriction policy (personally owned Windows blocked) 4. Verified devices are corporate-owned 5. Discovered devices not pre-registered in Autopilot | Imported device serial numbers via Autopilot; instructed users to restart enrollment |

**Outcome:** By pre-registering devices as corporate-owned, Contoso resolves the enrollment restriction issue and ensures future corporate devices enroll smoothly.

---

## Confirmed Scenarios with Detailed Solutions

The following scenarios are drawn directly from the module and validated as real-world applicable implementations.

---

### Scenario 1: Manual Windows Enrollment for Remote Workers (Existing Devices)

**Problem:** 200 existing Windows desktops used by remote workers need to be enrolled in Intune without re-imaging.

**Solution:**

1. **Confirm prerequisites** on each device:
   - Windows 10 version 1809+ or Windows 11
   - Device is NOT already joined to another Entra ID tenant
   - User has an assigned Intune license
2. **Configure automatic enrollment** in Microsoft Entra admin center:
   - Navigate to **Devices** > **Device settings** > **Mobility (MDM and MAM)** > **Microsoft Intune**
   - Set **MDM user scope** to **Some** and target the remote workers' security group
3. **Have users enroll** via **Settings** > **Accounts** > **Access work or school** > **Connect** > Sign in with corporate credentials > **Join**
4. **Validate** enrollment in the Intune admin center: **Devices** > **All devices**
5. **Confirm MDM status** on the device using:
   ```cmd
   dsregcmd /status
   ```
   Verify `AzureAdJoined: YES` and `MDMUrl` is populated

**Sources:**
- [Enroll Windows devices in Intune](https://learn.microsoft.com/en-us/mem/intune/enrollment/windows-enroll)

---

### Scenario 2: Enrollment Fails — "Device Can't Be Enrolled" (Ownership Restriction)

**Problem:** Finance department users receive error "This device can't be enrolled" when attempting to enroll new laptops. The department has a policy blocking personally owned devices.

**Solution:**

1. Navigate to **Devices** > **Monitor** > **Enrollment failures** and find the failed enrollment
2. Confirm failure reason: "Enrollment restriction — personally owned device blocked"
3. Identify the restriction policy under **Devices** > **Enrollment** > **Enrollment device platform restrictions**
4. **Pre-register the devices as corporate-owned** via Autopilot:
   - Obtain the hardware hash CSV from the OEM or using the [Get-WindowsAutoPilotInfo](https://www.powershellgallery.com/packages/Get-WindowsAutoPilotInfo) PowerShell script
   - Upload to **Devices** > **Windows** > **Windows enrollment** > **Devices** (Autopilot)
5. Alternatively, import serial numbers manually: **Devices** > **Enroll devices** > **Corporate device identifiers**
6. Instruct users to retry enrollment

**Sources:**
- [Corporate device identifiers](https://learn.microsoft.com/en-us/mem/intune/enrollment/corporate-identifiers-add)
- [Windows Autopilot device registration](https://learn.microsoft.com/en-us/autopilot/add-devices)

---

### Scenario 3: Device Enrolled But Policies Not Applying

**Problem:** A device successfully enrolled but assigned configuration profiles show "Pending" or "Error" in the Intune admin center.

**Solution:**

1. In the Intune admin center, navigate to **Devices** > **All devices** > select the device > **Device configuration**
2. Identify any policies with **Error** status and select the policy to view per-setting error details
3. For **Pending** status — trigger a manual sync on the device:
   - **Settings** > **Accounts** > **Access work or school** > **Info** > **Sync**
4. For **Conflict** status — identify overlapping policies and resolve conflicting settings
5. Check Event Viewer on the device for policy application events:
   ```
   Applications and Services Logs > Microsoft > Windows > DeviceManagement-Enterprise-Diagnostics-Provider > Admin
   ```
   - Filter Event ID 401 for policy failures

**Sources:**
- [Monitor device configuration profiles in Intune](https://learn.microsoft.com/en-us/mem/intune/configuration/device-profile-monitor)

---

### Scenario 4: Device Shows "Microsoft Entra Registered" Instead of "Microsoft Entra Joined"

**Problem:** A user enrolled by adding their work account via the old "Add a work or school account" flow rather than joining the device. The device now shows as registered instead of joined, limiting MDM policy coverage.

**Solution:**

1. Confirm join type using `dsregcmd /status`:
   ```
   AzureAdJoined : NO
   WorkplaceJoined : YES
   ```
2. Disconnect the current work account:
   - **Settings** > **Accounts** > **Access work or school** > Select the account > **Disconnect**
3. Re-enroll using the correct join flow:
   - **Settings** > **Accounts** > **Access work or school** > **Connect** > Enter corporate email > **Join this device to Microsoft Entra ID**
4. Validate that `dsregcmd /status` now shows `AzureAdJoined: YES`

**Sources:**
- [Microsoft Entra joined vs. registered devices](https://learn.microsoft.com/en-us/entra/identity/devices/concept-device-registration)

---

### Scenario 5: Certificate Error During Enrollment (Error Code 0x80180014)

**Problem:** A user receives error code `0x80180014` when trying to enroll a Windows device.

**Solution:**

1. Verify the device's date and time are accurate:
   - **Settings** > **Time & Language** > **Date & Time**
   - Enable **Set time automatically** and **Set time zone automatically**
2. Restart the device
3. If the issue persists, clear cached certificates:
   - Open **certmgr.msc** (Certificate Manager)
   - Under **Personal** > **Certificates**, delete any expired or invalid Microsoft Intune-related certificates
4. Retry enrollment
5. If still failing, check the enrollment event log:
   ```
   eventvwr.msc > Applications and Services Logs > Microsoft > Windows > DeviceManagement-Enterprise-Diagnostics-Provider
   ```

**Sources:**
- [Troubleshoot Windows enrollment errors](https://learn.microsoft.com/en-us/mem/intune/enrollment/troubleshoot-windows-enrollment-errors)

---

## Related Scenarios and Detailed Solutions

---

### Related Scenario 1: iOS/iPadOS Device Enrollment via Company Portal (BYOD)

**Challenge:** Employees need to enroll personal iPhones to access corporate email and Teams.

**Solution:**

1. Ensure **APNs certificate** is configured: Intune admin center > **Devices** > **Enrollment** > **Apple** > **Apple MDM Push certificate**
2. Create an **iOS user enrollment profile** for BYOD
3. Instruct users to:
   - Download the **Intune Company Portal** app from the App Store
   - Open the app, sign in with corporate credentials
   - Follow on-screen prompts to install the management profile
4. Apply **App Protection Policies (MAM)** to control corporate data within apps without full device management
5. Validate enrollment in the Intune admin center

**Sources:**
- [iOS/iPadOS user enrollment](https://learn.microsoft.com/en-us/mem/intune/enrollment/ios-user-enrollment)
- [APNs certificate setup](https://learn.microsoft.com/en-us/mem/intune/enrollment/apple-mdm-push-certificate-get)

---

### Related Scenario 2: Validate Hybrid Entra Join for Existing Domain Devices

**Challenge:** On-premises domain-joined devices need to be validated as hybrid-joined and enrolled in Intune.

**Solution:**

1. Run `dsregcmd /status` on the device and confirm:
   ```
   AzureAdJoined : YES
   DomainJoined : YES
   ```
2. Check that **Microsoft Entra Connect** is syncing the device objects from on-premises AD
3. Verify the device appears in **Microsoft Entra admin center** > **Devices** > **All devices** with join type "Hybrid Microsoft Entra joined"
4. If co-management is enabled, confirm the device also appears in the **Intune admin center** > **Devices** > **All devices**

**Sources:**
- [Hybrid Microsoft Entra join validation](https://learn.microsoft.com/en-us/entra/identity/devices/troubleshoot-hybrid-join-windows-current)

---

### Related Scenario 3: Bulk Import of Corporate Device Identifiers (Pre-Registering Owned Devices)

**Challenge:** You have a large batch of corporate devices that aren't enrolled in Autopilot but need to be marked as corporate-owned before users enroll them.

**Solution:**

1. Collect device serial numbers or IMEI numbers from your asset register
2. Create a CSV file in the format:
   ```csv
   serial number,IMEI,manufacturer
   ABC123456,123456789012345,Dell
   ```
3. In the Intune admin center, navigate to **Devices** > **Enroll devices** > **Corporate device identifiers**
4. Select **Add** > **CSV file** and upload the file
5. After import, enrolled devices with matching serial numbers/IMEI will automatically be marked as Corporate-owned

**Sources:**
- [Add corporate identifiers to Intune](https://learn.microsoft.com/en-us/mem/intune/enrollment/corporate-identifiers-add)

---

### Related Scenario 4: Enforcing Minimum OS Version for Security Compliance

**Challenge:** You need to ensure only devices running Windows 10 20H1 (build 19041) or later can enroll, preventing older, potentially vulnerable OS versions.

**Solution:**

1. In the Intune admin center, navigate to **Devices** > **Enrollment** > **Enrollment device platform restrictions**
2. Create or edit a **Device type restriction**
3. Under **Windows**:
   - Set **Minimum OS version** to `10.0.19041.0`
   - Optionally set **Maximum OS version** if you need to cap versions
4. Assign the policy to the appropriate user groups
5. Test by attempting enrollment from an older Windows device — it should be blocked
6. Check **Devices** > **Monitor** > **Enrollment failures** to confirm the restriction is working

**Sources:**
- [Set enrollment restrictions](https://learn.microsoft.com/en-us/mem/intune/enrollment/enrollment-restrictions-set)

---

## Tools and Resources Used

| Tool / Resource | Description | Link |
|---|---|---|
| **Microsoft Intune admin center** | Primary portal for managing devices, policies, restrictions, and troubleshooting | [intune.microsoft.com](https://intune.microsoft.com) |
| **Microsoft Entra admin center** | Identity and device management portal (formerly Azure AD) | [entra.microsoft.com](https://entra.microsoft.com) |
| **`dsregcmd /status`** | Windows command-line tool for detailed device join and MDM enrollment status | Run from Command Prompt or PowerShell as Administrator |
| **Windows Event Viewer (`eventvwr.msc`)** | Logs policy application, sync, and MDM events on the device | Path: `Applications and Services Logs > Microsoft > Windows > DeviceManagement-Enterprise-Diagnostics-Provider` |
| **`certmgr.msc`** | Windows Certificate Manager — used to clear invalid enrollment certificates | Run from the Run dialog (Windows key + R) |
| **`Test-NetConnection` (PowerShell)** | Tests network connectivity to Intune endpoints | `Test-NetConnection -ComputerName enrollment.manage.microsoft.com -Port 443` |
| **Intune Company Portal App** | End-user app for iOS/iPadOS and Android enrollment and compliance | [Company Portal in App Store](https://apps.apple.com/app/intune-company-portal/id719171358) / [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser) |
| **Windows Autopilot** | Zero-touch Windows device provisioning and corporate registration | [Autopilot docs](https://learn.microsoft.com/en-us/autopilot/windows-autopilot) |
| **Apple Automated Device Enrollment (ADE)** | Zero-touch supervised iOS/macOS enrollment via Apple Business Manager | [Apple Business Manager](https://business.apple.com) |
| **Apple Push Notification service (APNs)** | Required certificate for Intune communication with Apple devices | [APNs certificate setup](https://learn.microsoft.com/en-us/mem/intune/enrollment/apple-mdm-push-certificate-get) |
| **Managed Google Play** | Required integration for Android Enterprise enrollment | Configured in Intune admin center > Devices > Enrollment > Android |
| **Get-WindowsAutoPilotInfo** | PowerShell script for harvesting hardware hashes from existing Windows devices | [PowerShell Gallery](https://www.powershellgallery.com/packages/Get-WindowsAutoPilotInfo) |
| **Intune Troubleshooting + support blade** | Centralized view of user enrollment failures and error codes in the Intune portal | Intune admin center > Troubleshooting + support > Troubleshoot |
| **Intune Enrollment Failures Report** | Monitor report for all failed enrollment attempts with failure reasons | Intune admin center > Devices > Monitor > Enrollment failures |
| **Network endpoints reference** | Required firewall/proxy allowlist for Intune connectivity | [Network requirements for Microsoft Intune](https://learn.microsoft.com/en-us/mem/intune/fundamentals/intune-endpoints) |
| **Enroll iOS/iPadOS docs** | Official guide for iOS/iPadOS device enrollment | [iOS/iPadOS enrollment](https://learn.microsoft.com/en-us/mem/intune/enrollment/ios-enroll) |
| **Enroll Android docs** | Official guide for Android device enrollment | [Android enrollment](https://learn.microsoft.com/en-us/mem/intune/enrollment/android-enroll) |
| **Troubleshoot Windows enrollment errors** | In-depth Windows enrollment error code reference | [Windows enrollment troubleshooting](https://learn.microsoft.com/en-us/mem/intune/enrollment/troubleshoot-windows-enrollment-errors) |
| **Troubleshoot device enrollment (general)** | General enrollment troubleshooting guide for all platforms | [General enrollment troubleshooting](https://learn.microsoft.com/en-us/mem/intune/enrollment/troubleshoot-device-enrollment-in-intune) |
| **Set enrollment restrictions** | Documentation for creating and managing enrollment restriction policies | [Enrollment restrictions](https://learn.microsoft.com/en-us/mem/intune/enrollment/enrollment-restrictions-set) |
| **Corporate identifiers add** | Steps for importing corporate device serial numbers and IMEIs | [Corporate identifiers](https://learn.microsoft.com/en-us/mem/intune/enrollment/corporate-identifiers-add) |

---
