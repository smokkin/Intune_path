# Benefits of Microsoft Endpoint Manager (Microsoft Intune)

## 1. Intelligent and Unified Endpoint Security
 
> **Source:** [Unit 2 – Secure and Intelligent](https://learn.microsoft.com/en-us/training/modules/benefits-microsoft-endpoint-manager/2-secure-and-intelligent)

Microsoft Intune provides **native integration with cloud-powered security controls** and **risk-based Conditional Access** for apps and data, delivering intelligent and unified endpoint security.

---

### Intelligent Security

Intune enables a unique set of capabilities to simplify endpoint security management:

| Feature | Description |
|---|---|
| **Security Baselines** | Preconfigured groups of Windows settings that help apply a known group of settings and recommended default values. Creating a security baseline profile in Intune creates a template consisting of multiple *device configuration* profiles. |
| **BitLocker Management** | BitLocker-based modern encryption management. BitLocker integrates with Windows 10/11 OS to address data theft or exposure from lost, stolen, or decommissioned computers. |
| **Advanced Threat Protection** | Microsoft Defender Advanced Threat Protection (Microsoft Defender ATP), along with its partners for iOS and Android, provides a comprehensive Mobile Threat Defense solution. |
| **Secure Score** | Helps you assess your workload security posture by recommending a prioritized list of security vulnerabilities for remediation. |
| **Windows Hello for Business** | Passwordless authentication for Windows 10/11. |

---

### Unified Security

You can securely access corporate resources through **continuous assessment** and **intent-based policies** with Conditional Access app control powered by Microsoft Entra ID, natively integrated in Microsoft Intune.

**Unified security management with Microsoft Defender for Intune** enables quick, automated remediation of app vulnerabilities.

Microsoft Intune's cross-platform device controls support a **Zero Trust endpoint strategy**, including:

#### Endpoint Compliance and Risk
- Adapt in real time to changes in device health.
- Use Microsoft 365 cloud to evaluate risks.
- Determine risks calculated based on advanced Microsoft machine learning.

#### Conditional Access
- Define contextual policies at the **user**, **location**, **device**, and **app** levels.
- Evaluate compliance that Microsoft Entra ID enforces to provide Conditional Access.

#### App Protection Policy
- Protect apps and Office 365 data on **unmanaged devices**.

#### Additional Capabilities
- Extend native platform security to meet all use cases.
- Third-party risk and compliance signaling.

> **Diagram Reference:** Cross-platform device controls to help provide trust, and the conditions that must stay within compliance, are visualized in the official Microsoft Learn diagram for this unit.

---

## 2. Streamlined and Flexible Unified Endpoint Management
 
> **Source:** [Unit 3 – Streamlined and Flexible](https://learn.microsoft.com/en-us/training/modules/benefits-microsoft-endpoint-manager/3-streamlined-and-flexible)

Microsoft Intune meets organizations **where they are in their cloud journey**. It lets you secure, deploy, and manage all users, apps, and devices **without disruption to existing processes**, while increasing productivity and collaboration.

---

### Unified Management

Manage your PCs, Macs, and mobile devices in one place:

- **Manage all your endpoints from a single console:** Microsoft Intune provides a single console for management activities.
- **Extend on-premises infrastructure with cloud security:** Use guided deployments to extend on-premises infrastructure.
- **Provide the best Office 365 management experience:** Deliver the best Office experience with security and configuration management, and cloud content optimization.
- **Manage key mobile apps Microsoft Outlook and Edge:** Stay secure with Microsoft Apps (also known as Office 365 Pro Plus) and Microsoft Edge for iOS and Android.

---

### Zero Touch Provisioning

With Microsoft Intune, you can **simplify software updates and provisioning** for all devices. By using **Windows Autopilot**, **Android Enterprise**, **Apple DEP**, and **Samsung Knox Mobile Enrollment**, you can:

- Decrease the cost of your device-image creation workload when provisioning machines.
- Provide self-service provisioning directly by end users.
- Deliver end-user productivity more quickly when provisioning machines.
- Ensure out-of-the-box security for new and updated machines.
- Lower the costs for keeping your device hardware current.

---

## 3. Protecting Data Without Enrollment

> **Source:** [Unit 4 – Protect Without Enrollment](https://learn.microsoft.com/en-us/training/modules/benefits-microsoft-endpoint-manager/4-protect-without-enrollment)

**Mobile Application Management (MAM)** allows you to manage and protect your organization's data within an application. With **MAM without enrollment (MAM-WE)**, a work- or school-related app containing sensitive data can be managed on almost any device, including personal devices in **bring-your-own-device (BYOD)** scenarios.

Intune MAM can manage many productivity apps, such as the **Microsoft Office apps**.

---

### MAM Configurations

Intune MAM supports two configurations:

| Configuration | Description |
|---|---|
| **Intune MDM + MAM** | IT administrators can only manage apps using MAM and app-protection policies on devices that are **enrolled** with Intune mobile device management (MDM). |
| **MAM without device enrollment (MAM-WE)** | IT administrators manage apps using MAM and app protection policies on devices **not enrolled** with Intune MDM. This means Intune can manage apps on devices enrolled with third-party Enterprise Mobility Management (EMM) providers, or not enrolled with any MDM at all. |

> **Key Benefit:** MAM-WE is critical for BYOD environments, allowing organizations to protect corporate data on personal employee devices without requiring full device enrollment.

---

## 4. Increasing User Productivity

> **Source:** [Unit 5 – Increases Productivity](https://learn.microsoft.com/en-us/training/modules/benefits-microsoft-endpoint-manager/5-increases-productivity)

By implementing a management infrastructure, you can provide IT services, apps, protection, and configuration to your end users to **make them more productive**.

**Examples of automated provisioning benefits:**
- End users automatically receive their certificates, WiFi profiles, and VPN settings without manual configuration.
- End users are better protected because they have the correct settings pre-applied.

### Endpoint Analytics

**Endpoint Analytics** aims to improve user productivity and reduce IT support costs by providing insights into how your organization is working and the quality of the experience delivered to end users.

Endpoint Analytics can:
- Identify configuration or hardware issues that might be slowing down devices.
- Provide **recommended actions** to remediate issues proactively.
- Enable changes to be made without disrupting end users or generating a help-desk ticket.

### Microsoft Adoption Score

Endpoint Analytics is also part of the [**Microsoft Adoption Score**](https://learn.microsoft.com/en-us/microsoft-365/admin/adoption/adoption-score). The Adoption Score provides:

- Productivity data
- Insights and recommendations for end users' experience
- Your organization's overall technology experience score

<img width="757" height="454" alt="image" src="https://github.com/user-attachments/assets/95dfd2c3-5fde-4826-ab87-1ee93f26bf05" />

---

## 5. Benefits of Co-Management

> **Source:** [Unit 6 – Benefits of Co-Management](https://learn.microsoft.com/en-us/training/modules/benefits-microsoft-endpoint-manager/6-benefits-of-co-management)

**Co-management** adds new functionality to your existing **Configuration Manager** deployment without changing how you already work. When you enable co-management, you immediately begin benefiting from the cloud, and can apply that value to your existing management infrastructure and processes.

### Immediate Value from Co-Management Enrollment

When you enroll existing Configuration Manager clients in co-management, you gain the following immediate benefits:

- **Conditional Access** with device compliance
- **Intune-based remote actions** (for example: restart, remote control, or factory reset)
- **Centralized visibility** of device health
- **Link users, devices, and apps** with Microsoft Entra ID
- **Modern provisioning** with Windows Autopilot
- **Remote actions**

> **Key Point:** Co-management is a gradual migration path. You do not need to change your existing workflows to start benefiting from cloud features.

---

## 6. Maximizing Your Return on Investment
 
> **Source:** [Unit 7 – Maximizes Investment](https://learn.microsoft.com/en-us/training/modules/benefits-microsoft-endpoint-manager/7-maximizes-investment)

Microsoft Intune helps you **maximize your return on investment** and accelerate time-to-value with fast rollout of services and devices, providing end-to-end integration across the familiar Microsoft stack.

---

### Advanced Endpoint Analytics

You can **proactively improve the user experience** and track your progress against organization and industry baselines with Integrated Endpoint Analytics. Use data-driven change management to maximize the effectiveness of IT and reduce help desk costs.

The following tools and capabilities support this goal:

| Tool / Feature | Purpose |
|---|---|
| **Technology Experience Score** | Track overall technology experience quality |
| **Desktop Analytics** | Analyze device readiness and health |
| **Log Analytics** | Investigate and query endpoint telemetry |
| **Real-Time Advanced Threat Detection** | Detect threats as they occur |
| **Dynamic User Risk Assessment** | Continuously evaluate user risk levels |

**Benefits of Advanced Endpoint Analytics:**
- Proactively maintain device performance and health.
- Benefit from data-driven IT management that shows how endpoint hardware and software performance issues impact end-user productivity.
- Use machine learning for recommended security and configuration settings.

---

### Deep Microsoft 365 Integration

Get the most value from your **Microsoft 365 integrated solution** using the latest cloud features to help protect users' privacy and your organization's data and assets.

By using **role-based administration**, **Graph API**, **PowerShell**, and **Cloud Content Optimization**, you can gain the following benefits:

- **Unified IT administration experience:** Use role-based administration to secure the access needed to administer Configuration Manager.
- **Continuous security improvements for everyone.**
- **Automated app compatibility and software updates** for OS and apps.
- **Secure access to the objects that you manage**, such as collections, deployments, and sites.
- **The capability to extend or build unique, intelligent applications** using Microsoft Graph API, Device Compliance API, and SDK for app protection policies.
- **Sophisticated PowerShell scripts** to easily accomplish complex tasks.
- **Cloud content optimization.**
- **Increased end-user satisfaction** through native platform experiences.

---

## Tools and Resources Used

| Tool / Resource | Category | Reference |
|---|---|---|
| **Microsoft Intune** | Endpoint Management Platform | [Microsoft Intune Docs](https://learn.microsoft.com/en-us/mem/intune/) |
| **Microsoft Endpoint Manager (MEM)** | Unified Admin Center | [MEM Overview](https://learn.microsoft.com/en-us/mem/endpoint-manager-overview) |
| **Microsoft Entra ID** (formerly Azure AD) | Identity & Access Management | [Entra ID Docs](https://learn.microsoft.com/en-us/entra/identity/) |
| **Microsoft Defender ATP** | Advanced Threat Protection | [Defender Docs](https://learn.microsoft.com/en-us/defender-endpoint/) |
| **Configuration Manager (SCCM)** | On-Premises Device Management | [Config Manager Docs](https://learn.microsoft.com/en-us/mem/configmgr/) |
| **Windows Autopilot** | Zero-Touch Device Provisioning | [Autopilot Docs](https://learn.microsoft.com/en-us/autopilot/) |
| **Android Enterprise** | Android Device Enrollment | [Android Enterprise Docs](https://learn.microsoft.com/en-us/mem/intune/enrollment/android-enroll) |
| **Apple DEP (Device Enrollment Program)** | Apple Device Enrollment | [Apple DEP Docs](https://learn.microsoft.com/en-us/mem/intune/enrollment/device-enrollment-program-enroll-ios) |
| **Samsung Knox Mobile Enrollment** | Samsung Device Enrollment | [Knox Enrollment Docs](https://learn.microsoft.com/en-us/mem/intune/enrollment/android-samsung-knox-mobile-enroll) |
| **BitLocker** | Windows Disk Encryption | [BitLocker Docs](https://learn.microsoft.com/en-us/windows/security/operating-system-security/data-protection/bitlocker/) |
| **Windows Hello for Business** | Passwordless Authentication | [Windows Hello Docs](https://learn.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/) |
| **Security Baselines** | Policy Configuration Templates | [Security Baselines Docs](https://learn.microsoft.com/en-us/mem/intune/protect/security-baselines) |
| **Endpoint Analytics** | Productivity & Performance Insights | [Endpoint Analytics Docs](https://learn.microsoft.com/en-us/mem/analytics/) |
| **Microsoft Adoption Score** | Org-Wide Productivity Tracking | [Adoption Score Docs](https://learn.microsoft.com/en-us/microsoft-365/admin/adoption/adoption-score) |
| **Microsoft Graph API** | Programmatic Access & Automation | [Graph API Docs](https://learn.microsoft.com/en-us/graph/) |
| **PowerShell** | Scripting & Automation | [PowerShell Docs](https://learn.microsoft.com/en-us/powershell/) |
| **Desktop Analytics** | Device Readiness & Health Analysis | [Desktop Analytics Docs](https://learn.microsoft.com/en-us/mem/configmgr/desktop-analytics/overview) |
| **Log Analytics** | Telemetry Query & Investigation | [Log Analytics Docs](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/log-analytics-overview) |
| **Conditional Access** | Context-Based Access Policies | [Conditional Access Docs](https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview) |
| **MAM / MAM-WE** | Mobile Application Management | [MAM Docs](https://learn.microsoft.com/en-us/mem/intune/apps/app-management) |
| **App Protection Policies** | Data Leakage Prevention | [App Protection Docs](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy) |
| **Microsoft 365 Apps (Office 365 Pro Plus)** | Productivity Suite | [M365 Apps Docs](https://learn.microsoft.com/en-us/deployoffice/) |
| **Microsoft Edge (iOS/Android)** | Secure Mobile Browser | [Edge Docs](https://learn.microsoft.com/en-us/deployedge/) |
| **Device Compliance API** | Compliance Automation | [Compliance API Docs](https://learn.microsoft.com/en-us/graph/api/resources/intune-deviceconfig-devicecompliancepolicy) |

---

## Confirmed Scenarios and Detailed Solutions

The following scenarios are derived from the content in these modules and reflect real-world applications. Each includes a detailed solution and applicable sources.

---

### Scenario 1: Enforcing Security Policies on BYOD Devices Without Full Device Enrollment

**Problem:** An organization wants to protect corporate Microsoft Office 365 data on employee-owned (BYOD) phones without requiring full MDM enrollment, which employees may resist for privacy reasons.

**Solution:** Deploy **MAM-WE (Mobile Application Management Without Enrollment)**.

**Detailed Steps:**
1. In the **Microsoft Intune admin center** (`https://intune.microsoft.com`), navigate to **Apps > App protection policies**.
2. Select **Create policy** and choose the platform (iOS/iPadOS or Android).
3. Configure the policy:
   - Set **Data transfer** restrictions (block copy/paste to personal apps).
   - Enable **PIN requirements** for app access.
   - Enable **Require corporate credentials** for access.
   - Set **Conditional launch** settings (block jailbroken/rooted devices).
4. Assign the policy to the relevant user groups.
5. No device enrollment is required — the policy applies at the **app level**.

**Outcome:** Corporate data in Outlook, Teams, and other M365 apps is protected. Employees keep full personal privacy on their own devices.

**Applicable Sources:**
- [App protection policies in Intune](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy)
- [MAM without enrollment](https://learn.microsoft.com/en-us/mem/intune/apps/android-deployment-scenarios-app-protection-work-profiles)
- [Unit 4 – Protect Without Enrollment](https://learn.microsoft.com/en-us/training/modules/benefits-microsoft-endpoint-manager/4-protect-without-enrollment)

---

### Scenario 2: Blocking Non-Compliant Devices from Accessing Corporate Email

**Problem:** An organization needs to ensure that only compliant, managed devices can access corporate Exchange Online email. Devices with outdated OS versions or missing encryption should be blocked automatically.

**Solution:** Use **Intune Device Compliance Policies** combined with **Conditional Access** in Microsoft Entra ID.

**Detailed Steps:**
1. In **Intune admin center**, go to **Devices > Compliance policies**.
2. Create a compliance policy for Windows 10/11, iOS, and/or Android:
   - Require **BitLocker** (Windows) or device encryption (mobile).
   - Set **minimum OS version** requirements.
   - Require a **device PIN/password**.
3. Navigate to **Microsoft Entra ID > Security > Conditional Access**.
4. Create a new Conditional Access policy:
   - **Assignments:** Target all users or specific groups.
   - **Cloud apps:** Select **Office 365** or **Exchange Online**.
   - **Conditions:** Set device platforms.
   - **Grant:** Select "Require device to be marked as compliant."
5. Enable the policy in **Report-only** mode first to review impact, then set to **On**.

**Outcome:** Non-compliant devices are automatically blocked from email. Users are shown a remediation screen.

**Applicable Sources:**
- [Device compliance policies in Intune](https://learn.microsoft.com/en-us/mem/intune/protect/device-compliance-get-started)
- [Conditional Access with Intune](https://learn.microsoft.com/en-us/mem/intune/protect/conditional-access)
- [Unit 2 – Secure and Intelligent](https://learn.microsoft.com/en-us/training/modules/benefits-microsoft-endpoint-manager/2-secure-and-intelligent)

---

### Scenario 3: Zero-Touch Provisioning of New Corporate Windows Laptops

**Problem:** An organization purchases 200 new Windows 11 laptops. IT wants to ship them directly to remote employees without imaging or touching the devices. Devices should automatically configure themselves upon first boot.

**Solution:** Use **Windows Autopilot** with **Intune auto-enrollment**.

**Detailed Steps:**
1. Have the hardware vendor register device hardware IDs with Windows Autopilot (or register them yourself via the **Intune admin center > Devices > Windows > Windows enrollment > Devices**).
2. Create an **Autopilot deployment profile** in Intune:
   - Deployment mode: **User-driven** or **Self-deploying**.
   - Set **Out-of-box experience (OOBE)** settings (skip privacy, EULA, account setup prompts).
3. Create and assign a **Device Configuration profile** with WiFi, VPN, certificates, and apps.
4. Ship devices directly to users.
5. User powers on laptop → OOBE launches → device connects to internet → Azure AD/Entra ID join occurs automatically → Intune policies and apps are pushed.

**Outcome:** Devices are fully configured with corporate settings before users begin work, with zero IT physical touch.

**Applicable Sources:**
- [Windows Autopilot overview](https://learn.microsoft.com/en-us/autopilot/windows-autopilot)
- [Autopilot user-driven mode](https://learn.microsoft.com/en-us/autopilot/user-driven)
- [Unit 3 – Streamlined and Flexible](https://learn.microsoft.com/en-us/training/modules/benefits-microsoft-endpoint-manager/3-streamlined-and-flexible)

---

### Scenario 4: Migrating from Configuration Manager to Co-Management

**Problem:** An organization has used on-premises **Microsoft Configuration Manager (SCCM)** for years. They want to start leveraging cloud capabilities (Conditional Access, remote actions) without fully replacing SCCM immediately.

**Solution:** Enable **Co-Management** to run Configuration Manager and Intune in parallel.

**Detailed Steps:**
1. Ensure you are running **Configuration Manager version 1710** or later.
2. In **Configuration Manager**, navigate to **Administration > Cloud Services > Co-management**.
3. Run the **Co-management Configuration Wizard**.
4. Connect your Configuration Manager site to your **Microsoft Entra ID** tenant.
5. Enable **auto-enrollment** for existing domain-joined devices into Intune.
6. Configure **workload sliders** — choose which workloads to shift to Intune (start with **Compliance policies**, then gradually add **Resource access policies**, **Endpoint protection**, etc.).
7. Monitor device health from the **Microsoft Intune admin center**.

**Outcome:** You gain immediate cloud benefits (Conditional Access, remote wipe, device health visibility) while keeping SCCM for existing workflows. Gradual migration minimizes risk.

**Applicable Sources:**
- [What is co-management?](https://learn.microsoft.com/en-us/mem/configmgr/comanage/overview)
- [How to enable co-management](https://learn.microsoft.com/en-us/mem/configmgr/comanage/how-to-enable)
- [Unit 6 – Benefits of Co-Management](https://learn.microsoft.com/en-us/training/modules/benefits-microsoft-endpoint-manager/6-benefits-of-co-management)

---

### Scenario 5: Reducing IT Help Desk Tickets Using Endpoint Analytics

**Problem:** An organization's IT help desk is overwhelmed with tickets about slow devices and broken software. IT wants to proactively identify and fix issues before users notice them.

**Solution:** Enable and configure **Endpoint Analytics** in Microsoft Intune.

**Detailed Steps:**
1. In the **Intune admin center**, navigate to **Reports > Endpoint analytics**.
2. Enable data collection by assigning the **Intune data collection policy** to devices (Windows 10/11 version 1903 or later required).
3. Review the **Startup performance** score — identifies devices with slow boot/sign-in times and recommends hardware or software changes.
4. Review **Recommended software** insights — shows outdated OS versions and apps causing crashes.
5. Use **Proactive remediations** (now called **Remediations**):
   - Navigate to **Devices > Remediations**.
   - Create a script package to detect and fix a known issue (e.g., clear a problematic registry key or reinstall a broken app).
   - Deploy to affected device groups on a schedule.
6. Track improvements over time using the **Endpoint analytics score** dashboard.

**Outcome:** IT resolves device issues silently and automatically. Help desk ticket volume decreases significantly.

**Applicable Sources:**
- [Endpoint analytics overview](https://learn.microsoft.com/en-us/mem/analytics/overview)
- [Proactive remediations](https://learn.microsoft.com/en-us/mem/analytics/proactive-remediations)
- [Unit 5 – Increases Productivity](https://learn.microsoft.com/en-us/training/modules/benefits-microsoft-endpoint-manager/5-increases-productivity)
- [Unit 7 – Maximizes Investment](https://learn.microsoft.com/en-us/training/modules/benefits-microsoft-endpoint-manager/7-maximizes-investment)

---

### Scenario 6: Applying Security Baselines to Harden Windows Devices

**Problem:** A security team needs to ensure all Windows 11 corporate devices have consistent, hardened security settings aligned with Microsoft's recommendations. Manual configuration is not scalable.

**Solution:** Deploy **Intune Security Baselines** across all managed Windows devices.

**Detailed Steps:**
1. In the **Intune admin center**, navigate to **Endpoint security > Security baselines**.
2. Select **MDM Security Baseline for Windows 10 and later**.
3. Click **Create profile** and provide a name.
4. Review the default settings (they reflect Microsoft's security recommendations):
   - BitLocker encryption enabled.
   - Windows Defender Firewall enabled.
   - SmartScreen enabled.
   - Password complexity required.
5. Modify any settings that conflict with your organization's operational requirements.
6. Assign the baseline to your **All Windows Devices** group.
7. Monitor **baseline compliance** under **Endpoint security > Security baselines > [Profile] > Device status**.

**Outcome:** All Windows devices are consistently hardened. Compliance reporting shows which devices deviate from the baseline.

**Applicable Sources:**
- [Security baselines in Intune](https://learn.microsoft.com/en-us/mem/intune/protect/security-baselines)
- [Windows security baseline settings](https://learn.microsoft.com/en-us/mem/intune/protect/security-baseline-settings-mdm-all)
- [Unit 2 – Secure and Intelligent](https://learn.microsoft.com/en-us/training/modules/benefits-microsoft-endpoint-manager/2-secure-and-intelligent)

---

### Scenario 7: Maximizing ROI with Microsoft Graph API and Intune Automation

**Problem:** A large enterprise with 10,000+ devices needs to automate compliance reporting, device group assignments, and policy updates without manual admin effort.

**Solution:** Use the **Microsoft Graph API** with Intune to build automated management workflows.

**Detailed Steps:**
1. Register an app in **Microsoft Entra ID > App registrations**.
2. Grant the app the required **Microsoft Graph API permissions** for Intune (e.g., `DeviceManagementConfiguration.ReadWrite.All`).
3. Use the Graph Explorer (`https://developer.microsoft.com/en-us/graph/graph-explorer`) to test queries:
   ```http
   GET https://graph.microsoft.com/v1.0/deviceManagement/managedDevices
   ```
4. Build automation using PowerShell or a REST client:
   ```powershell
   # Example: Get all non-compliant devices
   $token = Get-IntuneAuthToken  # Your auth function
   $uri = "https://graph.microsoft.com/v1.0/deviceManagement/managedDevices?`$filter=complianceState eq 'noncompliant'"
   $response = Invoke-RestMethod -Uri $uri -Headers @{Authorization = "Bearer $token"} -Method Get
   $response.value | Select-Object deviceName, userPrincipalName, complianceState
   ```
5. Schedule the script to run daily and send compliance reports to your security team.

**Outcome:** Compliance reporting is automated. Policy violations are caught immediately without manual admin review.

**Applicable Sources:**
- [Intune Graph API reference](https://learn.microsoft.com/en-us/graph/api/resources/intune-graph-overview)
- [Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Unit 7 – Maximizes Investment](https://learn.microsoft.com/en-us/training/modules/benefits-microsoft-endpoint-manager/7-maximizes-investment)

---
