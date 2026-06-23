# Microsoft Configuration Manager (ConfigMgr) Client — Deploy, Monitor & Manage

---

## Overview

Microsoft Endpoint Configuration Manager (ConfigMgr / MECM) allows IT administrators to manage Windows computers at scale. The **Configuration Manager client** is a software agent installed on each Windows device that enables centralized management including software inventory, hardware inventory, OS deployment, application delivery, policy enforcement, and compliance monitoring.

This document consolidates three training units from the Microsoft Learn module:

| Unit | Title | Estimated Read |
|------|-------|---------------|
| Unit 2 | Deploy the Configuration Manager Client | 3 minutes |
| Unit 3 | Monitor the Configuration Manager Client | 3 minutes |
| Unit 4 | Manage the Configuration Manager Client | 2 minutes |

---

## Deploy the Configuration Manager Client

### Benefits of the Configuration Manager Client

Installing the Configuration Manager client on a Windows device enables capabilities for both IT administrators and end users.

**For IT Administrators:**
- Track software installed on devices
- Collect hardware inventory information
- Manage and deploy operating systems and Line-of-Business (LoB) applications
- With cloud management innovations, many features are no longer restricted to the on-premises network perimeter

**For End Users:**
- Access a feature-rich, self-service software catalog to independently install approved applications
- Configure personal working hours to minimize interruptions during updates or deployments

---

### Client Deployment Options

The following are the most commonly used methods to deploy the Configuration Manager client to Windows computers:

---

#### 1. Client Push

- Deploys the client **directly from the Configuration Manager console**
- Devices are typically discovered using **Active Directory LDAP integration** (locating devices by Organizational Unit)
- Once discovered, client push is initialized — this copies the client files to the target computer and **automatically initiates the installation**

> ⚠️ **Consideration:** For large-scale deployments, client push can cause **increased network traffic** due to the initial file copy process.

---

#### 2. Manual Deployment

- Involves manually copying client installation source files along with a **script file** containing the install parameters
- Installation is typically executed via `ccmsetup.exe`, but can also be run from the MSI included in the client files
- Various **client installation parameters** control how the client installs and form part of the scripted install

**Example CCMSetup command line:**

```cmd
Ccmsetup.exe SMSSITECODE=AUTO CCMLOGMAXSIZE=10000 CCMLOGMAXHISTORY=2
```

> ⚠️ **Note:** When not used with an automation tool, the manual scripted install can be time consuming as a delivery mechanism at scale.

**Parameter Breakdown:**

| Parameter | Purpose |
|-----------|---------|
| `SMSSITECODE=AUTO` | Automatically detect and assign the client to the correct site |
| `CCMLOGMAXSIZE=10000` | Sets the maximum log file size (in KB) |
| `CCMLOGMAXHISTORY=2` | Number of old log files to retain before overwriting |

---

#### 3. OS Deployment (OSD)

- One of the **most common** ways to deliver the client
- The Configuration Manager client is **slip-streamed into the Windows setup task sequence**, along with the necessary installation parameters
- Best used when a device is being **built for the first time** (or rebuilt from scratch)

> ⚠️ **Limitation:** This method is **not appropriate for existing devices** it can only be used during initial OS provisioning.

---

#### 4. Microsoft Intune (Co-Management)

- A modern method designed to facilitate **co-management** between Intune and Configuration Manager
- Intune drives the **Configuration Manager client installation** and registers the device with the **Cloud Management Gateway (CMG)**
- After installation, workloads can be managed from **either Intune or Configuration Manager** independently

---

## Monitor the Configuration Manager Client

After installing the Configuration Manager client on Windows devices and connecting them to your site, you can monitor and manage devices directly inside the **Configuration Manager console**.

### Built-in Client Status Indicators

The console includes several built-in status indicators that alert administrators when a client may be experiencing issues or requires attention:

---

#### Client Online Status

| Status | Condition |
|--------|-----------|
| **Online** | Device is connected to its assigned management point and has sent a ping-like message within the last **5 minutes** |
| **Offline** | Management point has not received a message from the client in **more than 5 minutes** |

---

#### Client Activity Status

| Status | Condition |
|--------|-----------|
| **Active** | Client has communicated with Configuration Manager within the **past 7 days** |
| **Inactive** | Client has **not** performed any of the following in the past 7 days: |

Actions tracked for activity status:
- Requested a policy update
- Sent a heartbeat message
- Sent hardware inventory

---

#### Primary User

- Viewable from the ECM (Endpoint Configuration Manager) console at a high level
- Calculated over a **60-day period** based on the **most frequent login user** on the device

---

#### Operating System Build

- Exposed via inventory data collected from the machine
- Allows administrators to **see the OS version** of a device without needing to connect to or perform remote management on it

---

#### Client Check

- Represents the state of a **periodic evaluation** that the Configuration Manager client runs on the device
- The evaluation checks the device health and can **automatically remediate** certain problems found
- Remediation can be configured **not to run** on specific devices (e.g., business-critical servers)

> 💡 **Co-Management Note:** Device remediation is a workload that can be utilized through co-management, allowing the client to be evaluated regardless of how the device was brought into the hierarchy.
>
> While **Windows Intune** also offers device compliance, its subset of non-compliance actions is still evolving. For this reason, **using Configuration Manager for remediation is often the most appropriate choice** in advanced scenarios such as making file or configuration changes to enforce compliance.

---

## Manage the Configuration Manager Client

Once the Configuration Manager client is installed and the device is successfully assigned to a site, the device appears in:

> **Assets and Compliance** workspace → **Devices** node

From here, the device is likely added to one or more **collections** from which it is actively managed.

---

### What is a Collection?

Collections are a way for Configuration Manager to **group devices or users** that share a common attribute. Collections can be defined by:

- A **query** (e.g., OS type, installed application)
- **Direct membership** (manually added devices)
- A combination of both

**What you can do with a Collection:**
- Target software or OS deployments
- Run compliance reports
- Apply client settings
- Execute management actions on all members simultaneously

---

### Common Client Management Actions

When a device is selected from the Devices pane or within a collection, the **right-click context menu** provides access to many management actions. The table below outlines the most common options:

| Client Action | Description |
|---------------|-------------|
| **Start Resource Explorer** | Review inventory information about a device, including OS information, memory, and installed applications |
| **Start Policy Retrieval** | Execute client actions locally, direct from the site server — triggers a policy refresh on the device |
| **Add to a Collection** | Add the client directly to a collection (e.g., a targeted installation collection) from the object itself |
| **Client Settings RSOP** | Review the current client settings applied to a given endpoint (Resultant Set of Policy) |

> 💡 Some actions can be performed on a **per-collection basis** (affecting all members) or on an **individual device object** directly.

---

## Tools and Resources Used

| Tool / Resource | Description | Link |
|-----------------|-------------|------|
| **Microsoft Endpoint Configuration Manager (MECM / ConfigMgr)** | Enterprise device management platform for Windows | [Microsoft Docs](https://learn.microsoft.com/en-us/mem/configmgr/) |
| **CCMSetup.exe** | Configuration Manager client installer executable | [CCMSetup Reference](https://learn.microsoft.com/en-us/mem/configmgr/core/clients/deploy/about-client-installation-properties) |
| **Microsoft Intune** | Cloud-based device management for co-management scenarios | [Microsoft Intune](https://learn.microsoft.com/en-us/mem/intune/) |
| **Cloud Management Gateway (CMG)** | Enables internet-based client management | [CMG Overview](https://learn.microsoft.com/en-us/mem/configmgr/core/clients/manage/cmg/overview) |
| **Active Directory LDAP** | Used for device discovery in client push deployments | [AD Discovery](https://learn.microsoft.com/en-us/mem/configmgr/core/servers/deploy/configure/about-discovery-methods) |
| **Assets and Compliance Workspace** | ConfigMgr console node for viewing and managing enrolled devices | Part of ConfigMgr Console |
| **Resource Explorer** | In-console tool for reviewing hardware/software inventory on a device | Part of ConfigMgr Console |
| **Configuration Manager Compliance Settings** | Evaluate and remediate device configurations | [Compliance Settings](https://learn.microsoft.com/en-us/mem/configmgr/compliance/deploy-use/get-started-with-compliance-settings) |
| **Microsoft Learn — Source Module** | Original training module these pages are extracted from | [Enroll Devices Using Endpoint Configuration Manager](https://learn.microsoft.com/en-us/training/modules/enroll-devices-use-endpoint-configuration-manager/) |

---

## Confirmed Scenarios with Detailed Solutions

The following are real-world scenarios derived directly from the concepts in this module, with detailed steps and applicable external references.

---

### ✅ Scenario 1: Client Push Fails for Newly Discovered Devices

**Problem:** After Active Directory discovery, client push installation is triggered but fails silently on several workstations.

**Root Causes (Common):**
- Firewall blocking SMB (TCP 445) or RPC ports
- Remote Registry service not running on target
- Insufficient admin credentials for the client push account
- Target device is offline or not reachable

**Detailed Solution Steps:**

1. In the ConfigMgr console, navigate to **Monitoring** → **Client Status** → review failed push install logs
2. On the site server, check `CCM.log` and `ccmsetup.log` located at:
   ```
   C:\Windows\ccmsetup\Logs\ccmsetup.log
   ```
3. Verify **Remote Registry** service is running on the target device:
   ```cmd
   sc \\<TargetPC> query RemoteRegistry
   sc \\<TargetPC> start RemoteRegistry
   ```
4. Confirm Windows Firewall allows **File and Printer Sharing** (SMB) inbound on the target
5. Ensure the **Client Push Installation Account** has local administrator rights on the target machine
6. Re-trigger client push: Right-click device in console → **Install Client**

**Applicable Sources:**
- [Troubleshoot client push installation](https://learn.microsoft.com/en-us/mem/configmgr/core/clients/deploy/deploy-clients-to-windows-computers#troubleshoot-client-push-installation)
- [Client installation log files](https://learn.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/log-files#client-installation-log-files)

---

### ✅ Scenario 2: Deploying the Client via Manual Script (Automation / GPO)

**Problem:** You need to deploy the ConfigMgr client to 200+ existing workstations without rebuilding them and without using client push.

**Detailed Solution Steps:**

1. Copy the ConfigMgr client source files from the site server to a network share accessible by all targets:
   ```
   \\SiteServer\SMS_<SiteCode>\Client\
   ```
2. Create a batch or PowerShell script using the `ccmsetup.exe` command:
   ```cmd
   Ccmsetup.exe SMSSITECODE=AUTO CCMLOGMAXSIZE=10000 CCMLOGMAXHISTORY=2
   ```
3. Use **Group Policy** (GPO) to deploy the script as a **Computer Startup Script**:
   - Open Group Policy Management Console (GPMC)
   - Create or edit a GPO linked to the target OU
   - Navigate to: *Computer Configuration → Windows Settings → Scripts → Startup*
   - Add the script pointing to the network share
4. Force a Group Policy update on target machines (if immediate rollout is needed):
   ```cmd
   gpupdate /force
   ```
5. Monitor installation progress from `ccmsetup.log` on each target or from the ConfigMgr console

**Applicable Sources:**
- [Client installation properties reference](https://learn.microsoft.com/en-us/mem/configmgr/core/clients/deploy/about-client-installation-properties)
- [Deploy clients using group policy](https://learn.microsoft.com/en-us/mem/configmgr/core/clients/deploy/deploy-clients-to-windows-computers#deploy-clients-using-group-policy)

---

### ✅ Scenario 3: Client Shows as "Inactive" in the Console

**Problem:** A device shows as **inactive** in the Configuration Manager console even though it is powered on and connected to the network.

**Root Cause:** The client has not performed a heartbeat, policy request, or hardware inventory in the past 7 days.

**Detailed Solution Steps:**

1. Verify the device is reachable (ping or remote desktop test)
2. From the ConfigMgr console, right-click the device and select **Start Policy Retrieval** to force an immediate policy refresh
3. On the client machine, manually trigger client actions via the **Configuration Manager control panel applet**:
   - Open **Control Panel** → **Configuration Manager**
   - Go to the **Actions** tab
   - Run:
     - *Machine Policy Retrieval & Evaluation Cycle*
     - *Hardware Inventory Cycle*
     - *Heartbeat Discovery Cycle*
4. Verify the Management Point is accessible from the device:
   ```cmd
   ping <ManagementPointFQDN>
   ```
5. Review `ClientIDManagerStartup.log` and `InventoryAgent.log` on the client:
   ```
   C:\Windows\CCM\Logs\
   ```

**Applicable Sources:**
- [Monitor client status](https://learn.microsoft.com/en-us/mem/configmgr/core/clients/manage/monitor-clients)
- [Client log files reference](https://learn.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/log-files#client-log-files)

---

### ✅ Scenario 4: Deploying ConfigMgr Client via Microsoft Intune (Co-Management)

**Problem:** Your organization is moving to modern management but needs to retain ConfigMgr for specific workloads. You need to auto-install the ConfigMgr client on Intune-enrolled devices.

**Detailed Solution Steps:**

1. Set up a **Cloud Management Gateway (CMG)** in your ConfigMgr environment (required for internet-based management)
2. In the ConfigMgr console, navigate to:
   - **Administration** → **Cloud Services** → **Co-management**
   - Run the **Co-management Configuration Wizard**
3. In Microsoft Intune (Endpoint Manager portal), configure the **ConfigMgr client app deployment**:
   - Navigate to **Devices** → **Windows** → **Configuration profiles**
   - Create a profile to push the `ccmsetup.exe` with the appropriate parameters including the CMG endpoint
4. After client installation, set **co-management workload sliders** to determine which workloads (e.g., Compliance Policies, Resource Access, Endpoint Protection) are managed by Intune vs ConfigMgr
5. Monitor co-management status from:
   - ConfigMgr console: **Monitoring** → **Co-Management**
   - Intune portal: **Devices** → **Monitor** → **Co-management eligibility**

**Applicable Sources:**
- [Co-management overview](https://learn.microsoft.com/en-us/mem/configmgr/comanage/overview)
- [How to enable co-management](https://learn.microsoft.com/en-us/mem/configmgr/comanage/how-to-enable)
- [Cloud Management Gateway overview](https://learn.microsoft.com/en-us/mem/configmgr/core/clients/manage/cmg/overview)

---

### ✅ Scenario 5: Using Collections to Target a Software Deployment

**Problem:** You need to deploy a Line-of-Business (LoB) application only to devices running Windows 11 in the Finance department.

**Detailed Solution Steps:**

1. In the ConfigMgr console, navigate to **Assets and Compliance** → **Device Collections**
2. Create a **New Device Collection**:
   - Right-click **Device Collections** → **Create Device Collection**
   - Set **Limiting Collection** to *All Systems*
   - Add a **Query Rule** with a WQL query such as:
     ```sql
     SELECT * FROM SMS_R_System WHERE SMS_R_System.OperatingSystemNameandVersion LIKE '%Windows 11%'
     ```
3. Add additional **Direct Membership Rules** or **Include Collection Rules** to further scope to the Finance OU
4. Navigate to **Software Library** → **Application Management** → **Applications**
5. Right-click your LoB application → **Deploy**
6. Set the **Collection** to the newly created Finance-Windows11 collection
7. Set deployment purpose (**Required** for mandatory or **Available** for self-service)
8. Monitor deployment compliance under **Monitoring** → **Deployments**

**Applicable Sources:**
- [Create collections in ConfigMgr](https://learn.microsoft.com/en-us/mem/configmgr/core/clients/manage/collections/create-collections)
- [Deploy applications](https://learn.microsoft.com/en-us/mem/configmgr/apps/deploy-use/deploy-applications)

---

### ✅ Scenario 6: Client Check Failing — Remediation Not Running on a Device

**Problem:** Client check reports failures on a non-critical workstation, but remediation never runs. The device remains non-compliant.

**Detailed Solution Steps:**

1. In the ConfigMgr console, navigate to **Monitoring** → **Client Status**
2. Click **Client Check** to see which devices have check failures
3. Verify that the device's **Client Settings** do not have remediation disabled:
   - Right-click the device → **Client Settings RSOP** to view resultant settings
   - Look for the **Client Status** section — confirm *Enable client remediation* is set to **Yes**
4. If remediation is disabled for a collection that includes this device:
   - Navigate to **Administration** → **Client Settings**
   - Edit the relevant custom client settings
   - Ensure *Schedule re-evaluation for deployments* and *Enable client health* are configured
5. Force a **Client Check** cycle on the device via:
   - ConfigMgr Control Panel applet → **Actions** → *Machine Policy Retrieval*
   - Then re-run *Client Check* from the same panel
6. Review `CcmEval.log` on the client for detailed remediation activity:
   ```
   C:\Windows\CCM\Logs\CcmEval.log
   ```

**Applicable Sources:**
- [Client health checks and remediation](https://learn.microsoft.com/en-us/mem/configmgr/core/clients/manage/client-health-checks)
- [Configure client status](https://learn.microsoft.com/en-us/mem/configmgr/core/clients/deploy/configure-client-status)

---

Alternatively, save this file locally and push it via Git:

```bash
git add README.md
git commit -m "Add ConfigMgr client deploy/monitor/manage documentation"
git push origin main
```


---
