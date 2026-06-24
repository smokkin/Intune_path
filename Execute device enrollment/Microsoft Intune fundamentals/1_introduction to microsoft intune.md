# Introduction to Microsoft Intune and Endpoint Management

---

## 1. What is Microsoft Intune?

Microsoft Intune is a **single, integrated management platform** for managing, protecting, and monitoring all of your organization's endpoints.

### What Are Endpoints?

Endpoints include all the apps and devices that your organization uses. Examples include:

- Mobile devices
- Desktop and laptop computers
- Virtual machines
- Embedded devices
- Shared devices
- Retail point-of-sale devices
- Ruggedized devices
- Digital interactive whiteboards
- Conference-room devices
- Holographic wearable computers

### What You Can Accomplish with Microsoft Intune

By protecting and managing your organization's endpoints with Microsoft Intune, you can:

- Help protect the data that people at your organization access
- Help safeguard the devices and apps that access organizational resources
- Ensure your organization uses proper credentials to access and share company data
- Confirm that security rules are in place based on your organization's requirements
- Confirm that every organization member's devices are correctly configured and protected
- Ensure that all corporate services are available to end users on all the devices they use
- Help new members of your organization have a good onboarding experience
- Ensure that users get a first-class support experience for all the products they use

### What Microsoft Intune Integrates

Microsoft Intune integrates:

- **Intune** (cloud-based MDM/MAM)
- **Microsoft Endpoint Configuration Manager** (on-premises/hybrid management)
- **Windows Autopilot** (zero-touch device provisioning)

You can use the **Microsoft Intune admin console** to help keep your organization's cloud and on-premises devices, apps, and data secure.

> **Important:** Microsoft Endpoint Configuration Manager and Microsoft Intune are now **one management system**. If you already have Configuration Manager, you also have Microsoft Intune.

---

## 2. Understand Microsoft Intune

Microsoft Intune provides:

- **Cloud-based mobile device management (MDM)**
- **Cloud-based mobile application management (MAM)**
- **Cloud-based computer management**

It helps protect your organization by controlling features and settings on:

| Platform             |
|----------------------|
| Android              |
| Android Enterprise   |
| iOS / iPadOS         |
| macOS                |
| Windows 10 / 11      |

### Integrations

Microsoft Intune integrates closely with:

- **Microsoft Entra ID** — for identity and access control
- **Microsoft Purview Information Protection** — for data classification and protection
- **Microsoft Defender** — advanced threat protection products

### Extended Connectivity

- When used with **Microsoft 365**, Intune helps your workforce be productive on all devices while keeping your organization's information protected.
- If you have **on-premises infrastructure** such as Exchange or Active Directory, you can use **Intune connectors** to connect to external services.
- Intune is included in Microsoft's **[Security suite](https://www.microsoft.com/security)**.

> **Architecture note:** The Intune infrastructure diagram (from Microsoft Learn) illustrates how Intune interacts with both on-premises and cloud infrastructure components.

---

## 3. Understand Microsoft Endpoint Configuration Manager

Microsoft Endpoint Configuration Manager is a **computer management solution** for managing:

- Desktop computers
- Laptops
- Servers (on your network or the internet)

### What You Can Do with Configuration Manager

- Deploy apps, software updates, and operating systems
- Configure sites and clients
- Run and monitor management tasks

### Supported Platforms & Environments

- **Supported OS:** Windows and macOS
- Devices can run in **virtual environments** such as:
  - Hyper-V on Windows servers
  - Virtual machines (VMs) in **Azure**
- If you run a server as an **Azure-based VM**, you can install the Configuration Manager client on that server.

### Cloud Integration

You can attach your Configuration Manager deployment to the **Microsoft 365 cloud** to provide integration with:

- Microsoft Intune
- Microsoft Entra ID
- Microsoft Defender
- Other cloud services

---

## 4. Cloud Attach — Tenant Attach & Co-Management

**Cloud attach** allows you to use both Microsoft Intune and Microsoft Endpoint Configuration Manager from within Intune.

There are **two steps** to cloud attach your on-premises devices:

```
Step 1: Tenant Attach  →  Step 2: Co-Management
```

These steps allow you to incrementally reach full cloud attachment — getting immediate value from tenant attach, and extra value from co-management.

---

### Step 1: Tenant Attach

**Tenant attach** registers your Configuration Manager deployment with your Intune tenant.

- Once connected, Configuration Manager devices and infrastructure are recognized and actionable from within Intune.
- Uses the **Configuration Manager connector** to enable data flow to Intune.
- Does **not** require turning on co-management.
- Provides **instant cloud value** after connection.

---

### Step 2: Co-Management

**Co-management** lets you concurrently manage Windows 10/11 devices with **both** Configuration Manager and Intune.

Co-management combines:
- Existing on-premises **Configuration Manager** and **Active Directory** investment
- Cloud services: **Intune**, **Microsoft Entra ID**, and other **Microsoft 365** cloud services

You choose whether **Configuration Manager** or **Intune** is the management authority. You can keep some tasks on-premises while running other tasks in the cloud.

#### Two Paths to Co-Management

| Path | Scenario | Steps |
|------|----------|-------|
| **Existing Clients** | Windows 10/11 devices already managed by Configuration Manager | Set up hybrid Microsoft Entra ID → Enroll devices into Intune |
| **New Cloud Devices** | Brand-new Windows 10/11 devices | Join Microsoft Entra ID → Auto-enroll in Intune → Install Configuration Manager client |

#### Immediate Capabilities After Enrolling in Co-Management

- ✅ Conditional access with device compliance
- ✅ Intune-based remote actions (restart, remote control, factory reset)
- ✅ Centralized device health visibility
- ✅ Microsoft Entra ID linking of users, devices, and apps
- ✅ Modern provisioning with Windows Autopilot

---

## 5. Endpoint Analytics

**Endpoint analytics** provides insights into how well your organization works and the quality of experience delivered to end users.

### What Endpoint Analytics Can Do

- Help identify **configuration or hardware issues** that slow down devices
- Provide **recommended actions** to remediate identified issues
- Proactively make changes **without disrupting end users** or generating help desk tickets

### Integration with Microsoft Adoption Score

Endpoint analytics are part of the **[Microsoft Adoption Score](https://learn.microsoft.com/en-us/microsoft-365/admin/adoption/adoption-score)**.

The Adoption Score provides:
- Productivity data
- Insights and recommendations for end users' experience
- Your organization's overall technology experience data

> **Goal:** Improve user productivity and reduce IT support costs by acting on data-driven insights.

---

## 6. Windows Autopilot

**Windows Autopilot** simplifies enrolling devices in Intune by letting you provide enrollment details **before** an end user receives a new computer.

### How It Works

- The Autopilot process runs immediately when the new computer **powers on for the first time**.
- The only interaction required from the end user is to:
  1. Connect to a network
  2. Verify their credentials
- Everything beyond that is **automated**.
- From the user's perspective, just a few simple steps make the device business-ready.

### What Windows Autopilot Can Do (via Cloud-Based Services)

- Automatically join devices to **Microsoft Entra ID**
- Auto-enroll devices into **MDM services** such as Microsoft Intune
- Restrict rights on the device
- Auto-assign devices to **configuration groups**
- Present customized **out-of-box-experience (OOBE)** content for your organization
- **Reset, repurpose, and recover** devices

### Benefits of Using Windows Autopilot

| Benefit | Description |
|---------|-------------|
| Reduces IT time | Less time deploying, managing, and retiring devices |
| Reduces infrastructure | Fewer requirements to maintain devices |
| Maximizes end-user ease of use | Simple, guided setup experience |

---

## Tools and Resources Used

| Tool / Resource | Description | Link |
|----------------|-------------|------|
| Microsoft Intune | Cloud-based MDM/MAM/computer management | [Microsoft Intune](https://learn.microsoft.com/en-us/mem/intune/) |
| Microsoft Endpoint Configuration Manager | On-premises/hybrid computer management solution | [Configuration Manager Docs](https://learn.microsoft.com/en-us/mem/configmgr/) |
| Windows Autopilot | Zero-touch device provisioning and enrollment | [Windows Autopilot](https://learn.microsoft.com/en-us/autopilot/) |
| Microsoft Entra ID | Identity and access control (formerly Azure AD) | [Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity/) |
| Microsoft Purview Information Protection | Data classification and protection | [Microsoft Purview](https://learn.microsoft.com/en-us/purview/) |
| Microsoft Defender | Advanced threat protection | [Microsoft Defender](https://learn.microsoft.com/en-us/defender/) |
| Microsoft 365 | Cloud productivity suite | [Microsoft 365](https://www.microsoft.com/microsoft-365) |
| Microsoft Adoption Score | Productivity data and insights platform | [Adoption Score Docs](https://learn.microsoft.com/en-us/microsoft-365/admin/adoption/adoption-score) |
| Microsoft Intune Admin Center | Unified console for Intune management | [Intune Admin Center](https://intune.microsoft.com) |
| Microsoft Security Suite | Security products including Intune | [Microsoft Security](https://www.microsoft.com/security) |
| Intune Connectors | Connect Intune to on-premises services (Exchange, AD) | [Intune Connectors](https://learn.microsoft.com/en-us/mem/intune/fundamentals/) |
| Tutorial: Walk through Intune Admin Center | Guided tour of the Intune console | [Tutorial Link](https://learn.microsoft.com/en-us/mem/intune/fundamentals/tutorial-walkthrough-endpoint-manager) |

---

## Confirmed Scenarios and Detailed Solutions

The following are real-world scenarios derived from the content above, with step-by-step solutions and applicable sources.

---

### Scenario 1: Enrolling a New Windows 11 Device via Windows Autopilot

**Situation:** Your IT team is deploying 50 new Windows 11 laptops. You want users to set up their devices themselves with minimal IT involvement.

**Solution:**

1. **Register devices with Autopilot** — Obtain the hardware hash from the OEM or using PowerShell, and upload it to the [Microsoft Intune Admin Center](https://intune.microsoft.com) under **Devices > Windows > Windows Enrollment > Devices**.
2. **Create an Autopilot Deployment Profile** — In Intune admin center, go to **Devices > Enrollment > Deployment Profiles** and configure User-Driven or Self-Deploying mode.
3. **Assign the profile** to a device group.
4. **Configure OOBE settings** — Set your organization's custom out-of-box experience.
5. **Ship the device to the user** — When powered on, the device connects to the internet, pulls the Autopilot profile, joins Microsoft Entra ID, and auto-enrolls in Intune.
6. **User enters their corporate credentials** — Device is fully configured and policies applied automatically.

**Applicable Sources:**
- [Windows Autopilot Overview](https://learn.microsoft.com/en-us/autopilot/windows-autopilot)
- [Intune Autopilot Deployment Profiles](https://learn.microsoft.com/en-us/autopilot/profiles)
- [Module Source](https://learn.microsoft.com/en-us/training/modules/intro-to-endpoint-manager/7-windows-autopilot)

---

### Scenario 2: Setting Up Co-Management for Existing Configuration Manager Clients

**Situation:** Your organization manages 500 on-premises Windows 10/11 devices using Configuration Manager. You want to gradually move management to the cloud using Intune without losing existing investments.

**Solution:**

1. **Verify Prerequisites** — Confirm Configuration Manager version 1906 or later, Microsoft Entra ID tenant, and Intune licenses.
2. **Enable Tenant Attach first** — In Configuration Manager console, go to **Administration > Cloud Services > Cloud Attach** and run the Cloud Attach wizard.
3. **Set up Hybrid Microsoft Entra Join** — Configure your on-premises AD to sync devices with Microsoft Entra ID via **Microsoft Entra Connect**.
4. **Enable Co-management in Configuration Manager** — Navigate to **Administration > Cloud Services > Co-management** and start the co-management wizard.
5. **Choose pilot collection** — Enroll a subset of devices first to validate.
6. **Shift workloads incrementally** — Move specific workloads (Compliance Policies, Resource Access, Windows Update policies) from Configuration Manager to Intune one at a time.
7. **Monitor in Intune Admin Center** — Review device compliance, health, and co-management status.

**Applicable Sources:**
- [Co-management Overview](https://learn.microsoft.com/en-us/mem/configmgr/comanage/overview)
- [How to Enable Co-management](https://learn.microsoft.com/en-us/mem/configmgr/comanage/how-to-enable)
- [Module Source](https://learn.microsoft.com/en-us/training/modules/intro-to-endpoint-manager/5-cloud-attach)

---

### Scenario 3: Applying Conditional Access with Device Compliance

**Situation:** You need to ensure only compliant, enrolled devices can access Microsoft 365 services such as Exchange Online and SharePoint.

**Solution:**

1. **Create a Compliance Policy in Intune** — Go to **Devices > Compliance Policies > Create Policy**, selecting Windows 10/11. Set rules such as minimum OS version, BitLocker required, and antivirus enabled.
2. **Assign the policy** to the target user or device group.
3. **Set non-compliance action** — Configure actions like "Mark device non-compliant" after a grace period.
4. **Create a Conditional Access Policy in Microsoft Entra ID** — Under **Protection > Conditional Access**, create a new policy:
   - **Users:** Target group
   - **Cloud apps:** Microsoft 365 apps (Exchange, SharePoint)
   - **Conditions:** Device platforms — Windows
   - **Grant:** Require device to be marked as compliant
5. **Enable the policy** and monitor sign-in logs.

**Applicable Sources:**
- [Device Compliance Policies in Intune](https://learn.microsoft.com/en-us/mem/intune/protect/device-compliance-get-started)
- [Conditional Access and Intune](https://learn.microsoft.com/en-us/mem/intune/protect/conditional-access)
- [Microsoft Entra Conditional Access](https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview)
- [Module Source](https://learn.microsoft.com/en-us/training/modules/intro-to-endpoint-manager/5-cloud-attach)

---

### Scenario 4: Using Endpoint Analytics to Diagnose Slow Device Boot Times

**Situation:** Your help desk is receiving an influx of complaints about slow device startup times. You want to proactively identify and fix the issue without waiting for users to file tickets.

**Solution:**

1. **Access Endpoint Analytics** — In the [Intune Admin Center](https://intune.microsoft.com), navigate to **Reports > Endpoint Analytics**.
2. **Review the Startup Performance Score** — Identify devices with below-average boot or sign-in times.
3. **Drill into device details** — Click on specific devices to view boot process breakdowns (BIOS time, disk performance, Group Policy processing).
4. **Review Recommended Insights** — Endpoint Analytics suggests remediation actions such as replacing spinning HDDs with SSDs, reducing startup app load, or adjusting Group Policy.
5. **Create a Proactive Remediation script** (Intune Remediations) — Deploy a PowerShell script to fix identified issues (e.g., disable unnecessary startup programs).
6. **Monitor improvement** — Track score improvements in the dashboard over time.

**Applicable Sources:**
- [Endpoint Analytics Overview](https://learn.microsoft.com/en-us/mem/analytics/overview)
- [Startup Performance in Endpoint Analytics](https://learn.microsoft.com/en-us/mem/analytics/startup-performance)
- [Proactive Remediations](https://learn.microsoft.com/en-us/mem/analytics/proactive-remediations)
- [Module Source](https://learn.microsoft.com/en-us/training/modules/intro-to-endpoint-manager/6-endpoint-analytics)

---

### Scenario 5: Managing Mobile Devices (iOS/Android) with Intune MAM

**Situation:** Employees use personal iPhones and Android devices to access corporate email and files (BYOD). You need to protect corporate data on these devices without a full MDM enrollment.

**Solution:**

1. **Configure App Protection Policies (MAM-WE)** — In Intune admin center, go to **Apps > App Protection Policies** and create a policy for iOS and Android.
2. **Set data protection rules:**
   - Prevent copy/paste to non-managed apps
   - Require PIN to access managed apps
   - Block backup of corporate data to personal cloud storage
   - Require encryption
3. **Assign the policy** to user groups.
4. **Deploy managed apps** (Outlook, Teams, OneDrive) via the **Company Portal** or via app assignment in Intune.
5. **Users install the Company Portal** (or use apps with integrated Intune SDK) — they are NOT required to enroll the full device.
6. **Corporate data is wiped selectively** if a device is lost or the employee leaves — personal data is untouched.

**Applicable Sources:**
- [App Protection Policies Overview](https://learn.microsoft.com/en-us/mem/intune/apps/app-protection-policy)
- [MAM without Enrollment](https://learn.microsoft.com/en-us/mem/intune/apps/mam-faq)
- [Module Source](https://learn.microsoft.com/en-us/training/modules/intro-to-endpoint-manager/3-intune)

---

### Scenario 6: Deploying Software Updates to Windows Devices via Configuration Manager

**Situation:** You need to deploy a critical Windows security update to 200 on-premises Windows 11 workstations managed by Configuration Manager.

**Solution:**

1. **Synchronize Software Updates** — In Configuration Manager, go to **Software Library > Software Updates > All Software Updates** and sync the update catalog.
2. **Identify the required update** — Search by KB number or classification (Security Updates).
3. **Create a Software Update Group** — Right-click the update(s) and select **Deploy**. Configure a Software Update Group.
4. **Create a Deployment Package** — Define where the update content will be stored on your distribution points.
5. **Configure the Deployment** — Set deployment deadline, user experience settings (suppress restarts, maintenance window), and monitoring alerts.
6. **Distribute Content** to Distribution Points.
7. **Monitor Compliance** — Navigate to **Monitoring > Deployments** to track success/failure rates.

**Applicable Sources:**
- [Deploy Software Updates in Configuration Manager](https://learn.microsoft.com/en-us/mem/configmgr/sum/deploy-use/deploy-software-updates)
- [Software Update Management Overview](https://learn.microsoft.com/en-us/mem/configmgr/sum/understand/software-updates-introduction)
- [Module Source](https://learn.microsoft.com/en-us/training/modules/intro-to-endpoint-manager/4-endpoint-configuration-manager)

---
