# Azure Virtual Desktop: Comprehensive Guide for Planning, Configuration, and Administration
---

## 1. Overview & Capabilities

Azure Virtual Desktop (AVD) is a cloud-based desktop and app virtualization service running on Microsoft Azure. It allows users to securely connect to remote desktops and applications from a wide variety of devices (Windows, macOS, iOS, Android, Linux, and HTML5-capable modern web browsers).

### Optimal User Experience
* **Native and Web Clients:** Secure access via localized native applications or a zero-footprint HTML5 browser.
* **Low Latency:** Session host virtual machines are optimally provisioned near data resources and core applications inside Azure datacenters.
* **Fast Profile Loading:** Leverages FSLogix containerized user profiles to enable instantaneous user sign-ins and eliminate traditional profile corruption or synchronization delays.
* **Personal vs. Pooled Desktops:** Supports personal (persistent) desktops for individual users who require local configuration privileges without impacting adjacent environments.

### Enhanced Security
* **Microsoft Entra ID Integration:** Enables robust authentication mechanisms including Multi-Factor Authentication (MFA) and granular Role-Based Access Control (RBAC).
* **Reverse Connect Technology:** Session host VMs establish outbound connections to the Azure Virtual Desktop infrastructure. This avoids exposing direct inbound ports (like TCP 3389) to the public internet, dramatically shrinking the attack surface.
* **Data Isolation:** Isolates discrete user sessions in both single-session and multi-session host topologies.

### Simplified Management
* **Unified Control Plane:** Centralized management of host pools, application groups, workspaces, and user assignments via standard Azure administration toolsets.
* **Monitoring:** Native integration with Azure Monitor for robust auditing, telemetry, performance alerts, and analytical debugging.

### Performance Management
* **Load Balancing Policies:** * *Breadth Mode:* Sequentially distributes incoming user sessions across available session host VMs within a host pool to optimize individual performance.
  * *Depth Mode:* Allocates users to a single session host VM until it reaches its maximum capacity threshold before shifting new connections to the next VM, maximizing compute cost efficiencies.
* **Autoscale:** Automatically starts and stops session hosts based on schedules, business hours, or real-time fluctuating demand.

### Licensing & Compute Cost Savings
* **Eligible Licensing:** Included at no extra base software cost for eligible Microsoft 365 or Windows Enterprise license holders. Customers only pay for underlying Azure infrastructure consumption.
* **Azure Reserved VM Instances:** Provides up to 72% compute cost reductions compared to traditional pay-as-you-go billing when opting into 1-year or 3-year term commitments.

---

## 2. Step-by-Step Configuration Guide

### Prerequisites
Before executing this deployment, ensure you possess the following architectural elements:
1. **Azure Account:** An active Azure subscription with either the **Owner** or **Contributor** built-in Azure RBAC role explicitly assigned.
2. **Virtual Network (VNet):** A pre-configured VNet residing in the target Azure region where your session hosts will be deployed.
3. **Identity Framework:** A Microsoft Entra ID tenant containing a test user account. This account must hold either the **Virtual Machine User Login** or **Virtual Machine Administrator Login** RBAC role assigned at the subscription level, resource group level, or directly on the individual session host virtual machines.
4. **Client Software:** A Remote Desktop client or an HTML5-compliant web browser.

### Deployment Steps via Azure Portal

1. Sign in to the [Azure Portal](https://portal.azure.com/).
2. In the global search bar, type **Azure Virtual Desktop** and select the matching service.
3. From the overview interface, select **Create a host pool**.
4. Configure the **Basics** tab using the baseline parameters defined below:

| Parameter | Value / Description |
| :--- | :--- |
| **Subscription** | Select your active Azure subscription. |
| **Resource group** | Select an existing resource group or click **Create new**. |
| **Host pool name** | Enter a descriptive identifier (e.g., `aad-hp01`). |
| **Location** | Select the target Azure region for deployment. |
| **Validation environment** | Select **No** (choosing Yes isolates the environment for pre-release service updates). |
| **Preferred app group type** | Select **Desktop**. |
| **Host pool type** | Select **Personal** (dedicates specific session hosts to individual users). |
| **Assignment type** | Select **Automatic** (automatically binds a user to the first available host on their primary sign-in). |

5. Select **Next: Virtual Machines** and populate the VM properties:

| Parameter | Value / Description |
| :--- | :--- |
| **Add Azure virtual machines** | Select **Yes**. |
| **Name prefix** | Enter a routing string (e.g., `aad-hp01-sh`). |
| **Virtual machine location** | Select the same Azure region as your target Virtual Network. |
| **Availability options** | Select **No infrastructure dependency required**. |
| **Security type** | Select **Standard**. |
| **Image** | Select **Windows 11 Enterprise, version 22H2**. |
| **Virtual machine size** | Retain or change the SKU based on workload demands. |
| **Number of VMs** | Set to a minimum of `1`. |
| **OS disk type** | Select **Premium SSD** for standard operational performance. |
| **Boot Diagnostics** | Select **Enable with managed storage account (recommended)**. |
| **Virtual network** | Select your pre-configured target VNet. |
| **Network security group** | Select **Basic**. |
| **Public inbound ports** | Select **No** (Reverse connect architecture ensures outbound-only posture). |
| **Select directory to join** | Select **Microsoft Entra ID**. |
| **Enroll VM with Intune** | Select **No**. |
| **Admin Username** | Enter a local administrator username for the session hosts. |
| **Admin Password** | Enter a strong password string for the administrator account. |

6. Proceed to the **Workspace** tab and enter the following properties:

| Parameter | Value / Description |
| :--- | :--- |
| **Register desktop app group** | Select **Yes** (automatically registers the default Desktop Application Group). |
| **To this workspace** | Click **Create new** and provide a clear name (e.g., `aad-ws01`). |

7. Select **Next: Review + create**, wait for the validation pass, and click **Create**. Once provisioning completes, select **Go to resource**.

### Assigning Users to Application Groups
1. Inside your target Host Pool management pane, navigate to **Application groups**.
2. Select the automatically created group (e.g., `aad-hp01-DAG`).
3. Select **Assignments** under the settings sidebar.
4. Click **+ Add**, search for your designated Microsoft Entra ID user account, and click **Select**.

### Enabling Remote Desktop Client Connections

> [!TIP]
> This advanced configuration step is optional if you are using a native Windows endpoint joined to the exact same Microsoft Entra ID tenant as your session host VMs and leveraging the official native Remote Desktop client for Windows.

To permit consistent authentication across all alternate endpoints (such as web browsers or macOS clients), you must supply a custom RDP property:
1. Navigate back to your primary **Host Pool Overview**, then select **RDP Properties**.
2. Click the **Advanced** tab.
3. Locate the text box and prepend exactly: `targetisaadjoined:i:1;` at the absolute beginning of the custom connection string.
4. Click **Save**.

---

## 3. Command-Line Administration (Azure CLI & PowerShell)

Command-line environments are natively accessible via Azure Cloud Shell or via standard local platform installations (Windows, macOS, Linux).

### Azure CLI Commands (`az desktopvirtualization`)
Ensure the `desktopvirtualization` extension is loaded into your terminal before execution.

#### Find Available Azure Regions
```bash
az account list-locations --query "sort_by([].{DisplayName:displayName, Location:name}, &Location)" -o table
