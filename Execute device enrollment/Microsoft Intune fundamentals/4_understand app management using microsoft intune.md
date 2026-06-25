# App Management Using Microsoft Endpoint Manager (Intune)

> **Platform:** Microsoft Intune / Microsoft Endpoint Manager

---

## Overview

Microsoft Endpoint Manager (which includes **Microsoft Intune** and **Microsoft Configuration Manager**) provides a comprehensive platform for managing apps across your organization's devices. This documentation covers the full app management lifecycle — from adding apps and configuring them, to protecting organizational data and retiring outdated apps.

Whether devices are corporate-owned and enrolled in MDM, or personal (BYOD) and unenrolled, Intune provides policy-based controls that keep organizational data secure.

---

## The App Management Lifecycle

The app management lifecycle begins when you add an app and progresses through additional phases until the app is removed. The phases are:

```
Add → Deploy → Configure → Protect → Retire
```

### 1. Add

The first step is to add apps to Intune or create them in Configuration Manager.

**Intune supports the following app types:**
- Apps written in-house (line-of-business / LOB apps)
- Apps from the store (iOS App Store, Google Play, Microsoft Store)
- Built-in apps
- Web apps (links)

**Configuration Manager:**
- You create the app and define requirements that must be met before the app is installed.

> All app types follow the same basic add procedure regardless of source.

---

### 2. Deploy

After adding an app to Intune or Configuration Manager, you deploy it.

**In Intune:**
- Assign the app to users and/or devices that you manage.
- Monitor deployment success from within the **Azure portal**.
- For volume-licensed apps (e.g., Apple Business Manager, Microsoft Store for Business), Intune synchronizes with the store to deploy and track license usage from the Intune admin console.

**In Configuration Manager:**
- Deploy the app to a **device collection** based on Windows OS requirements you select.

---

### 3. Configure

Both Intune and Configuration Manager provide methods to configure app installation and updates.

**Configurable app behaviors include:**

| Configuration Type | Description |
|---|---|
| iOS/iPadOS App Configuration Policies | Supply settings for compatible iOS/iPadOS apps at runtime (e.g., server names, branding) |
| Managed Browser Policies | Configure settings for **Microsoft Edge**, restrict website access |
| Microsoft Outlook Settings | Set specific settings when deploying Outlook |
| User vs. System Context | Choose whether the app installs in User or System context |
| Installation Dependencies & Detection Rules | Rules to detect if an app is installed, needs updating, or has dependencies |

---

### 4. Protect

Intune provides multiple layers of protection for app data.

**Main protection methods:**

- **Conditional Access** — Controls access to email and other services based on specified conditions (e.g., device type, compliance with a device policy).
- **App Protection Policies (APP)** — Work at the individual app level to protect company data:
  - Restrict copying data between unmanaged and managed apps.
  - Prevent apps from running on jailbroken or rooted devices.

---

### 5. Retire

When apps become outdated, they must be removed.

**In Intune:**
- Assign an **uninstall intent** to a group of users or devices.

**In Configuration Manager**, follow these steps in order:
1. Retire the app.
2. Delete all deployments of the app.
3. Remove all references to the app from other deployments.
4. Delete all of the app's revisions.

---

## App Configuration Policies

App configuration policies eliminate app setup problems by providing configuration settings automatically before end users run a specific app so end users do not need to take action themselves.

### What App Configuration Policies Do

- Apply to both **iOS/iPadOS** and **Android** apps.
- Deliver configuration settings when the app is run for the first time.
- Provide consistency across the enterprise and reduce helpdesk calls.
- Accelerate adoption of new apps.

### Configuration Settings Examples

An app configuration setting may require you to specify any of the following:

```
- A custom port number
- Language settings
- Security settings
- Branding settings (e.g., a company logo)
```

> **Important:** Configuration parameters are defined by the app developer. Always review vendor documentation to see what configurations an app supports. For some applications, Intune automatically populates the available configuration settings.

---

## App Protection Policies

App Protection Policies (APP) are one of Intune's primary mechanisms for mobile app security.

### What App Protection Policies Do

- Use **Microsoft Entra identity** to isolate organization data from personal data.
- Restrict actions users can take with organizational data (copy-paste, save, view).
- Can be deployed on:
  - Devices enrolled in **Intune**
  - Devices enrolled in **another MDM service**
  - Devices **not enrolled** in any MDM service

> **Note:** App protection policies are designed to apply uniformly across a group of apps — for example, applying a policy across all Office mobile apps.

---

### App Protection with Enrollment (MDM)

When used alongside an MDM service, APP adds an extra layer of protection:

- A user signs into a device with **organization credentials** → organizational data access → APP controls how that data is saved and shared.
- When the same user signs in with their **personal identity** → those same protections are NOT applied.
- Result: IT controls organizational data; end users retain control and privacy over personal data.

**Layers of protection (MDM + APP):**

```
┌──────────────────────────────────────┐
│           MDM Service Layer          │  ← Device-level management
│  ┌────────────────────────────────┐  │
│  │  App Protection Policy Layer   │  │  ← App-level data protection
│  └────────────────────────────────┘  │
└──────────────────────────────────────┘
```

---

### App Protection Without Enrollment

Organizations can use APP **with and without MDM at the same time**.

**Example scenario:**

| Device | Management Status | Protection |
|---|---|---|
| Company-issued tablet | Enrolled in MDM | MDM + App Protection Policies |
| Employee's personal phone | Not enrolled | App Protection Policies only |

This is especially relevant for BYOD (Bring Your Own Device) environments.

---

## Protected Apps

Intune supports several **Microsoft** and **partner** apps that natively incorporate mobile application management capabilities.

### Capabilities of Protected Apps

Protected apps allow you to:

- Restrict **copy-and-paste** and **save-as** functions.
- Configure web links to open inside the **secure Microsoft browser**.
- Enable **multi-identity use** and app-level **Conditional Access**.
- Apply **data loss prevention (DLP) policies** without managing the user's device.
- Enable app protection **without requiring enrollment**.
- Enable app protection on devices managed with **third-party enterprise mobility management (EMM) tools**.

> See the full list of supported apps: [Microsoft Intune protected apps](https://learn.microsoft.com/en-us/mem/intune/apps/apps-supported-intune-apps)

---

### Creating Protected Apps Using Tools

If you are an app developer, you can build Intune-integrated protected apps using:

| Tool | Platform | Purpose |
|---|---|---|
| **Intune App SDK** | iOS and Android | Natively integrates MAM capabilities into the app |
| **App Wrapping Tool** | iOS and Android | Wraps an existing app to add Intune management without code changes |

**Example — Outlook for iOS and Android:**

Outlook is a protected app that can be fully managed with Intune. It supports:
- App protection policies
- App configuration policies
- **Microsoft Entra Conditional Access** — ensures users can only access work or school content using Outlook for iOS and Android.

---

## Data Protection Framework

As more organizations implement mobile device strategies, protecting against **data leakage** is critical. Intune's App Protection Policies (APP) ensure an organization's data remains contained in a managed app — regardless of whether the device is enrolled.

Intune provides a **three-level data protection framework** where each level builds on the previous one.

---

### Level 1 — Enterprise Basic Data Protection

**Target:** Entry-level configuration, suitable for most organizations just beginning with APP.

**Key controls:**
- Apps are protected with a **PIN** and are **encrypted**.
- Performs **selective wipe** operations.
- For Android devices: validates **Android device attestation**.
- Provides data protection control similar to Exchange Online mailbox policies.

---

### Level 2 — Enterprise Enhanced Data Protection

**Target:** Most mobile users accessing work or school data.

**Key controls:**
- Introduces **App Protection Policies (APP)**.
- Adds **data leakage prevention mechanisms**.
- Enforces **minimum OS requirements**.

---

### Level 3 — Enterprise High Data Protection

**Target:** Users accessing **high-risk data** (e.g., healthcare, legal, financial).

**Key controls:**
- Advanced **data protection mechanisms**.
- **Enhanced PIN configuration**.
- **APP Mobile Threat Defense** integration.

---

### Framework Summary

| Level | Name | Target Users | Key Features |
|---|---|---|---|
| **1** | Enterprise Basic Data Protection | All users, entry-level | PIN, encryption, selective wipe, Android attestation |
| **2** | Enterprise Enhanced Data Protection | Most mobile users | APP policies, DLP, min OS requirements |
| **3** | Enterprise High Data Protection | High-risk data users | Advanced mechanisms, enhanced PIN, Mobile Threat Defense |

> For full details: [Data protection framework using app protection policies](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-framework)

---

## Tools and Resources Used

| Tool / Resource | Description | Link |
|---|---|---|
| Microsoft Intune | Cloud-based MDM and MAM solution | [Intune Overview](https://learn.microsoft.com/en-us/mem/intune/fundamentals/what-is-intune) |
| Microsoft Configuration Manager | On-premises device and app management | [ConfigMgr Docs](https://learn.microsoft.com/en-us/mem/configmgr/) |
| Microsoft Entra ID (Azure AD) | Identity platform used for Conditional Access and identity isolation | [Entra ID](https://learn.microsoft.com/en-us/entra/identity/) |
| Intune App SDK | SDK for developers to embed MAM capabilities into iOS/Android apps | [Intune App SDK](https://learn.microsoft.com/en-us/mem/intune/developer/app-sdk) |
| App Wrapping Tool | Wraps existing iOS/Android apps with Intune MAM support (no code changes) | [App Wrapping Tool](https://learn.microsoft.com/en-us/mem/intune/developer/apps-prepare-mobile-application-management) |
| App Configuration Policies | Policies to pre-configure app settings before first run | [App Config Policies](https://learn.microsoft.com/en-us/mem/intune/apps/app-configuration-policies-overview) |
| App Protection Policies | Policies to protect organizational data at the app level | [App Protection Policies](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy) |
| Data Protection Framework | Three-level taxonomy for mobile app management hardening | [Data Protection Framework](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-framework) |
| Microsoft Intune Protected Apps List | Catalog of Microsoft and partner apps with built-in MAM support | [Protected Apps List](https://learn.microsoft.com/en-us/mem/intune/apps/apps-supported-intune-apps) |
| Microsoft Edge (Managed Browser) | Intune-managed browser for controlled web access | [Managed Browser](https://learn.microsoft.com/en-us/mem/intune/apps/manage-microsoft-edge) |
| Outlook for iOS and Android | Example protected app with full Intune policy support | [Outlook + Intune](https://learn.microsoft.com/en-us/exchange/clients-and-mobile-in-exchange-online/outlook-for-ios-and-android/outlook-for-ios-and-android-in-the-government-cloud) |
| Plan for Configuration Manager App Management | Guidance for planning and configuring apps in ConfigMgr | [ConfigMgr App Planning](https://learn.microsoft.com/en-us/mem/configmgr/apps/plan-design/plan-for-and-configure-application-management) |

---

## Confirmed Scenarios and Detailed Solutions

The following are real-world scenarios derived from the content above, including step-by-step solutions and applicable references.

---

### Scenario 1: Deploying a Line-of-Business (LOB) App to Corporate Devices

**Problem:** Your organization has an internally built app that needs to be distributed to all corporate-owned iOS devices.

**Solution:**

1. In the [Intune admin center](https://intune.microsoft.com), navigate to **Apps > All Apps > Add**.
2. Select **Line-of-business app** as the app type.
3. Upload the `.ipa` file (iOS) or `.apk` file (Android).
4. Fill in the app information: name, description, publisher, minimum OS version.
5. Under **Assignments**, assign the app to the relevant **device group** or **user group** with intent **Required**.
6. Monitor deployment status under **Apps > Monitor > App install status**.

**References:**
- [Add an iOS/iPadOS LOB app to Intune](https://learn.microsoft.com/en-us/mem/intune/apps/lob-apps-ios)
- [Monitor app information and assignments](https://learn.microsoft.com/en-us/mem/intune/apps/apps-monitor)

---

### Scenario 2: Pre-Configuring Outlook for New Employees Using App Configuration Policies

**Problem:** New employees are getting helpdesk calls because they cannot configure Outlook correctly on their mobile devices.

**Solution:**

1. In the Intune admin center, go to **Apps > App configuration policies > Add > Managed devices**.
2. Set the platform to **iOS/iPadOS** or **Android Enterprise**.
3. Select **Microsoft Outlook** as the target app.
4. Add configuration keys such as:
   ```
   Key:   com.microsoft.outlook.EmailProfile.EmailAddress
   Value: {{userprincipalname}}
   
   Key:   com.microsoft.outlook.EmailProfile.EmailAccountName
   Value: {{username}}
   ```
5. Assign the policy to the target user group.
6. On first launch, Outlook will auto-configure without user input.

**References:**
- [App configuration policies for Microsoft Intune](https://learn.microsoft.com/en-us/mem/intune/apps/app-configuration-policies-overview)
- [Configure Outlook for iOS and Android with Intune](https://learn.microsoft.com/en-us/exchange/clients-and-mobile-in-exchange-online/outlook-for-ios-and-android/outlook-for-ios-and-android-configuration-with-microsoft-intune)

---

### Scenario 3: Protecting Corporate Data on BYOD (Personal) Devices Without Enrollment

**Problem:** Employees use personal phones to access company email and OneDrive. IT needs to protect that data without enrolling those devices.

**Solution:**

1. In the Intune admin center, go to **Apps > App protection policies > Create policy**.
2. Select the platform (iOS/iPadOS or Android).
3. Target apps: select **Microsoft Office 365 apps** (Outlook, Teams, OneDrive, etc.).
4. Configure data protection settings:
   - **Prevent backup** of org data to iCloud/Google Backup.
   - Set **Restrict cut, copy, and paste** between managed and unmanaged apps.
   - Enable **Encrypt org data**.
5. Set Access requirements (PIN, biometric).
6. Set Conditional launch (e.g., block access if device is jailbroken/rooted).
7. Assign the policy to a **user group** (not a device group — policies follow the user identity).

**References:**
- [App protection policies overview](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy)
- [How to create and assign app protection policies](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policies)

---

### Scenario 4: Implementing the Data Protection Framework — Starting with Level 1

**Problem:** Your organization is new to Intune APP and wants to start implementing baseline data protection.

**Solution (Level 1 — Enterprise Basic):**

1. Create an App Protection Policy targeting all managed apps.
2. Under **Data protection**, configure:
   - **Backup org data**: Block
   - **Send org data to other apps**: Policy managed apps only
   - **Save copies of org data**: Block
3. Under **Access requirements**, configure:
   - **PIN for access**: Require
   - **PIN type**: Numeric (minimum 4 digits)
4. Under **Conditional launch**, configure:
   - **Jailbroken/rooted devices**: Block access
5. For Android, enable **SafetyNet device attestation** (validates Android device integrity).
6. Assign to all users.

**When ready to advance to Level 2**, add:
- Minimum OS version requirements.
- Data leakage prevention settings (block screenshots, require managed keyboard on Android).

**When ready to advance to Level 3**, add:
- Mobile Threat Defense integration (e.g., Microsoft Defender for Endpoint).
- Enhanced PIN (e.g., alphanumeric, biometric override settings).

**References:**
- [Data protection framework using app protection policies](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-framework)
- [Android app protection policy settings](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy-settings-android)
- [iOS/iPadOS app protection policy settings](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy-settings-ios)

---

### Scenario 5: Wrapping an In-House Android App with the Intune App Wrapping Tool

**Problem:** Your organization has a custom Android app built by an internal team. The team does not have time to integrate the Intune SDK. You need to add MAM support without changing the source code.

**Solution:**

1. Download the [Intune App Wrapping Tool for Android](https://github.com/Microsoft/intune-app-wrappingtools).
2. On a Windows machine, run the tool from the command line:
   ```bash
   java -jar IntuneMAMPackager/Contents/MacOS/IntuneMAMPackager \
     -i /path/to/original.apk \
     -o /path/to/wrapped.apk \
     -p /path/to/signing.keystore \
     -a signing_alias \
     -k keystore_password \
     -g key_password
   ```
3. Upload the wrapped `.apk` to Intune as a LOB app.
4. Create and assign an **App Protection Policy** targeting the wrapped app.
5. Employees accessing the app will now be subject to Intune MAM controls.

> **Note:** The App Wrapping Tool does not work on apps from public stores; it is for in-house apps only.

**References:**
- [Prepare Android apps for app protection policies with the Intune App Wrapping Tool](https://learn.microsoft.com/en-us/mem/intune/developer/app-wrapper-prepare-android)
- [Intune App Wrapping Tool for iOS](https://learn.microsoft.com/en-us/mem/intune/developer/app-wrapper-prepare-ios)

---

### Scenario 6: Retiring and Removing an App Across the Organization

**Problem:** A third-party app is no longer licensed. It must be removed from all managed devices.

**Solution — In Intune:**

1. Go to **Apps > All Apps**, locate the app.
2. Under **Assignments**, change the assignment intent from **Required** or **Available** to **Uninstall**.
3. Assign the **Uninstall** intent to the target user or device group.
4. Intune will push the uninstall command on the next device check-in.

**Solution — In Configuration Manager:**

1. Open the Configuration Manager console.
2. Navigate to **Software Library > Application Management > Applications**.
3. Right-click the app → **Retire**.
4. Delete all **deployments** of the app.
5. Remove all **references** to the app from other deployments.
6. Delete all **revisions** of the app.
7. Finally, delete the application object.

**References:**
- [Uninstall an app using Intune](https://learn.microsoft.com/en-us/mem/intune/apps/apps-add#uninstalling-apps)
- [Delete an application in Configuration Manager](https://learn.microsoft.com/en-us/mem/configmgr/apps/deploy-use/delete-retire-application)

---


*Documentation sourced from [Microsoft Learn — App Management Using Microsoft Endpoint Manager](https://learn.microsoft.com/en-us/training/modules/app-management-using-microsoft-endpoint-manager/). © Microsoft 2024.*
