# Implement Dynamic Deployment — Windows Subscription Activation, Provisioning Packages & Microsoft Entra Join

> `IT Administrators, Desktop Engineers, Windows Deployment Specialists`
> `Windows 10, Windows 11, Microsoft Entra ID (Azure AD), Microsoft Intune`

---

## 1. Examine Subscription Activation

> **Module Unit:** [Unit 2 — Examine Subscription Activation](https://learn.microsoft.com/en-us/training/modules/implement-dynamic-deployment/2-examine-subscription-activation)  
> **Estimated time:** 7 minutes

Activating a Windows subscription is necessary to comply with licensing requirements. Activation links the Windows product key to a particular installation of Windows on a device. It assures software integrity and provides access to Microsoft support and a full range of updates.

### Activation Methods (Overview)

There are three main methods for activation:

| Method | Description |
|--------|-------------|
| **Retail** | Standard consumer product key activation |
| **OEM** | Key embedded in device firmware by the manufacturer |
| **Microsoft Volume Licensing** | Uses Volume Activation Services for enterprise environments |

Microsoft Volume Licensing customers use **Volume Activation Services**, which consists of:

- Active Directory–based activation
- Key Management Service (KMS)
- Multiple Activation Key (MAK)

---

### Current Volume Activation Methods

Enterprise environments use three main types of volume activation models running on Windows Server 2016+:

#### Key Management Service (KMS)

A role service used to activate systems within a network from a computer where a KMS host has been installed. By default, volume editions of Windows connect to a KMS host to request activation. **No action is required from users.**

#### Multiple Activation Key (MAK)

Uses product keys that can activate a specific number of computers. MAKs can activate any Windows volume edition.

#### Active Directory–Based Activation

A role service that uses AD DS (Active Directory Domain Services) to store activation objects. This approach simplifies maintenance of volume activation services. **No host server is required**, and activation requests are processed automatically during client computer startup.

---

### Subscription Activation

Devices are often purchased with **Windows Pro** pre-installed using a firmware-embedded key. Traditionally, these needed to be re-imaged with Windows Enterprise edition. **Subscription Activation removes the need for re-imaging**, transforming a Windows Pro device into Windows Enterprise without a reboot.

You can deploy **Windows Enterprise E3/E5/A3/A5** licenses using:

- **Windows Enterprise Subscription Activation** (with an existing Enterprise Agreement)
- **Windows Enterprise E3 in CSP** (via Cloud Solution Provider)
- **Microsoft Entra ID** (for identity management and license assignment)

| Deployment Path | Details |
|----------------|---------|
| **With existing EA** | Windows Enterprise E3 or E5 licenses may be available at no additional cost, depending on your EA |
| **Without existing EA** | Licenses must be purchased from a Cloud Solution Provider (CSP) before assignment |

---

### Subscription Activation Requirements

To implement Subscription Activation, your organization must meet **all** of the following requirements:

- [ ] Windows Pro / Pro Education / Enterprise / Education is installed **and activated** on the target devices.
- [ ] An instance of **Microsoft Entra ID** is available for identity management.
- [ ] Devices are either **Microsoft Entra joined** or **Microsoft Entra hybrid joined**.
- [ ] *(Education only)* The tenant must have an active subscription to Microsoft 365 with a Windows Enterprise license, or a Windows Enterprise/Education subscription.

---

### How Subscription Activation Works

With Subscription Activation, users can upgrade their devices from **Windows Pro to Windows Enterprise** without entering a product key and without restarting their computers.

**Option 1 — EA or Microsoft Products and Services Agreement (MPSA):**

When a licensed user signs in using Microsoft Entra credentials associated with a **Windows 11 Enterprise E3/E5 or A3/A5** license on a qualifying device:

1. The operating system switches from Windows Pro to Windows Enterprise.
2. All appropriate Windows Enterprise features become available.
3. If the subscription expires or is transferred to another user, the device **reverts to Windows Pro** after a grace period of **up to 90 days**.

**Option 2 — Enterprise E3 via CSP:**

- Delivered as a **subscription-based model** (monthly fee).
- Provides Windows Enterprise edition features without upfront purchasing.
- Available through the **CSP channel via Partner Center** as an online service.
- Flexible, **per-user subscription** for small- and medium-sized organizations (1 to hundreds of users).
- When a user signs in with Microsoft Entra credentials linked to a Windows Enterprise E3 license, the OS switches from Windows Pro to Windows Enterprise.

---

### VDA Subscription Activation

For **virtualized clients**, you can take advantage of subscriptions to Windows Enterprise via Virtual Desktop Access (VDA). Windows Enterprise E3 and E5 are available for VDA in:

- **Microsoft Azure**
- Other appropriate hosting platforms

> **Important:** VMs must be configured to enable Windows Enterprise subscriptions for VDA. Both **AD DS-joined** and **Microsoft Entra joined** clients are supported.

VDA Subscription Activation supports **Inherited Activation** starting with **Windows 10, version 1803**. This enables a Windows VM to inherit its activation state from the Windows host.

---

### VDA Subscription Activation Requirements

For VMs to support Windows Subscription Activation, they must:

- [ ] Run **Windows 11 Pro**
- [ ] Be a member of an **AD DS domain** or be **Microsoft Entra joined**
- [ ] Be **Generation 1** VMs
- [ ] Be hosted by a **Qualified Multitenant Hoster (QMTH)**, such as Microsoft Azure

---

### Inherited Activation

Windows virtual machines can inherit their activation state from a Windows host. When a Windows user with an **E3/E5/A3/A5 license** creates a new Windows VM on their Windows host, the VM inherits the activation state automatically.

> **Further Reading:** [Windows Subscription Activation — Microsoft Docs](https://learn.microsoft.com/en-us/windows/deployment/windows-10-enterprise-subscription-activation)

---

## 2. Deploy Using Provisioning Packages

> **Module Unit:** [Unit 3 — Deploy Using Provisioning Packages](https://learn.microsoft.com/en-us/training/modules/implement-dynamic-deployment/3-deploy-provisioning-packages)  
> **Estimated time:** 3 minutes

A **provisioning package** is a method of applying configuration settings to a Windows 10 or later device using:

- Removable media (e.g., USB drive)
- Direct download to the device

Provisioning packages are created using a graphical tool called **Windows Configuration Designer (WCD)**. Similar to Group Policies, administrators use WCD to select configuration options. WCD then exports a **`.ppkg` package file** containing the settings, which can be applied to Windows 10 or Windows 11 devices.

> **Advantage:** Provisioning packages are faster than deploying a new customized Windows image because the file sizes are small.

---

### What a Provisioning Package Can Do

You can use a provisioning package to modify a Windows installation and configure many runtime settings **without reinstalling the OS**. Use cases include:

- Configure the Windows user interface
- Add additional files or applications
- Adjust connectivity settings
- Meet mobile network requirements
- Manage certificates
- Comply with security policies
- Remove installed software
- Reset the Windows installation to its initial state

---

### Configurable Settings

In a provisioning package, you can configure settings including (but not limited to):

| Setting | Example Use |
|---------|-------------|
| `ComputerAccount` | Set device name, join a domain, specify OU, set domain-join credentials |
| `Certificates` | Bundle certificate files for deployment |
| `EditionUpgrade` | Upgrade Windows Pro → Enterprise or Education; change product keys |
| `Folders` | Add files and folders to the device |
| `Policies` | Apply security and compliance policies |

**`ComputerAccount` — Example configuration capabilities:**

```text
- Assign the name for a Windows computer
- Specify which domain the computer joins
- Define the Organizational Unit (OU) for the computer account
- Specify the user account used to join the domain
```

**`EditionUpgrade` — Example configuration capabilities:**

```text
- Upgrade Windows Pro → Windows Enterprise or Windows Education
- Upgrade Windows Home → Windows Education (no reinstallation required)
- Change a product key
- Perform license activation
- Apply runtime settings to:
    * A running Windows computer
    * An offline Windows image
    * During OS installation
```

---

### How to Apply a Provisioning Package

After configuring settings in WCD, export the package to a **`.ppkg` file**. During export, you can:

- **Encrypt** the package
- **Sign** the package (allows enforcing that only trusted packages are applied on client computers)

Apply the provisioning package using one of the following methods:

```powershell
# Method 1: PowerShell cmdlet
Add-ProvisioningPackage -PackagePath "C:\Packages\config.ppkg"
```

```text
Method 2: Run the .ppkg file directly (double-click or execute)
Method 3: Add via the Settings app → Accounts → Access work or school → Add or remove a provisioning package
```

> **Note:** Provisioning Packages are beneficial for organizations without a centralized management tool. They are also valuable in **enterprise scenarios with limited or unreliable internet connectivity**, since they can be applied **offline**.

---

## 3. Use Windows Configuration Designer

> **Module Unit:** [Unit 4 — Use Windows Configuration Designer](https://learn.microsoft.com/en-us/training/modules/implement-dynamic-deployment/4-use-windows-configuration-designer)  

**Windows Configuration Designer (WCD)** is included in the **Windows Assessment and Deployment Kit (Windows ADK)**. It is used to create provisioning packages through a GUI wizard interface.

> WCD provides three wizard types: **Desktop**, **Mobile**, and **Kiosk**.

<img width="752" height="471" alt="image" src="https://github.com/user-attachments/assets/bd560ba8-23a4-401a-9103-c01372c9e415" />

---

### WCD Wizard Configuration Table

The following table describes available configuration steps across the three wizard types:

| Step | Description | Desktop Wizard | Mobile Wizard | Kiosk Wizard |
|------|-------------|:--------------:|:-------------:|:------------:|
| **Set up device** | Assign device name, enter product key to upgrade Windows, configure shared use, remove preinstalled software | ✅ | Only device name and upgrade key | ✅ |
| **Set up network** | Connect to a wireless network | ✅ | ✅ | ✅ |
| **Account management** | Enroll in AD DS, enroll in Microsoft Entra ID, or create a local administrator account | ✅ | ❌ | ✅ |
| **Bulk enrollment in Microsoft Entra ID** | Enroll the device in Microsoft Entra ID before using a WCD wizard | ❌ | ✅ | ❌ |
| **Add applications** | Install applications via the provisioning package | ✅ | ❌ | ✅ |
| **Add certificates** | Include a certificate file in the provisioning package | ✅ | ❌ | ✅ |
| **Configure kiosk account and app** | Create a local account to run kiosk mode app; specify the app for kiosk mode | ❌ | ❌ | ✅ |
| **Configure kiosk common settings** | Set tablet mode, configure welcome/shutdown screens, turn off timeout settings | ❌ | ❌ | ✅ |

> **Timing:** A provisioning package can be applied **during Windows 11 deployment** or **after OS installation**.

---

### Microsoft Entra Join with Automatic MDM Enrollment (WCD Context)

The **Azure AD/MDM dynamic provisioning method** is cloud-driven and based on **Microsoft Entra ID P1 or P2** and **Microsoft Intune**. After enrolling a device in Intune MDM:

- MDM enforces compliance with corporate policies
- MDM can add or remove applications
- MDM reports device compliance back to Microsoft Entra ID
- Microsoft Entra ID allows access to corporate resources **only to compliant devices**

**Using Azure AD/MDM, you can:**

- Join devices to Microsoft Entra ID automatically
- Auto-enroll users' devices into MDM services
- Configure joined devices using MDM policies

**Requirements for Azure AD/MDM deployment model:**

| Requirement | Details |
|-------------|---------|
| **OS** | Windows 10/11 Pro or Enterprise edition |
| **Identity** | An instance of Microsoft Entra ID |
| **MDM** | An appropriate MDM solution, such as Microsoft Intune |

---

## 4. Use Microsoft Entra Join with Automatic MDM Enrollment

> **Module Unit:** [Unit 5 — Use Microsoft Entra Join with Automatic MDM Enrollment](https://learn.microsoft.com/en-us/training/modules/implement-dynamic-deployment/5-use-azure-ad-join-automatic-mobile-device-management-enrollment)  
> **Estimated time:** 2 minutes

Joining a device to **Microsoft Entra ID** is considered a method of **dynamic deployment**. This process applies to both:

- **Organization-owned devices** (CYOD — Choose Your Own Device)
- **User-owned devices** (BYOD — Bring Your Own Device)

---

### Organizational-Owned Devices

Instead of joining a domain via a traditional domain controller, the device is enrolled using **Microsoft Entra join**. This approach:

- Streamlines deployment and offers new deployment options
- Supports scenarios such as a **store-bought PC** where the user self-enrolls with their credentials
- Applies **organizational policies and configurations** during the enrollment process to ensure compliance
- Can **deny enrollment** if compliance conditions are not met
- Is **faster and easier** than requiring a user to reimage the device
- Can serve as a **standard IT device provisioning process**, not just an exception

---

### User-Owned (BYOD) Scenarios

MDM enrollment provides an easy way for users and vendors to use personal devices to access organizational apps and resources.

**User experience:**

1. User adds their **Microsoft work account** to Windows.
2. User gets simpler and safer access to organizational apps and resources.
3. IT enforces certain policies — users can **accept or reject** them but must accept to gain access.

**IT/MDM capabilities in BYOD scenarios:**

- MDM enforces compliance with corporate policies
- MDM can add or remove applications
- MDM reports device compliance to Microsoft Entra ID
- Microsoft Entra ID restricts corporate resource access to **only compliant devices**

---

## 5. Confirmed Scenarios, Detailed Solutions & Applicable Sources

The following real-world scenarios are confirmed applicable based on the module content above, along with detailed solutions, steps, and external references.

---

### ✅ Scenario 1: Upgrading Windows Pro to Enterprise Without Re-Imaging

**Problem:** A company procures new laptops pre-loaded with Windows Pro (OEM). The IT team needs to upgrade them to Windows Enterprise without re-deploying an image.

**Solution:** Use **Windows Subscription Activation** with Microsoft Entra ID.

**Steps:**

1. Verify all devices have Windows Pro activated and are Microsoft Entra joined (or hybrid joined).
2. In the Microsoft 365 admin center, assign **Windows Enterprise E3 or E5** licenses to users.
3. Instruct users to sign in to Windows with their Microsoft Entra (work) credentials.
4. The operating system automatically upgrades from Windows Pro to Windows Enterprise — **no reboot required**.
5. Enterprise features become available immediately.

> **Grace Period:** If a license expires or is reassigned, the device reverts to Windows Pro after up to **90 days**.

**Sources:**
- [Windows Subscription Activation](https://learn.microsoft.com/en-us/windows/deployment/windows-10-enterprise-subscription-activation)
- [Assign licenses to users — Microsoft 365 admin center](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/assign-licenses-to-users)

---

### ✅ Scenario 2: Deploying Standardized Settings to Devices Without Internet Access

**Problem:** A manufacturing company needs to configure Windows devices on a factory floor where internet access is restricted or unavailable.

**Solution:** Use **Provisioning Packages** created with Windows Configuration Designer (WCD), deployed via USB.

**Steps:**

1. Download and install the **Windows ADK** (includes Windows Configuration Designer).
2. Open WCD and create a new project using the appropriate wizard (Desktop, Mobile, or Kiosk).
3. Configure the required settings:
   - `ComputerAccount` — device name, domain join, OU
   - `Policies` — security and compliance baselines
   - `Certificates` — required certificates
4. Export the project as a signed and encrypted **`.ppkg`** file.
5. Copy the `.ppkg` file to a USB drive.
6. On each target device, plug in the USB drive and run the `.ppkg` file, or apply it via:
   ```powershell
   Add-ProvisioningPackage -PackagePath "D:\config.ppkg"
   ```
7. Confirm the settings are applied.

**Sources:**
- [Provisioning packages for Windows — Microsoft Docs](https://learn.microsoft.com/en-us/windows/configuration/provisioning-packages/provisioning-packages)
- [Windows Configuration Designer reference](https://learn.microsoft.com/en-us/windows/configuration/provisioning-packages/provisioning-install-icd)
- [Add-ProvisioningPackage — PowerShell](https://learn.microsoft.com/en-us/powershell/module/provisioning/add-provisioningpackage)

---

### ✅ Scenario 3: Setting Up Store-Bought PCs for Corporate Use (CYOD)

**Problem:** A company adopts a Choose Your Own Device (CYOD) model where employees purchase devices from a retail store and need to enroll them into corporate management.

**Solution:** Use **Microsoft Entra join with automatic MDM enrollment** via Microsoft Intune.

**Steps:**

1. Ensure the organization has **Microsoft Entra ID P1 or P2** and **Microsoft Intune** licensed.
2. Configure **automatic MDM enrollment** in the Intune portal:
   - Go to **Microsoft Intune admin center** → **Devices** → **Enrollment** → **Automatic Enrollment**
   - Set MDM user scope to the appropriate group or **All**
3. On the employee's new device, during Windows OOBE (Out-of-Box Experience):
   - Select **Set up for an organization**
   - Sign in with the employee's **Microsoft Entra credentials**
4. The device automatically joins Microsoft Entra ID and enrolls in Intune MDM.
5. Compliance policies, apps, and configurations are pushed automatically.
6. If compliance conditions are not met, enrollment is denied.

**Sources:**
- [Set up automatic enrollment for Windows devices](https://learn.microsoft.com/en-us/mem/intune/enrollment/windows-enroll)
- [Microsoft Entra join — What is it?](https://learn.microsoft.com/en-us/azure/active-directory/devices/concept-azure-ad-join)
- [Microsoft Intune overview](https://learn.microsoft.com/en-us/mem/intune/fundamentals/what-is-intune)

---

### ✅ Scenario 4: BYOD — Employees Using Personal Devices to Access Corporate Resources

**Problem:** Remote employees want to access corporate email, SharePoint, and Teams using personal Windows devices.

**Solution:** Use **MDM enrollment for user-owned (BYOD) devices** via Microsoft Entra ID and Microsoft Intune.

**Steps:**

1. User opens **Settings → Accounts → Access work or school → Connect**.
2. User enters their **Microsoft work account (Entra ID credentials)**.
3. Windows prompts the user to accept organizational policies (MDM enrollment terms).
4. If accepted, the device enrolls in Intune MDM.
5. Intune enforces compliance policies (e.g., requiring BitLocker, PIN, OS version).
6. Compliant devices are granted access to corporate resources secured by Microsoft Entra ID (Conditional Access).
7. Non-compliant devices are blocked from accessing corporate applications.

> **Note:** Users retain the option to remove their work account at any time, which unenrolls the device from MDM.

**Sources:**
- [BYOD and Microsoft Intune](https://learn.microsoft.com/en-us/mem/intune/enrollment/device-enrollment)
- [Conditional Access in Microsoft Entra ID](https://learn.microsoft.com/en-us/azure/active-directory/conditional-access/overview)
- [Add a work or school account to Windows](https://support.microsoft.com/en-us/windows/add-or-remove-accounts-on-your-pc-104dc19f-6430-4b49-6a2b-e4dbd1dcdf32)

---

### ✅ Scenario 5: Deploying Kiosk Devices (e.g., Digital Signage, Self-Service Terminals)

**Problem:** A retail company needs to deploy Windows devices in kiosk mode (single-app) at multiple store locations, with no centralized imaging infrastructure.

**Solution:** Use **Windows Configuration Designer** with the **Kiosk wizard** to create a provisioning package.

**Steps:**

1. Open **Windows Configuration Designer** and create a new project using the **Kiosk wizard**.
2. Configure:
   - **Set up device:** Assign device names and remove preinstalled software
   - **Set up network:** Configure Wi-Fi SSID and credentials
   - **Account management:** Create a local administrator account
   - **Add applications:** Include the kiosk app
   - **Configure kiosk account and app:** Create a local kiosk user and specify the app to run
   - **Configure kiosk common settings:** Enable tablet mode, configure shutdown/timeout behavior
3. Export the package as a signed `.ppkg` file.
4. Apply to each kiosk device via USB or network share.

**Sources:**
- [Set up a kiosk on Windows 11](https://learn.microsoft.com/en-us/windows/configuration/kiosk-single-app)
- [Windows Configuration Designer kiosk settings](https://learn.microsoft.com/en-us/windows/configuration/provisioning-packages/provisioning-create-package)

---

### ✅ Scenario 6: VDA — Activating Windows Enterprise on Azure VMs for Remote Workers

**Problem:** An organization wants remote workers to use Azure Virtual Desktop (AVD) with full Windows Enterprise features, without managing on-premise KMS infrastructure.

**Solution:** Use **VDA Subscription Activation** with **Inherited Activation** on Azure-hosted VMs.

**Steps:**

1. Ensure host machines (physical or VM) are licensed with **Windows Enterprise E3/E5/A3/A5**.
2. Ensure VMs are:
   - Running **Windows 11 Pro**
   - **Generation 1** VMs
   - Joined to AD DS or Microsoft Entra ID
   - Hosted in **Microsoft Azure** (a Qualified Multitenant Hoster)
3. When a licensed user creates a new Windows VM on their licensed Windows host, the VM **automatically inherits** the activation state (Inherited Activation).
4. Enterprise features become available on the VM without additional configuration.

**Sources:**
- [Windows Subscription Activation for VDA](https://learn.microsoft.com/en-us/windows/deployment/windows-10-enterprise-subscription-activation)
- [Azure Virtual Desktop overview](https://learn.microsoft.com/en-us/azure/virtual-desktop/overview)
- [Inherited Activation](https://learn.microsoft.com/en-us/windows/deployment/windows-10-enterprise-subscription-activation#inherited-activation)

---

## 6. Tools and Resources Used

| Tool / Resource | Description | Link |
|----------------|-------------|------|
| **Windows Configuration Designer (WCD)** | GUI tool for creating provisioning packages; included in Windows ADK | [Download Windows ADK](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install) |
| **Windows Assessment and Deployment Kit (Windows ADK)** | Suite of tools for Windows deployment, including WCD | [Windows ADK overview](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install) |
| **Microsoft Entra ID (Azure AD)** | Cloud identity and access management service | [What is Microsoft Entra ID?](https://learn.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-whatis) |
| **Microsoft Intune** | Cloud-based MDM and MAM solution | [Microsoft Intune overview](https://learn.microsoft.com/en-us/mem/intune/fundamentals/what-is-intune) |
| **Key Management Service (KMS)** | Volume activation role service for enterprise networks | [KMS activation planning](https://learn.microsoft.com/en-us/windows/deployment/volume-activation/plan-for-volume-activation-client) |
| **Multiple Activation Key (MAK)** | Product key for activating a set number of computers | [MAK activation](https://learn.microsoft.com/en-us/windows/deployment/volume-activation/plan-for-volume-activation-client) |
| **Active Directory Domain Services (AD DS)** | Directory service for domain-joined environments | [AD DS overview](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) |
| **Add-ProvisioningPackage** | PowerShell cmdlet to apply a provisioning package | [PowerShell reference](https://learn.microsoft.com/en-us/powershell/module/provisioning/add-provisioningpackage) |
| **Microsoft 365 Admin Center** | Portal for managing licenses, users, and subscriptions | [admin.microsoft.com](https://admin.microsoft.com) |
| **Microsoft Intune Admin Center** | Portal for device management, enrollment, and compliance | [intune.microsoft.com](https://intune.microsoft.com) |
| **Partner Center / CSP Channel** | Cloud Solution Provider platform for purchasing Windows E3 subscriptions | [Partner Center](https://partner.microsoft.com/en-us/dashboard) |
| **Azure Virtual Desktop (AVD)** | Microsoft's cloud-hosted virtual desktop solution (relevant for VDA) | [AVD overview](https://learn.microsoft.com/en-us/azure/virtual-desktop/overview) |
| **Windows Subscription Activation** | Microsoft Docs reference for Subscription Activation details | [Docs link](https://learn.microsoft.com/en-us/windows/deployment/windows-10-enterprise-subscription-activation) |

---
