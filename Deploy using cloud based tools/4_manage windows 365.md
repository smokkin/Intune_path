# Managing, Configuring, and Administering Windows 365 Cloud PCs

A complete deployment and administration reference manual for managing Windows 365 Cloud PCs in small, medium, and large enterprise environments.

---

## 1. Explore Windows 365

### Overview
Windows 365 is a cloud-based service that automatically generates a new Windows virtual machine called **Cloud PCs** for end users. Each Cloud PC is assigned to an individual user, providing them with a dedicated Windows device. By using Windows 365, you can experience the benefits of Microsoft 365, which include increased productivity, enhanced security, and seamless collaboration. 

Windows 365 is available in two editions:
* **Windows 365 Business:** Made specifically for smaller companies (up to 300 users) who want ready-to-use Cloud PCs with simple management options. There are no licensing prerequisites to set up Windows 365 Business. There are no dependencies on Azure or Active Directory. Purchases are made through the Microsoft 365 admin center or the Windows 365 product site.
* **Windows 365 Enterprise:** Designed for larger organizations seeking unlimited users to create Cloud PCs. It offers the flexibility to build custom Cloud PCs using your own device images, provides additional management options, and seamlessly integrates with Microsoft Intune. Windows 365 Enterprise utilizes Microsoft Entra ID and AD DS domains to enhance its functionality.

### Edition Comparisons

#### General Comparisons
| Capability | Windows 365 Business | Windows 365 Enterprise |
| :--- | :--- | :--- |
| **Domain Join** | Microsoft Entra join without Azure Virtual Network (VNet) Support | Microsoft Entra join without VNet support.<br>Microsoft Entra join with VNet support.<br>Hybrid Microsoft Entra ID with VNet support. |

#### Purchasing and Licensing Comparisons
| Capability | Windows 365 Business | Windows 365 Enterprise |
| :--- | :--- | :--- |
| **Purchase Channels** | Web direct, self-service, Cloud Solution Provider (CSP). | Web direct, Enterprise Agreements (EA), CSP. |
| **License Assignment** | Microsoft 365 Admin Center or the Microsoft Entra admin center. | Microsoft 365 Admin Center or the Microsoft Entra admin center. |
| **Licensing Requirements** | No licensing pre-requirements to buy and deploy Windows 365 Business. Other features (like device management) can be used if users are licensed for Microsoft Intune. | Each user must be licensed for Windows 10 or 11 Enterprise, Microsoft Intune, and Microsoft Entra ID P1. |
| **Networking Costs** | Outbound data/month is based on the RAM of the Cloud PC:<br>- 2-GB RAM = 12-GB outbound data<br>- 4-GB or 8-GB RAM = 20-GB outbound data<br>- 16-GB RAM = 40-GB outbound data<br>- 32-GB RAM = 70-GB outbound data<br>Data bandwidth may be restricted when these levels are exceeded. | When providing a network, Networking goes through the customer's Azure VNet and isn't included in the license. Azure bandwidth pricing applies.<br>If using a Microsoft-hosted network, the same charges as described in Windows 365 Business networking charges apply. |
| **User Limits** | Capped to 300 users per tenant. | No user cap per tenant. |

#### Administration Comparisons
| Capability | Windows 365 Business | Windows 365 Enterprise |
| :--- | :--- | :--- |
| **Provisioning** | Provisioning is simplified and uses default configurations. Cloud PCs are automatically provisioned with a standard image after a Cloud PC license is assigned. | Provisioning is configurable and customizable to the needs of the organization. Admins select the network, configure user permissions (local admin or not), and assign the policy to a Microsoft Entra group. Cloud PCs are then provisioned by using standard gallery images or custom images. |
| **Policy Management** | Not Supported. | Group Policy Objects (GPO) and Intune MDM are supported. |
| **Application Deployment** | Supported only if you have an Intune license. | Supported. |
| **Windows Updates** | Default Windows Update for Business settings are configured for users. With an Intune license, these settings can be edited. | Can be managed by using Microsoft Intune. |
| **Device Management** | Device management is limited to assigning and unassigning of Cloud PC licenses in the Microsoft Admin Center. Some device management is possible in Microsoft Intune if you have an Intune license but Cloud PCs won't be visible in the Windows 365 blade. | Microsoft Intune admin center options, including image management, link and access on-premises resources, granular targeting of policies, resizing Cloud PCs, other user experience settings, and all the policy-based management options available to physical devices. |
| **Monitoring** | Not supported. | Endpoint Analytics reporting and monitoring, service health, and operational health alerts. |
| **Troubleshooting** | Not supported. | Microsoft Intune troubleshooting including the Troubleshooting blade, device management actions, and reprovisioning of Cloud PCs to their initial state. |
| **Partner/Programmatic Access** | Not supported. | Partners can manage Cloud PCs through Microsoft 365 Lighthouse or restful web APIs (Graph) to support Managed Service Provider tooling for up to 300 users. |
| **Universal Print** | Not supported. | Supported. |

#### End-User Comparisons
| Capability | Windows 365 Business | Windows 365 Enterprise |
| :--- | :--- | :--- |
| **Management** | Users can restart, reset, rename, and troubleshoot their Cloud PCs on the Windows 365 homepage. | Users can restart, rename, and troubleshoot their Cloud PCs on the Windows 365 homepage. |
| **Role** | By default, each user is a Standard User on their Cloud PC. (Local Administrator permissions can be granted via remote management actions or organizational default configuration). | By default, each user is assigned a standard user role on their Cloud PC. The administrator has the capability to modify this role within the Microsoft Intune admin center. |
| **Access** | Users can access their Cloud PC at windows365.microsoft.com or by using Microsoft Remote Desktop or the Windows 365 app. | Users can access their Cloud PC at windows365.microsoft.com or by using Microsoft Remote Desktop or the Windows 365 app. |
| **Platform** | Any platform that supports Microsoft Remote Desktop clients or the Windows 365 app. | Any platform that supports Microsoft Remote Desktop clients or the Windows 365 app. |

#### Security Comparisons
| Capability | Windows 365 Business | Windows 365 Enterprise |
| :--- | :--- | :--- |
| **Conditional Access** | Conditional Access policies can be deployed only by using Microsoft Entra ID with a Microsoft Entra ID P1 license. | Conditional Access policies can be deployed by using the Microsoft Intune admin center or Microsoft Entra ID. |
| **Per-User MFA** | Only MFA using Microsoft Entra Conditional Access is supported. Legacy per-user MFA isn't supported. | Legacy per-user MFA is supported for user connections to Microsoft Entra hybrid joined Cloud PCs. It's not supported for user connections to Microsoft Entra joined Cloud PCs. |
| **Security Baselines** | Not supported. | Dedicated Security Baselines can be edited and deployed by using Microsoft Intune. |
| **Microsoft Defender for Endpoint** | Supported if the customer separately has the requisite E5 license. | Integration with Defender for Endpoint. If the customer has an E5 license, all Cloud PCs respond to Defender for Endpoint policies and show up in MDE dashboards. |

> **Access Link:** Users can navigate to [https://windows365.microsoft.com](https://windows365.microsoft.com) to access their Cloud PCs. Remote Desktop clients are available for Windows, macOS, iOS/iPadOS, and Android.

<img width="340" height="358" alt="image" src="https://github.com/user-attachments/assets/3323748a-ffef-4f43-9b53-3f3627dfc578" />

---

## 2. Configure Windows 365

The configuration of Windows 365 mostly takes place inside the **Microsoft Intune admin center**.

### Assign Licenses to Users
To allocate licenses via the Microsoft Entra admin center:
1. Sign in to the **Microsoft Entra admin center** with a license administrator account.
2. Select **Licenses** to open the management layout.
3. Select a user or group, and use the **Select** button at the bottom of the page to confirm your selection.
4. Under *All products*, select **Windows 365** and click **Assign** at the top of the page.
5. On the *Assign license* page, select **Users and groups** to open a list of users and groups.
6. Select **Assign** at the bottom of the page.

### Create an Azure Network Connection (ANC)
An Azure network connection allows Cloud PCs to join the organization's domain and access on-premises assets.

1. Sign in to the **Microsoft Intune admin center** with an account that is an Intune Administrator in Microsoft Entra ID and has owner permissions to the virtual network in the Azure subscription.
2. Select **Devices** > **Windows 365** (under *Provisioning*) > **Azure network connection** > **Create connection**.
3. On the **Network details** page, enter a **Name** for the connection (must be unique within the tenant).
   <img width="1300" height="910" alt="image" src="https://github.com/user-attachments/assets/69fcca2c-ab14-4276-9b62-e525c3a7c581" />
5. Select a **Subscription** and **Resource group** that contains your Cloud PC resources.
6. Select a **Virtual network** and **Subnet**, then select **Next**.
7. On the **AD domain** page, provide the **AD domain name**, **Organizational unit** (optional), and an **AD domain username and password** for the service account used for connecting the Cloud PCs to your Active Directory domain.
8. Select **Next**.
9. On the **Review + Create** page, select **Create**.

### Configure a Custom Device Image (Optional)
Windows 365 provides a built-in gallery of monthly updated Windows Enterprise images:
* **Images with preinstalled Microsoft 365 Apps:** Standard system optimization + Teams optimizations preinstalled.
* **Images with OS optimizations:** Enterprise images fine-tuned for high performance in virtual environments.

To add a custom image (up to 20 custom generalized images can be uploaded):
1. Add your custom generalized image to your Azure subscription.
2. Sign in to the **Microsoft Intune admin center**.
3. Go to **Devices** > **Windows 365** > **Device images** > **Add**.
4. Select an image from the available list, and select **Add**.

### Create Provisioning Policies
Provisioning policies define how Cloud PCs are structured and assigned to user security groups.

1. Sign in to the **Microsoft Intune admin center**, select **Devices** > **Windows 365** > **Provisioning policies** > **Create policy**.
   <img width="900" height="549" alt="image" src="https://github.com/user-attachments/assets/637d2643-1d43-4870-b89c-dfd7a19c1fb7" />
3. On the **General** page, enter a **Name** for the new policy.
4. For **On-premises network connection**, select the network connection to use for this policy, then select **Next**.
5. On the **Image** page, for **Image type**, select one of the following options:
    * **Gallery image:** Choose **Select** > select an image from the gallery > **Select**.
    * **Custom image:** Choose **Select** > select an image from the list > **Select**.
6. Select **Next**.
7. On the **Assignments** page, choose **Select groups** > choose the Microsoft Entra user security groups or Microsoft 365 Groups you want this policy assigned to, choose **Select** and then **Next**.
8. On the **Review + create** page, select **Create**.

> [!NOTE]
> It can take up to 60 minutes for the policy creation process to complete, depending on when the Microsoft Entra Connect Sync last happened.

### Configure and Apply Device and App Configuration Profiles
Once the initial provisioning setup is finished, Cloud PCs are configured similarly to a physical device. You create configuration profiles and assign apps to users, devices, and groups within Intune.

---

## 3. Administer Windows 365

### Remote Actions
Standard physical device actions available for Cloud PCs include: *Restart, Sync, Rename, Quick Scan, Full Scan, and Update Windows Defender*.

Three remote actions are unique to Cloud PCs:
1.  **Reprovisioning**
2.  **Resize**
3.  **Collect diagnostics**

### Reprovisioning
The Reprovision remote action resets the Cloud PC to its initial state.

> [!WARNING]
> User data, applications, and customizations are not automatically guaranteed to be preserved—erasure depends on the specific device setup and configuration policies. Always back up critical data before executing a reprovision.

#### Common Use Cases:
* Testing different Cloud PC configuration states.
* Resolving a provisioned Cloud PC that is misbehaving.
* Providing an end-user with a completely fresh environment.

#### Steps to Reprovision:
1. Sign in to the **Microsoft Intune admin center**, select **Devices** > **All Devices** > choose a Cloud PC device > select **Reprovision**.
   <img width="900" height="550" alt="image" src="https://github.com/user-attachments/assets/670e4500-d6bd-447e-b7a5-d5f9e5f5110d" />
3. In the confirmation box, select **Yes**. The process will begin automatically.
4. Once the new Cloud PC is created, Windows 365 sends access information directly to the user.

### Resize a Cloud PC
The Resize action lets you upgrade a Cloud PC's RAM, CPU, and storage configurations.

#### Requirements & Constraints:
* The administrator must hold either **Global Admin** or **Intune Service Admin** Microsoft Entra roles.
* The Cloud PC status must be **Provisioned**.
* The user must be signed out with all work saved, as resizing forces a device restart and immediately disconnects active sessions.
* **Storage Limitation:** You can only *increase* OS disk storage. You *cannot* decrease the OS disk storage. Downsizing options containing lower disk allocations will be grayed out in the console.

#### Steps to Resize:
1. Sign in to the **Microsoft Intune admin center**, select **Devices** > **All Devices** > choose a device > select **Resize**.
   <img width="900" height="550" alt="image" src="https://github.com/user-attachments/assets/e65ee585-c4d4-42a5-b951-d30642f8744d" />
3. Review the list of available target SKUs (RAM/vCPU upgrades). Select your targeted tier.
4. Select **Resize**.

> [!IMPORTANT]
> The system prioritizes paid licenses when you have a mix of paid and trial licenses. If your license inventory lacks the required SKU tier, the resizing operation will fail. You must add matching licenses via the Microsoft Admin Center before retrying.

---

## Tools and Resources Used
* [Microsoft Intune Admin Center](https://intune.microsoft.com)
* [Microsoft Entra Admin Center](https://entra.microsoft.com)
* [Microsoft 365 Admin Center](https://admin.microsoft.com)
* [Windows 365 User Web Portal](https://windows365.microsoft.com)
* [Azure Bandwidth Pricing Information](https://azure.microsoft.com/pricing/details/bandwidth/)

---

## Source Notes
* **Missing Details:** The original text mentions the "Collect diagnostics" remote action for Cloud PCs but does not provide step-by-step guidance or clear instructions on how or where the diagnostic logs are saved or retrieved. 
* **In Development Reference:** The documentation notes that for other domain join architectures beyond the standard ones listed, administrators should check the "In development for Windows 365 Enterprise" section, without embedding the direct steps here.

---

## Confirmed and Workable Applied Scenarios

### Applied Scenario 1: Transitioning Local Admin Privileges on Business Cloud PCs
**Context:** A small enterprise running Windows 365 Business wants to upgrade three specific users to have Local Administrator rights to test legacy developer tools, while ensuring all future newly created Cloud PCs revert back to the safe "Standard User" configuration default.

#### Step-by-Step Solution:
1. Log in to the [Microsoft 365 Admin Center](https://admin.microsoft.com).
2. To ensure all future devices use standard restricted rights, change the organization default settings: Navigate to your organizational management parameters for Windows 365, locate the **User Type** option, and select **Standard User**.
3. To elevate the three existing users, open the user management list within the Windows 365 Business panel.
4. Select the user's specific Cloud PC, find the **Remote management actions** options, and execute the command to grant **Local Administrator** permissions.
5. Ask the users to log out and back in to apply the permission group updates.

*Source Link Verification:* [Getting started with Windows 365 Business and Cloud PCs](https://learn.microsoft.com/en-us/windows-365/business/get-started-windows-365-business)

---

### Applied Scenario 2: Troubleshooting a Failed Enterprise Cloud PC Resize due to Missing Licenses
**Context:** An enterprise administrator attempts to scale up a remote worker's Cloud PC from 2 vCPU / 8 GB RAM to 4 vCPU / 16 GB RAM via the Intune portal, but the operation fails or the target option is unavailable.

#### Step-by-Step Solution:
1. Direct the tenant procurement manager to sign in to the [Microsoft 365 Admin Center](https://admin.microsoft.com).
2. Navigate to **Billing** > **Purchase services**. Search for the specific matching upgraded Enterprise SKU tier (*Windows 365 Enterprise 4 vCPU, 16 GB RAM, 128 GB Storage*).
3. Purchase the required quantity of matching licenses.
4. Inform the end-user to save all active work files and close applications to prevent data loss.
5. In the **Microsoft Intune admin center**, locate **Devices** > **All Devices** > choose the user's provisioned Cloud PC.
6. Click **Resize**. The newly acquired license variant will now be active in the inventory pool. Select it and confirm the action by clicking **Resize**. The device will restart and apply the hardware update.

*Source Link Verification:* [Device management overview for Cloud PCs](https://learn.microsoft.com/en-us/windows-365/enterprise/device-management-overview)

---

