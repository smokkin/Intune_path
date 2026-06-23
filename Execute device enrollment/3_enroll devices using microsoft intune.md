# Enroll Devices by Using Microsoft Intune

---

## Overview

Microsoft Intune is a cloud-based endpoint management solution used to enroll, configure, secure, monitor, and remotely manage organization-owned and personally owned devices. Intune works with Microsoft Entra ID to register and enroll devices so they can access organizational resources securely. During enrollment, Intune installs a Mobile Device Management certificate on the device, which allows the Intune service to enforce policies such as enrollment restrictions, compliance policies, and configuration profiles. evices with Intune

Microsoft Intune supports mobile device management for both personal devices and corporate-owned devices. Personal devices are commonly used in bring-your-own-device scenarios, while corporate-owned devices are purchased and distributed by an organization for work or school use. [1](https://learn.microsoft.com/en-us/intune/device-enrollment/guide)

After a device is enrolled, Intune can deploy policies and profiles to configure device settings, enforce compliance requirements, deploy applications, protect organizational data, and support Conditional Access decisions. [1](https://learn.microsoft.com/en-us/intune/device-enrollment/guide)[2](https://learn.microsoft.com/en-us/intune/device-enrollment/enroll-devices)

Common management goals include:

- Allow users to access work resources from approved devices.
- Apply security policies such as password, encryption, and compliance requirements.
- Separate work data from personal data where supported.
- Restrict enrollment by platform, ownership, OS version, or device count.
- Remotely manage devices using actions such as sync, retire, wipe, restart, or remote lock.

---

## Enable Mobile Device Management

Before devices can enroll successfully, the tenant must be prepared for Intune device management. Required preparation typically includes licensing users, setting Intune as the mobile device management authority, configuring automatic enrollment where needed, and preparing platform-specific enrollment requirements. [3](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/enable-automatic-mdm)[4](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/quickstart-automatic-mdm)

### Prerequisites

At minimum, the following are required:

- An active Microsoft Intune subscription.
- Microsoft Entra ID configuration for users and devices.
- Microsoft Entra ID P1 or P2 for Windows automatic MDM enrollment.
- Appropriate administrator role, such as Global Administrator or Intune Administrator.
- Intune licenses assigned to users who will enroll devices.
- Platform-specific prerequisites such as Apple MDM push certificate, Managed Google Play connection, or Windows automatic enrollment configuration. [3](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/enable-automatic-mdm)[4](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/quickstart-automatic-mdm)[5](https://learn.microsoft.com/en-us/intune/device-enrollment/apple/setup-automated-ios)[6](https://learn.microsoft.com/en-us/intune/device-enrollment/android/setup-corporate-work-profile)

### Enable Windows Automatic Enrollment

Windows automatic enrollment allows Windows devices to enroll automatically when users add a work or school account or when corporate devices join Microsoft Entra ID. [3](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/enable-automatic-mdm)[4](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/quickstart-automatic-mdm)

Steps:

1. Sign in to the Microsoft Intune admin center.
2. Go to **Devices**.
3. Expand **Device onboarding**.
4. Select **Enrollment**.
5. Select the **Windows** tab.
6. Select **Automatic Enrollment**.
7. Configure **MDM user scope**:
   - **None**: Automatic MDM enrollment is disabled.
   - **Some**: Automatic enrollment applies only to selected users or groups.
   - **All**: Automatic enrollment applies to all users.
8. Save the configuration. [3](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/enable-automatic-mdm)[4](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/quickstart-automatic-mdm)

### Important Note About MDM and WIP User Scope

If the same user is targeted by both MDM user scope and Windows Information Protection user scope, behavior can differ based on device ownership. For corporate-owned devices, MDM user scope takes precedence and the device enrolls in Intune. For bring-your-own-device scenarios, WIP scope can take precedence and the device may not enroll for MDM management. [4](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/quickstart-automatic-mdm)

---

## Device Enrollment Considerations

Before choosing an enrollment method, consider device ownership, platform, user association, scale, privacy expectations, compliance requirements, and whether the device is new, existing, shared, kiosk-based, or previously managed by another MDM provider. [1](https://learn.microsoft.com/en-us/intune/device-enrollment/guide)[2](https://learn.microsoft.com/en-us/intune/device-enrollment/enroll-devices)

### Key Questions

Ask the following before enrollment:

- Is the device personally owned or corporate-owned?
- Is the device assigned to one user, multiple users, or no user?
- Does the organization need full device management or app/data protection only?
- Does the device need to be wiped or factory reset before enrollment?
- Is the device already enrolled in another MDM solution?
- Are enrollment restrictions required?
- Are Conditional Access policies dependent on device compliance?

### Existing MDM Enrollment

If devices are currently enrolled in another MDM provider, they should generally be unenrolled from the existing MDM provider before enrolling in Intune. A factory reset may be required depending on platform and scenario. [2](https://learn.microsoft.com/en-us/intune/device-enrollment/enroll-devices)[7](https://learn.microsoft.com/en-us/intune/device-enrollment/android/guide)

---

## Manage Corporate Enrollment Policy

Intune enrollment restrictions help control which devices are allowed to enroll. Restrictions can be configured by platform, OS version, manufacturer, ownership type, or number of devices per user. [8](https://learn.microsoft.com/en-us/intune/device-enrollment/create-platform-restrictions)[9](https://learn.microsoft.com/en-us/intune/device-enrollment/restrictions)

### Types of Enrollment Restrictions

Intune supports two main types of enrollment restrictions:

1. **Device platform restrictions**
   - Restrict enrollment by platform.
   - Restrict by OS version.
   - Restrict by manufacturer where supported.
   - Restrict personally owned devices.

2. **Device limit restrictions**
   - Restrict how many devices a user can enroll.
   - Device limits can be configured from 1 to 15 devices per user. [9](https://learn.microsoft.com/en-us/intune/device-enrollment/restrictions)

### Default Restriction Policy

Intune includes default enrollment restriction policies. These default policies apply to all user and userless enrollments until a higher-priority restriction is assigned. [8](https://learn.microsoft.com/en-us/intune/device-enrollment/create-platform-restrictions)[9](https://learn.microsoft.com/en-us/intune/device-enrollment/restrictions)

### Important Security Note

Enrollment restrictions are not security features. They are best-effort barriers for non-malicious users because compromised devices can misrepresent device characteristics. [9](https://learn.microsoft.com/en-us/intune/device-enrollment/restrictions)

### Create a Device Platform Restriction

Steps:

1. Sign in to the Microsoft Intune admin center.
2. Go to **Devices**.
3. Under **Device onboarding**, select **Enrollment**.
4. Under **Enrollment options**, select **Device platform restriction**.
5. Choose the platform, such as Android, iOS/iPadOS, macOS, or Windows.
6. Create or edit a restriction.
7. Configure whether the platform is allowed or blocked.
8. Configure ownership restrictions, minimum/maximum OS versions, or manufacturer restrictions where supported.
9. Assign the restriction to users or groups.
10. Review and create the restriction. [8](https://learn.microsoft.com/en-us/intune/device-enrollment/create-platform-restrictions)[9](https://learn.microsoft.com/en-us/intune/device-enrollment/restrictions)

---

## Enroll Windows Devices in Intune

Windows devices can be enrolled in Intune using several methods, including Windows automatic enrollment, Windows Autopilot, Company Portal-based bring-your-own-device enrollment, co-management with Configuration Manager, Group Policy, and provisioning packages. [10](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/guide)[2](https://learn.microsoft.com/en-us/intune/device-enrollment/enroll-devices)

### Windows Enrollment Options

| Enrollment Method | Best Used For |
|---|---|
| Windows automatic enrollment | Personal or corporate Windows devices that join or register with Microsoft Entra ID |
| Windows Autopilot | New or reset corporate-owned devices requiring modern provisioning |
| Company Portal enrollment | BYOD or existing devices where users initiate enrollment |
| Group Policy enrollment | Hybrid Microsoft Entra joined domain devices |
| Provisioning package | Bulk or shared corporate-owned devices |
| Co-management | Environments using Configuration Manager and Intune together |

[10](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/guide)[4](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/quickstart-automatic-mdm)

### Windows Automatic Enrollment Steps

1. Confirm users have Intune licenses.
2. Confirm Microsoft Entra ID P1 or P2 is available.
3. Sign in to the Intune admin center.
4. Go to **Devices > Enrollment > Windows > Automatic Enrollment**.
5. Set **MDM user scope** to **Some** for a pilot group or **All** for all users.
6. Save the configuration.
7. Join or register the Windows device with Microsoft Entra ID.
8. Confirm that the device appears in Intune. [3](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/enable-automatic-mdm)[4](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/quickstart-automatic-mdm)

### Windows Autopilot

Windows Autopilot is recommended for organization-owned devices where IT wants a streamlined out-of-box provisioning experience. Devices can be registered with Autopilot, assigned deployment profiles, and automatically enrolled in Intune during setup. [10](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/guide)

### BYOD Windows Enrollment

For personal Windows devices, users can enroll by adding a work or school account or by using the Company Portal app, depending on the organization’s enrollment configuration. [10](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/guide)[4](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/quickstart-automatic-mdm)

---

## Enroll Android Devices in Intune

Android enrollment in Intune supports both personal and corporate-owned scenarios. Android Enterprise is the recommended modern management framework for Android devices. [7](https://learn.microsoft.com/en-us/intune/device-enrollment/android/guide)[6](https://learn.microsoft.com/en-us/intune/device-enrollment/android/setup-corporate-work-profile)

### Android Enrollment Options

| Enrollment Method | Best Used For |
|---|---|
| Android Enterprise personally owned work profile | BYOD Android devices |
| Android Enterprise fully managed | Corporate-owned, business-only devices |
| Android Enterprise dedicated devices | Kiosk, shared, or single-purpose devices |
| Android Enterprise corporate-owned work profile | Corporate-owned devices with work and personal separation |
| Android Open Source Project | Supported AOSP scenarios |
| Android device administrator | Legacy scenarios only |

[7](https://learn.microsoft.com/en-us/intune/device-enrollment/android/guide)

### Important Android Device Administrator Note

Android device administrator management is deprecated and is no longer available for devices with access to Google Mobile Services. Organizations using Android device administrator should move to another Android management option where possible. [8](https://learn.microsoft.com/en-us/intune/device-enrollment/create-platform-restrictions)[9](https://learn.microsoft.com/en-us/intune/device-enrollment/restrictions)

### Android Enterprise Corporate-Owned Work Profile

Corporate-owned Android devices with a work profile are single-user devices intended for both work and personal use. Work and personal data remain separated, and admins can manage selected device-wide settings such as password requirements, Bluetooth, data roaming, and factory reset protection. [6](https://learn.microsoft.com/en-us/intune/device-enrollment/android/setup-corporate-work-profile)

### Android Enterprise Requirements

Common requirements include:

- Android Enterprise support.
- Android OS version supported by the chosen enrollment method.
- Google Mobile Services availability for many Android Enterprise scenarios.
- Intune tenant set as MDM authority.
- Intune tenant connected to Managed Google Play.
- Enrollment profile created in Intune. [6](https://learn.microsoft.com/en-us/intune/device-enrollment/android/setup-corporate-work-profile)[7](https://learn.microsoft.com/en-us/intune/device-enrollment/android/guide)

### Android Enrollment Setup Summary

1. Set Microsoft Intune as the MDM authority.
2. Connect Intune to Managed Google Play.
3. Choose the Android enrollment scenario.
4. Create an enrollment profile.
5. Create or assign device/user groups.
6. Enroll devices using the required enrollment method, such as QR code, token, Company Portal, or zero-touch provisioning where supported.
7. Confirm enrollment status in the Intune admin center. [6](https://learn.microsoft.com/en-us/intune/device-enrollment/android/setup-corporate-work-profile)[7](https://learn.microsoft.com/en-us/intune/device-enrollment/android/guide)

---

## Enroll iOS/iPadOS Devices in Intune

Intune supports iOS/iPadOS enrollment for personal and organization-owned Apple mobile devices. Supported enrollment methods include Automated Device Enrollment, Apple Configurator enrollment, and BYOD user/device enrollment. [11](https://learn.microsoft.com/en-us/intune/device-enrollment/apple/guide-ios-ipados)[5](https://learn.microsoft.com/en-us/intune/device-enrollment/apple/setup-automated-ios)

### iOS/iPadOS Enrollment Options

| Enrollment Method | Best Used For |
|---|---|
| Automated Device Enrollment | Corporate-owned or school-owned devices requiring supervised management |
| Apple Configurator | Devices manually prepared by IT |
| BYOD user/device enrollment | Personal iPhone or iPad devices |

[11](https://learn.microsoft.com/en-us/intune/device-enrollment/apple/guide-ios-ipados)

### Automated Device Enrollment

Automated Device Enrollment is used for organization-owned devices purchased through Apple Business Manager or Apple School Manager. It supports supervised management and large-scale deployment without IT physically touching every device. [11](https://learn.microsoft.com/en-us/intune/device-enrollment/apple/guide-ios-ipados)[5](https://learn.microsoft.com/en-us/intune/device-enrollment/apple/setup-automated-ios)

### iOS/iPadOS ADE Prerequisites

Requirements include:

- Access to Apple Business Manager or Apple School Manager.
- Active Apple ADE token.
- Apple MDM push certificate configured in Intune.
- New or wiped iOS/iPadOS devices purchased through Apple Business Manager or Apple School Manager.
- Intune enrollment policy assigned to devices. [5](https://learn.microsoft.com/en-us/intune/device-enrollment/apple/setup-automated-ios)

### Company Portal Deployment for ADE

For Automated Device Enrollment, deploy the Intune Company Portal app through Intune rather than the App Store. This ensures ADE devices receive the app correctly and can receive automatic updates when configured. [5](https://learn.microsoft.com/en-us/intune/device-enrollment/apple/setup-automated-ios)

### One-Time Microsoft Entra Join Issue

For new and existing tenants, there may be a one-time Microsoft Entra ID join failure during enrollment. A manual sync from the Intune admin center, Company Portal website, or Company Portal app can resolve the issue for future enrollments. [11](https://learn.microsoft.com/en-us/intune/device-enrollment/apple/guide-ios-ipados)

---

## Device Enrollment Manager

A Device Enrollment Manager is a special Intune account used to enroll many corporate-owned devices. This role is useful when a user or technician needs to enroll devices on behalf of other users or prepare devices in bulk. [12](https://github.com/MicrosoftLearning/MD-102T00-Microsoft-365-Endpoint-Administrator/blob/master/Instructions/Labs/0203-Manage%20Device%20Enrollment%20into%20Intune.md)[2](https://learn.microsoft.com/en-us/intune/device-enrollment/enroll-devices)

### Typical Use Cases

Use a Device Enrollment Manager when:

- IT staff must enroll many corporate-owned devices.
- Devices are being staged before being handed to users.
- A standard user enrollment limit is not enough.
- Devices are shared, kiosk-based, or assigned after initial provisioning.

### Important Consideration

Device Enrollment Manager should be used carefully because it increases enrollment capability for the assigned account. Organizations should restrict this role to trusted administrators or device provisioning staff.

---

## Monitor Device Enrollment

Intune provides reports and troubleshooting views to monitor enrollment success and failure. The main monitoring locations include the enrollment failures report, Troubleshooting + support page, and device enrollment report. [13](https://learn.microsoft.com/en-us/intune/device-enrollment/monitor-reports)[14](https://learn.microsoft.com/en-us/troubleshoot/mem/intune/device-enrollment/troubleshoot-device-enrollment-in-intune)

### Enrollment Failures Report

The enrollment failures report shows failed enrollment attempts, including failure date, reason, operating system, OS version, username, and enrollment method. [13](https://learn.microsoft.com/en-us/intune/device-enrollment/monitor-reports)

Steps:

1. Sign in to the Microsoft Intune admin center.
2. Go to **Devices > Monitor**.
3. Select **Enrollment failures**.
4. Choose **All users** or **Select user**.
5. Select a row to view details and recommended remediation steps. [13](https://learn.microsoft.com/en-us/intune/device-enrollment/monitor-reports)

### Troubleshooting + Support Page

The **Troubleshooting + support** page can be used to search for a specific user and review enrollment failures, assignments, devices, and app protection status. [13](https://learn.microsoft.com/en-us/intune/device-enrollment/monitor-reports)[14](https://learn.microsoft.com/en-us/troubleshoot/mem/intune/device-enrollment/troubleshoot-device-enrollment-in-intune)

### Device Enrollment Page

The device enrollment page shows enrollment policies applied to a device when it first enrolled, including enrollment restriction policies and enrollment status page policies. This helps identify unexpected policy targeting, priority, or filter behavior. [13](https://learn.microsoft.com/en-us/intune/device-enrollment/monitor-reports)

---

## Manage Devices Remotely

After enrollment, Intune can remotely manage devices using available remote actions. Remote actions vary by platform and device state. Common actions include sync, restart, retire, wipe, remote lock, locate, rename, and delete. [15](https://www.cybersystem.ca/blog/Intune-Remote-Device-Actions-Guide)[16](https://learn.microsoft.com.mcas.ms/en-us/mem/intune/)

### Common Remote Actions

| Action | Purpose |
|---|---|
| Sync | Forces the device to check in and receive latest policies |
| Restart | Restarts the device remotely where supported |
| Remote lock | Locks the device remotely |
| Retire | Removes company data while preserving personal data where supported |
| Wipe | Factory resets the device and removes data |
| Delete | Removes the device record from Intune |
| Locate device | Locates a supported managed device |
| Rename device | Changes the device name where supported |

[15](https://www.cybersystem.ca/blog/Intune-Remote-Device-Actions-Guide)

### Retire vs Wipe

Use **Retire** when the goal is to remove organizational data from a device without fully resetting it. Use **Wipe** when the device must be factory reset, such as when it is lost, stolen, being reassigned, or being decommissioned. [15](https://www.cybersystem.ca/blog/Intune-Remote-Device-Actions-Guide)

---

## Troubleshooting Guidance

### Initial Troubleshooting Checks

Before troubleshooting a failed enrollment, confirm:

- The user has an Intune license.
- The device platform is supported.
- Enrollment restrictions allow the device.
- The device limit has not been reached.
- The tenant is configured for Intune enrollment.
- Platform-specific prerequisites are complete.
- The device is not already managed by another MDM provider.
- Required network endpoints are reachable. [14](https://learn.microsoft.com/en-us/troubleshoot/mem/intune/device-enrollment/troubleshoot-device-enrollment-in-intune)[2](https://learn.microsoft.com/en-us/intune/device-enrollment/enroll-devices)

### Run Microsoft Self-Help Diagnostics

Microsoft provides diagnostic scenarios in the Microsoft 365 admin center for Intune enrollment issues. These diagnostics do not change the tenant but can detect known configuration issues and provide remediation steps. [14](https://learn.microsoft.com/en-us/troubleshoot/mem/intune/device-enrollment/troubleshoot-device-enrollment-in-intune)

Steps:

1. Go to the Microsoft 365 admin center.
2. Select **Help & support**.
3. Describe the issue, such as `I need help enrolling Windows devices`.
4. Enter the affected user’s email address if prompted.
5. Run the tests.
6. Review detected issues and remediation steps.
7. Rerun diagnostics after applying fixes. [14](https://learn.microsoft.com/en-us/troubleshoot/mem/intune/device-enrollment/troubleshoot-device-enrollment-in-intune)

### Common Enrollment Issues

#### Device Cap Reached

If a user has reached the allowed device limit, enrollment can fail. Review device limit restrictions and remove stale device records if appropriate. [9](https://learn.microsoft.com/en-us/intune/device-enrollment/restrictions)[14](https://learn.microsoft.com/en-us/troubleshoot/mem/intune/device-enrollment/troubleshoot-device-enrollment-in-intune)

#### Platform Blocked

If platform restrictions block a device type, enrollment fails. Review **Device platform restrictions** and confirm the affected user or group is assigned the correct policy. [8](https://learn.microsoft.com/en-us/intune/device-enrollment/create-platform-restrictions)[13](https://learn.microsoft.com/en-us/intune/device-enrollment/monitor-reports)

#### Missing License

If the user does not have an Intune license, user-based enrollment can fail. Confirm license assignment before retesting enrollment. [3](https://learn.microsoft.com/en-us/intune/device-enrollment/windows/enable-automatic-mdm)[14](https://learn.microsoft.com/en-us/troubleshoot/mem/intune/device-enrollment/troubleshoot-device-enrollment-in-intune)

#### Existing MDM Provider

If the device is already managed by another MDM provider, remove the previous MDM profile or factory reset the device before enrolling in Intune. [2](https://learn.microsoft.com/en-us/intune/device-enrollment/enroll-devices)[7](https://learn.microsoft.com/en-us/intune/device-enrollment/android/guide)

#### iOS/iPadOS ADE Company Portal Issue

For ADE scenarios, deploy Company Portal through Intune rather than the App Store to ensure compatibility and app availability. [5](https://learn.microsoft.com/en-us/intune/device-enrollment/apple/setup-automated-ios)

---

## Tools and Resources Used

The source material and related references mention or rely on the following tools, portals, libraries, and services:

- Microsoft Intune admin center: `https://intune.microsoft.com`
- Microsoft Intune / Microsoft Endpoint Manager
- Microsoft Entra ID
- Microsoft Entra ID P1 or P2
- Microsoft 365 admin center
- Intune Company Portal app
- Apple Business Manager
- Apple School Manager
- Apple Automated Device Enrollment
- Apple MDM Push Certificate
- Apple Configurator
- Managed Google Play
- Android Enterprise
- Windows Autopilot
- Windows Configuration Designer
- Configuration Manager co-management
- Group Policy for Windows automatic enrollment
- Enrollment failures report
- Troubleshooting + support page
- Device enrollment report
- Microsoft self-help diagnostics for Intune enrollment

### Source Links

- Microsoft Learn training unit: Manage mobile devices with Intune  
  `https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/2-manage-mobile-devices-intune`

- Microsoft Learn training unit: Enable mobile device management  
  `https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/3-enable-mobile-device-management`

- Microsoft Learn training unit: Explain considerations for device enrollment  
  `https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/4-explain-considerations-device-enrollment`

- Microsoft Learn training unit: Manage corporate enrollment policy  
  `https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/5-manage-corporate-enrollment-policy`

- Microsoft Learn training unit: Enroll Windows devices in Intune  
  `https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/6-enroll-windows-devices-intune`

- Microsoft Learn training unit: Enroll Android devices in Intune  
  `https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/7-enroll-android-devices-intune`

- Microsoft Learn training unit: Enroll iOS devices in Intune  
  `https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/8-enroll-ios-devices-intune`

- Microsoft Learn training unit: Explore Device Enrollment Manager  
  `https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/9-explore-device-enrollment-manager`

- Microsoft Learn training unit: Monitor device enrollment  
  `https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/10-monitor-device-enrollment`

- Microsoft Learn training unit: Manage devices remotely  
  `https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-intune/11-manage-devices-remotely`

- Device enrollment guide for Microsoft Intune  
  `https://learn.microsoft.com/en-us/intune/device-enrollment/guide`

- Enroll devices in Microsoft Intune  
  `https://learn.microsoft.com/en-us/intune/device-enrollment/enroll-devices`

- Enable MDM automatic enrollment for Windows  
  `https://learn.microsoft.com/en-us/intune/device-enrollment/windows/enable-automatic-mdm`

- Create device platform restrictions  
  `https://learn.microsoft.com/en-us/intune/device-enrollment/create-platform-restrictions`

- Overview of enrollment restrictions  
  `https://learn.microsoft.com/en-us/intune/device-enrollment/restrictions`

- Windows device enrollment guide  
  `https://learn.microsoft.com/en-us/intune/device-enrollment/windows/guide`

- Android device enrollment guide  
  `https://learn.microsoft.com/en-us/intune/device-enrollment/android/guide`

- iOS/iPadOS device enrollment guide  
  `https://learn.microsoft.com/en-us/intune/device-enrollment/apple/guide-ios-ipados`

- View enrollment reports  
  `https://learn.microsoft.com/en-us/intune/device-enrollment/monitor-reports`

- Troubleshoot device enrollment in Intune  
  `https://learn.microsoft.com/en-us/troubleshoot/mem/intune/device-enrollment/troubleshoot-device-enrollment-in-intune`

---

## Source Notes

- The supplied Microsoft Learn training links could not be extracted verbatim in this environment, so this README uses a faithful adaptation based on the linked module topics and related Microsoft Learn documentation.
- The source pages imply platform-specific prerequisites but do not provide every tenant-specific value, licensing SKU, or organization policy decision.
- Enrollment methods and supported platforms can change over time. Always validate current prerequisites in Microsoft Learn before production rollout.
- Some remote actions are platform-specific and may not be available for all device types or ownership models.
- Android device administrator is deprecated for devices with Google Mobile Services, so Android Enterprise should be preferred where supported.
- For iOS/iPadOS Automated Device Enrollment, Company Portal should be deployed through Intune rather than installed manually from the App Store.

---

## Confirmed and Worked-On Scenarios

This section provides practical scenarios that can be applied from the information above, with detailed solutions, steps, and applicable sources.

### Scenario 1: Enable Windows Automatic Enrollment for a Pilot Group

#### Goal

Automatically enroll Windows devices into Intune when selected users join or register their devices with Microsoft Entra ID.

#### Applicable Sources

- Enable MDM automatic enrollment for Windows
- Set up automatic enrollment in Intune
- Windows device enrollment guide

#### Detailed Steps

1. Confirm the pilot users have Intune licenses.
2. Confirm Microsoft Entra ID P1 or P2 is available.
3. Create a Microsoft Entra security group for pilot users.
4. Sign in to the Intune admin center.
5. Go to **Devices > Enrollment > Windows > Automatic Enrollment**.
6. Set **MDM user scope** to **Some**.
7. Select the pilot user group.
8. Set WIP user scope to **None** unless Windows Information Protection is intentionally required.
9. Save the configuration.
10. Test with one Windows device.
11. Confirm the device appears in **Devices > All devices**.
12. Review enrollment status and compliance policy assignment.

#### Validation

- The device appears in Intune.
- The device has an MDM certificate.
- Assigned configuration and compliance policies apply.
- No enrollment failure appears in **Devices > Monitor > Enrollment failures**.

---

### Scenario 2: Block Personal Android Devices for a Department

#### Goal

Prevent a selected group, such as Sales, from enrolling personally owned Android devices.

#### Applicable Sources

- Overview of enrollment restrictions
- Create device platform restrictions
- Android device enrollment guide

#### Detailed Steps

1. Sign in to the Intune admin center.
2. Go to **Devices > Enrollment**.
3. Select **Device platform restriction**.
4. Choose the Android restrictions tab.
5. Create a new restriction.
6. Name the restriction clearly, for example `Block Personal Android - Sales`.
7. Configure personally owned Android Enterprise work profile enrollment as blocked.
8. Assign the restriction to the Sales group.
9. Review and create the policy.
10. Ensure the policy priority is higher than the default policy.
11. Ask a test user in the Sales group to attempt enrollment from a personal Android device.
12. Confirm enrollment is blocked.
13. Review the enrollment failure report for validation.

#### Validation

- Personal Android enrollment is blocked for targeted users.
- Corporate Android enrollment methods remain available if configured separately.
- Enrollment failure details show the applicable restriction.

---

### Scenario 3: Increase Device Enrollment Limit for a User Group

#### Goal

Allow a group of users to enroll more devices, such as up to 10 devices per user.

#### Applicable Sources

- Overview of enrollment restrictions
- Device enrollment guide for Microsoft Intune

#### Detailed Steps

1. Sign in to the Intune admin center.
2. Go to **Devices > Enrollment**.
3. Select **Device limit restrictions**.
4. Create a new restriction.
5. Name the policy, for example `Device Limit - 10 Devices`.
6. Set the device limit to `10`.
7. Assign the restriction to the required user group.
8. Review and create the policy.
9. Confirm priority order if multiple restrictions exist.
10. Test enrollment with a user in scope.

#### Validation

- Targeted users can enroll up to the configured limit.
- Users outside the group remain governed by the default or assigned policy.
- Enrollment failures do not show device cap reached unless the new limit is exceeded.

---

### Scenario 4: Enroll Corporate-Owned Android Fully Managed Devices

#### Goal

Enroll Android devices that are owned by the organization and intended for business-only use.

#### Applicable Sources

- Android device enrollment guide
- Android Enterprise corporate-owned device documentation

#### Detailed Steps

1. Confirm Intune is set as MDM authority.
2. Connect Intune to Managed Google Play.
3. Confirm devices support Android Enterprise.
4. Go to **Devices > Android > Enrollment**.
5. Create a corporate-owned fully managed enrollment profile.
6. Generate the enrollment token or QR code.
7. Factory reset the Android device if required.
8. Start device setup.
9. Use the supported enrollment method, such as QR code scanning.
10. Complete sign-in and enrollment.
11. Assign compliance, configuration, and app policies.
12. Confirm the device appears in Intune.

#### Validation

- Device ownership is corporate.
- Device is enrolled as Android Enterprise fully managed.
- Required apps and policies deploy.
- Device compliance is evaluated.

---

### Scenario 5: Enroll iOS/iPadOS Devices with Automated Device Enrollment

#### Goal

Enroll organization-owned Apple devices using Apple Business Manager or Apple School Manager and Intune.

#### Applicable Sources

- iOS/iPadOS device enrollment guide
- Set up automated device enrollment for iOS/iPadOS

#### Detailed Steps

1. Confirm access to Apple Business Manager or Apple School Manager.
2. Configure an Apple MDM push certificate in Intune.
3. Create or upload the ADE token in Intune.
4. Sync Apple devices into Intune.
5. Create an iOS/iPadOS enrollment profile.
6. Assign the enrollment profile to devices.
7. Deploy Company Portal as a required VPP app using device licensing.
8. Wipe or reset the Apple device if required.
9. Start the device setup experience.
10. Complete Remote Management enrollment.
11. Confirm the device appears in Intune.

#### Validation

- Device is supervised.
- ADE profile is applied.
- Company Portal is deployed through Intune.
- Configuration profiles and apps are assigned successfully.

---

### Scenario 6: Troubleshoot a Failed Enrollment

#### Goal

Identify why a user cannot enroll a device into Intune.

#### Applicable Sources

- View enrollment reports
- Troubleshoot device enrollment in Intune

#### Detailed Steps

1. Confirm user licensing.
2. Confirm the device platform is supported.
3. Confirm enrollment restrictions do not block the platform or ownership type.
4. Check whether the user reached the enrollment device limit.
5. Confirm MDM authority and automatic enrollment settings.
6. Go to **Devices > Monitor > Enrollment failures**.
7. Search for the affected user or review all failures.
8. Open the failure details.
9. Review the reason, OS, OS version, enrollment method, and recommended remediation.
10. Go to **Troubleshooting + support**.
11. Search for the user.
12. Review assignments, device records, app protection status, and enrollment failures.
13. Apply recommended fixes.
14. Retry enrollment.
15. Rerun diagnostics if needed.

#### Validation

- Enrollment failure no longer appears after remediation.
- Device appears in Intune.
- User receives expected apps and policies.

---

### Scenario 7: Remotely Retire or Wipe a Device

#### Goal

Remove corporate data or fully reset a device remotely.

#### Applicable Sources

- Microsoft Intune remote device actions
- Microsoft Intune device management documentation

#### Detailed Steps

1. Sign in to the Intune admin center.
2. Go to **Devices > All devices**.
3. Select the target device.
4. Review device ownership and user assignment.
5. Choose the correct remote action:
   - Use **Retire** to remove company data while preserving personal data where supported.
   - Use **Wipe** to factory reset the device and remove all data.
6. Confirm the action.
7. Monitor device action status.
8. Confirm the device checks in and completes the action.

#### Validation

- Retired devices no longer have organizational data.
- Wiped devices are factory reset.
- Device status updates in Intune after check-in.

---

## GitHub README Usage

This content is ready to paste into a GitHub `README.md` file. Review tenant-specific names, groups, policy names, and URLs before publishing or using it for production documentation.
