# Microsoft Intune Compliance & Endpoint Manager — Monitoring and Reporting Guide

---

## 1. Intune Tenant Status

**Source:** [Learn about Intune tenant status](https://learn.microsoft.com/en-us/training/modules/compliance-endpoint-manager/2-intune-tenant-status)

### Overview

The **Microsoft Intune Tenant Status** page is a centralized hub where administrators can view current and important details about their tenant.

> **What is a tenant?**
> A tenant is a representation of an organization. It is a dedicated instance of **Microsoft Entra ID** (formerly Azure Active Directory) that an organization receives when they sign up for Microsoft Intune.

> **Note:** Microsoft Entra ID is the underlying infrastructure that supports identity management for all Microsoft cloud services. It stores information about license-assignment states for users. Your Intune subscription is hosted by a Microsoft Entra tenant.

### Key Details Available on the Tenant Status Page

| Detail | Description |
|---|---|
| Tenant Name | The name of your organization's Intune tenant |
| Tenant Location | The geographic region of your tenant |
| MDM Authority | The Mobile Device Management authority configured for your tenant |
| Service Release Number | Links to the *What's new in Intune* article on Microsoft Learn |
| End User Licenses | Total licenses in use and available |

### Navigation

- **Path:** Microsoft Intune admin center → **Tenant administration** → **Tenant status**
- The **Service Release Number** is a clickable link that opens the *What's new in Intune* article, allowing admins to review the latest features and updates.

<img width="736" height="469" alt="image" src="https://github.com/user-attachments/assets/665312fe-2d4a-4dd6-8984-70d902ab18bb" />

---

## 2. Intune Service Health Dashboard

**Source:** [Review Intune service health](https://learn.microsoft.com/en-us/training/modules/compliance-endpoint-manager/3-service-health-dashboard)

### Overview

As part of the **Tenant Status** dashboard, you can view details for:
- **Service incidents** that affect your tenant.
- **Intune news** that provides information about updates and planned changes.

### Intune Service Health

View details for **active incidents and advisories** without having to navigate to the Microsoft 365 Service Health Dashboard or the Message Center (both located in the [Microsoft 365 admin center](https://learn.microsoft.com/en-us/microsoft-365/admin/admin-overview/about-the-admin-center)).

> **Important:** Only incidents that affect **your specific tenant** are shown under the **Intune service health** section of the **Service health and message center** tab.

<img width="932" height="566" alt="image" src="https://github.com/user-attachments/assets/be5f1448-c42a-4644-a535-5de05745e35f" />

### Intune Message Center

View informational communications from the Intune service team without navigating to the Office Message Center. Communications include:
- Messages about **changes that have recently happened** to the Intune service.
- Messages about changes **on the way** for your tenant.

### Navigation

- **Path:** Microsoft Intune admin center → **Tenant administration** → **Tenant status** → **Service health and message center** tab

---

## 3. Troubleshooting + Support Workload

**Source:** [Understand the Troubleshooting + support workload](https://learn.microsoft.com/en-us/training/modules/compliance-endpoint-manager/4-troubleshooting-portal)

### Overview

The **Troubleshoot** pane lets **help desk operators** and **Intune administrators** view user information to address user help requests.

### Role-Based Access

- Organizations can assign the **Help desk operator** role to a group of users.
- The **Help desk operator** role can use the **Troubleshoot** pane.

### What the Troubleshoot Pane Shows

When a user contacts support with a technical issue, the help desk operator enters the user's name. Intune shows useful data that can help resolve many **tier-1 issues**, including:

- User status
- Assignments
- Compliance issues
- Device not responding
- Device not getting VPN or Wi-Fi settings
- App installation failure

### Enrollment Issue Visibility

The **Troubleshoot** pane also shows **user-enrollment issues**:
- Details about the issue and suggested remediation steps are displayed.
- Certain enrollment issues are **not captured**.
- Some errors might **not have remediation suggestions**.

### Navigation

- **Path:** Microsoft Intune admin center → **Troubleshooting + support** → **Troubleshoot**

<img width="2569" height="1417" alt="image" src="https://github.com/user-attachments/assets/3c504718-9959-4d14-a3e4-3cc07e0cedd9" />

---

## 4. Intune Reports

**Source:** [Learn about Intune reports](https://learn.microsoft.com/en-us/training/modules/compliance-endpoint-manager/5-intune-reports)

### Overview

Microsoft Intune reports allow you to **effectively and proactively monitor** the health and activity of endpoints across your organization. They also provide other reporting data across Intune, including:
- Device compliance
- Device health
- Device trends
- Custom reports for more specific data

### Report Focus Areas

| Type | Description | Primary Users |
|---|---|---|
| **Operational** | Timely, targeted data to help you focus and take action | Admins, subject matter experts, helpdesk |
| **Organizational** | Broader summary of overall view (e.g., device management state) | Managers, admins |
| **Historical** | Patterns and trends over a period of time | Managers, admins |
| **Specialist** | Raw data to create custom reports | Admins |

### Available Reports

| Report Name | Type |
|---|---|
| Non-compliant devices report | Operational |
| Windows 10/11 unhealthy endpoints report | Operational |
| Windows 10/11 detected malware report | Operational |
| Feature update failures report | Operational |
| Device compliance report | Organizational |
| Antivirus agent status report | Organizational |
| Detected malware report | Organizational |
| Windows 10/11 feature updates | Organizational |
| Device compliance trend report | Historical |
| Azure Monitor integration reports | Specialist |

> **Note:** A **device compliance summary report** is also available separately.

### Reporting Framework Capabilities

| Feature | Description |
|---|---|
| **Search and sort** | Search and sort across every column, no matter how large the dataset |
| **Data paging** | Scan data page-by-page or jump to a specific page |
| **Performance** | Quickly generate and view reports created from large tenants |
| **Export** | Quickly export reporting data generated from large tenants |

### Navigation

- **Path:** Microsoft Intune admin center → **Reports**

---

## 5. Endpoint Analytics

**Source:** [Understand endpoint analytics](https://learn.microsoft.com/en-us/training/modules/compliance-endpoint-manager/6-endpoint-analytics)

### Overview

End users often experience disruptions such as **long boot times** due to a combination of factors:
- Legacy hardware
- Software configurations not optimized for the end-user experience
- Issues caused by configuration changes and updates

Traditionally, IT has limited visibility into these issues, and support channels are slow and costly.

**Endpoint analytics** aims to:
- **Improve user productivity**
- **Reduce IT-support costs**

...by providing insights into the user experience, enabling IT to optimize the end-user experience with proactive support, and to detect regressions caused by configuration changes.

### Three Focus Areas of Endpoint Analytics

| Focus Area | Description |
|---|---|
| **Recommended software** | Recommendations for providing the best user experience |
| **Proactive remediation scripting** | Fix common support issues before end-users notice them |
| **Start up performance** | Help IT get users from power-on to productivity quickly without lengthy boot and sign-in delays |

### Business Impact

- Performance, reliability, and support issues that reduce user productivity can have a **large impact on an organization's bottom line**.
- Endpoint analytics provides **real-time insights** to act proactively rather than reactively.

### Navigation

- **Path:** Microsoft Intune admin center → **Reports** → **Endpoint analytics**

---

## 6. Connector Status

**Source:** [Review connector status](https://learn.microsoft.com/en-us/training/modules/compliance-endpoint-manager/7-connector-status)

### Overview

As part of the **Tenant Status** dashboard, you can view the status for **all available connectors** for Intune.

### What are Connectors?

Connectors are connections you configure to **external services**. There are two types:

| Type | Examples | Status Based On |
|---|---|---|
| **External service connections** | Apple Volume Purchase Program, Windows Autopilot | Last successful synchronization time |
| **Certificates / Credentials** | Apple Push Notification Services (APNS) certificates | Expiry timestamp of the certificate or credential |

<img width="1253" height="860" alt="image" src="https://github.com/user-attachments/assets/a98ed56e-774e-4bab-aa0a-cc4bb72a3d49" />

### How the Connector Status List is Organized

When you open the **Connector status** tab, connectors are displayed in this order:

1. **Unhealthy connectors** (displayed at the top)
2. **Connectors with warnings**
3. **Healthy connectors**
4. **Not Enabled** (connectors not yet configured)

### Interacting with Connectors

When you select a connector from the list:
- The portal presents the **relevant page** for that connector.
- You can view the status for **previously configured connectors**.
- You can select options to **add or create a new connector** of that type.
- You can **edit and fix issues** with an existing connector.

> **Example:** Selecting the **VPP Expiry Date** connector opens the **iOS Volume-Purchased Program Tokens** page, where you can view details, create a new configuration, or edit an existing one.

### Navigation

- **Path:** Microsoft Intune admin center → **Tenant administration** → **Tenant status** → **Connector status** tab

---

## 7. Client Health with Co-Management

**Source:** [Understand client health with co-management](https://learn.microsoft.com/en-us/training/modules/compliance-endpoint-manager/8-client-health)

### Overview

The health of your network is directly connected to the health of the devices moving in and out of it. With **co-management**, Intune and Configuration Manager work together to provide comprehensive client health visibility.

### Key Capabilities

| Capability | Description |
|---|---|
| **Intune communication** | Intune can communicate with an unhealthy client even when it is **not on your network** |
| **Configuration Manager reporting** | Reports back **98% of known healthy clients** |
| **Real-time visibility** | Detect, assess, and provide visibility across **all clients in real time** |
| **Compliance upgrades** | Adds support needed for compliance upgrades across all connected clients |

### CCMeval Utility

- **CCMeval** is an external utility to the Configuration Manager client.
- Provides **client health monitoring** and **auto remediation**.
- **Limitation:** This reporting relies on the device being **physically or virtually on your internal network**.
- **Co-management helps address this limitation** by enabling Intune to report on client health state even for off-network devices.

### What Co-Management Client Health Reporting Tells You

With co-management, Intune provides **timestamp information** for the validity of data, including whether your devices are:
- Healthy
- Able to connect
- Able to install apps
- Able to update to the required OS builds

### Navigation

- **Path:** Microsoft Endpoint Configuration Manager console → **Monitoring** workspace → **Co-management** node

---

## Tools and Resources Used

| Tool / Resource | Description | Link |
|---|---|---|
| **Microsoft Intune** | Cloud-based endpoint management and MDM/MAM service | [https://intune.microsoft.com](https://intune.microsoft.com) |
| **Microsoft Entra ID** | Identity and access management platform (formerly Azure AD) | [https://entra.microsoft.com](https://entra.microsoft.com) |
| **Microsoft 365 Admin Center** | Central portal for Microsoft 365 service management | [https://admin.microsoft.com](https://admin.microsoft.com) |
| **Microsoft 365 Service Health Dashboard** | View active service incidents across Microsoft 365 services | [https://admin.microsoft.com/#/servicehealth](https://admin.microsoft.com/#/servicehealth) |
| **Microsoft Endpoint Configuration Manager (MECM)** | On-premises device management platform (for co-management) | [https://learn.microsoft.com/en-us/mem/configmgr/](https://learn.microsoft.com/en-us/mem/configmgr/) |
| **Azure Monitor** | Monitoring and analytics platform; integrates with Intune reports | [https://learn.microsoft.com/en-us/azure/azure-monitor/](https://learn.microsoft.com/en-us/azure/azure-monitor/) |
| **Apple Volume Purchase Program (VPP)** | Apple's program for bulk app purchasing, integrated as an Intune connector | [https://learn.microsoft.com/en-us/mem/intune/apps/vpp-apps-ios](https://learn.microsoft.com/en-us/mem/intune/apps/vpp-apps-ios) |
| **Windows Autopilot** | Automated Windows device provisioning service, integrated as an Intune connector | [https://learn.microsoft.com/en-us/mem/autopilot/](https://learn.microsoft.com/en-us/mem/autopilot/) |
| **Apple Push Notification Services (APNS)** | Apple certificate-based service for managing iOS/macOS devices via Intune | [https://learn.microsoft.com/en-us/mem/intune/enrollment/apple-mdm-push-certificate-get](https://learn.microsoft.com/en-us/mem/intune/enrollment/apple-mdm-push-certificate-get) |
| **CCMeval** | Configuration Manager client health monitoring and auto-remediation utility | [https://learn.microsoft.com/en-us/mem/configmgr/core/clients/manage/monitor-clients](https://learn.microsoft.com/en-us/mem/configmgr/core/clients/manage/monitor-clients) |
| **Endpoint Analytics** | Built-in analytics for measuring end-user experience and startup performance | [https://learn.microsoft.com/en-us/mem/analytics/overview](https://learn.microsoft.com/en-us/mem/analytics/overview) |
| **What's New in Intune** | Release notes for the latest Intune features and updates | [https://learn.microsoft.com/en-us/mem/intune/fundamentals/whats-new](https://learn.microsoft.com/en-us/mem/intune/fundamentals/whats-new) |

---

## Confirmed Scenarios, Solutions, and Applicable Sources

The following scenarios are confirmed, applicable, and can be derived from the information in this module. Each includes detailed steps and references.

---

### Scenario 1 — Devices Showing as Non-Compliant After Policy Deployment

**Problem:** Devices appear non-compliant in Intune immediately after deploying a new compliance policy, even though settings appear correct.

**Solution:**

1. Navigate to **Microsoft Intune admin center** → **Reports** → **Device compliance report** (Organizational).
2. Review the list of non-compliant devices and the specific policy rules they fail.
3. Use the **Non-compliant devices report** (Operational) for more targeted, timely data.
4. Navigate to **Troubleshooting + support** → **Troubleshoot** and enter the affected user's name to inspect assignments and compliance status.
5. Verify whether the compliance policy is properly assigned to the correct user or device group.
6. Check the **Tenant Status** page to confirm no active service incidents are affecting policy delivery.
7. Allow time for the device to check in (Intune check-in cycle is typically 8 hours for compliance policies after initial enrollment).

**Applicable Sources:**
- [Intune Device Compliance Policies](https://learn.microsoft.com/en-us/mem/intune/protect/device-compliance-get-started)
- [Troubleshoot compliance policies in Intune](https://learn.microsoft.com/en-us/mem/intune/protect/troubleshoot-policies-in-microsoft-intune)
- [Non-compliant devices report (Operational)](https://learn.microsoft.com/en-us/mem/intune/fundamentals/reports)

---

### Scenario 2 — Service Incident Causing Device Management Disruption

**Problem:** Admins are unable to push policies or apps to devices and suspect a backend service issue.

**Solution:**

1. Navigate to **Microsoft Intune admin center** → **Tenant administration** → **Tenant status** → **Service health and message center** tab.
2. Review the **Intune service health** section for any active incidents or advisories affecting your tenant.
3. If an incident is found, note the incident ID and track progress.
4. Optionally navigate to the **Microsoft 365 Admin Center** → **Health** → **Service health** for a broader view.
5. Check the **Intune Message Center** for any planned maintenance or change notifications.
6. If no incident is listed, proceed to investigate at the tenant/device level using the **Troubleshoot** pane.

**Applicable Sources:**
- [Microsoft 365 Service Health Dashboard](https://learn.microsoft.com/en-us/microsoft-365/enterprise/view-service-health)
- [Intune Tenant Status](https://learn.microsoft.com/en-us/mem/intune/fundamentals/tenant-status)

---

### Scenario 3 — Help Desk Cannot Identify Why a User's Device is Not Getting Wi-Fi Settings

**Problem:** A user calls the help desk reporting their device is not receiving Wi-Fi configuration profiles.

**Solution:**

1. Navigate to **Microsoft Intune admin center** → **Troubleshooting + support** → **Troubleshoot**.
2. Enter the affected user's name to pull up their user status.
3. Review **Assignments** to confirm the Wi-Fi configuration profile is assigned to the user or device group.
4. Review **Compliance issues** and **Device status** to check for blocking conditions.
5. Confirm the device is enrolled and actively checking in (device not responding is a listed issue the pane detects).
6. Escalate if the issue is not visible in the Troubleshoot pane (some enrollment issues are not captured).

**Applicable Sources:**
- [Use the troubleshooting portal to help users at your company](https://learn.microsoft.com/en-us/mem/intune/fundamentals/help-desk-operators)
- [Troubleshoot Wi-Fi device configuration profiles](https://learn.microsoft.com/en-us/mem/intune/configuration/troubleshoot-wi-fi-profiles)

---

### Scenario 4 — APNS Certificate is Expiring and iOS Devices May Lose MDM Connectivity

**Problem:** An alert has been raised that the Apple Push Notification Services (APNS) certificate is nearing expiration.

**Solution:**

1. Navigate to **Microsoft Intune admin center** → **Tenant administration** → **Tenant status** → **Connector status** tab.
2. Look for the APNS connector entry — it will appear under **Unhealthy** or **Warning** if expiring.
3. Select the connector entry in the list to be taken to the **Apple MDM Push Certificate** page.
4. Renew the certificate **before expiration** using the same Apple ID that was used to create the original certificate.
5. Download the new certificate and upload it in the Intune portal.

> ⚠️ **Warning:** If the APNS certificate expires, all iOS/macOS devices will lose MDM connectivity and must be re-enrolled. Always use the **same Apple ID** to renew.

**Applicable Sources:**
- [Get an Apple MDM Push certificate for Intune](https://learn.microsoft.com/en-us/mem/intune/enrollment/apple-mdm-push-certificate-get)
- [Renew Apple MDM push certificate](https://learn.microsoft.com/en-us/mem/intune/enrollment/apple-mdm-push-certificate-get#renew-apple-mdm-push-certificate)

---

### Scenario 5 — Users Experiencing Long Boot Times and IT Wants to Investigate

**Problem:** IT receives multiple user complaints about slow boot times on corporate Windows devices.

**Solution:**

1. Navigate to **Microsoft Intune admin center** → **Reports** → **Endpoint analytics**.
2. Review the **Start up performance** score and baseline comparison.
3. Identify devices with the worst startup times and isolate common factors (hardware model, OS version, startup apps).
4. Use the **Proactive remediation scripting** feature to deploy scripts that fix common issues before users notice them.
5. Review **Recommended software** suggestions to ensure devices are running optimized configurations.
6. Monitor improvements in the Endpoint Analytics score over time after remediations are applied.

**Applicable Sources:**
- [What is Endpoint analytics?](https://learn.microsoft.com/en-us/mem/analytics/overview)
- [Startup performance in Endpoint analytics](https://learn.microsoft.com/en-us/mem/analytics/startup-performance)
- [Proactive remediations in Endpoint analytics](https://learn.microsoft.com/en-us/mem/analytics/proactive-remediations)

---

### Scenario 6 — Co-Managed Devices Showing Unknown Client Health Status

**Problem:** In a co-managed environment, some devices show as unknown or unhealthy in client health reports.

**Solution:**

1. Open the **Microsoft Endpoint Configuration Manager** console → **Monitoring** workspace → **Client status** node.
2. Review the **CCMeval** report for devices flagged as unhealthy.
3. Note that CCMeval requires the device to be on-network to report — for off-network devices, switch to the **Intune co-management** view.
4. In **Microsoft Intune admin center** → **Devices** → **Overview**, check the device's last check-in timestamp.
5. The Intune co-management health report will show whether the device is healthy, connected, able to install apps, and able to update the OS.
6. For persistent issues, review co-management workload assignments to ensure the correct authority (Intune vs. ConfigMgr) is managing the relevant workload.

**Applicable Sources:**
- [Co-management for Windows devices](https://learn.microsoft.com/en-us/mem/configmgr/comanage/overview)
- [How to monitor co-management](https://learn.microsoft.com/en-us/mem/configmgr/comanage/how-to-monitor)
- [Client health checks in Configuration Manager](https://learn.microsoft.com/en-us/mem/configmgr/core/clients/manage/monitor-clients)

---

### Scenario 7 — Generating a Custom Compliance Trend Report for Management

**Problem:** Management requests a report showing the trend of device compliance across the organization over the past several months.

**Solution:**

1. Navigate to **Microsoft Intune admin center** → **Reports** → **Device compliance** → **Reports** tab.
2. Select the **Device compliance trend report** (Historical type).
3. Use the **Data paging** feature to navigate through large datasets.
4. Use the **Export** feature to export the report data to a CSV file for sharing with management.
5. For advanced customization, connect **Azure Monitor** to Intune and build custom dashboards using Log Analytics or Power BI.

**Applicable Sources:**
- [Intune reports overview](https://learn.microsoft.com/en-us/mem/intune/fundamentals/reports)
- [Export Intune reports using Graph APIs](https://learn.microsoft.com/en-us/mem/intune/fundamentals/reports-export-graph-overview)
- [Intune data warehouse](https://learn.microsoft.com/en-us/mem/intune/developer/reports-nav-create-intune-reports)
- [Send data to Azure Monitor from Intune](https://learn.microsoft.com/en-us/mem/intune/fundamentals/review-logs-using-azure-monitor)

---
