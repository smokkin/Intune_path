# Harden Endpoints and Monitor Security with Microsoft Intune and Defender for Endpoint

**Roles**: Administrator, Security Engineer  
**Products**: Microsoft Intune, Microsoft Defender for Endpoint, Windows 11  

Strengthen endpoint security by deploying Microsoft security baselines, onboarding devices to Microsoft Defender for Endpoint, and configuring attack surface reduction rules. Monitor security posture and respond to threats using integrated security operations tools.

---

## Learning Objectives

By the end of this module, you are able to:

- Deploy security baseline templates to enforce Microsoft's recommended security configurations
- Onboard Windows devices to Microsoft Defender for Endpoint for advanced threat protection
- Configure attack surface reduction (ASR) rules to prevent common attack vectors
- Enable exploit protection and controlled folder access to defend against ransomware
- Monitor security posture and investigate security incidents using Defender and Intune reports
- Troubleshoot common security baseline and Defender onboarding issues

---

## Prerequisites

- Administrative access to Microsoft Intune admin center
- Administrative access to Microsoft Defender portal
- Microsoft Defender for Endpoint Plan 1 or Plan 2 license

---

## Introduction – Contoso's Scenario

Contoso's security team has identified several priorities for improving endpoint protection:

- **Harden security configurations**: Deploy Microsoft's recommended security settings across all Windows devices to reduce attack surface and ensure compliance with security best practices.
- **Advanced threat detection**: Onboard devices to Microsoft Defender for Endpoint to detect and respond to advanced threats, malware, and suspicious behavior.
- **Prevent common attacks**: Block known attack vectors like malicious Office macros, script-based exploits, and credential theft using attack surface reduction rules.
- **Ransomware protection**: Enable controlled folder access to prevent unauthorized applications from encrypting critical user data.
- **Security visibility**: Monitor security posture, track baseline compliance, and investigate security incidents using integrated dashboards and reports.

### Three Key Endpoint Security Capabilities

1. **Security baselines**: Pre-configured security templates based on Microsoft security recommendations that harden Windows, Microsoft Edge, and Microsoft Defender settings.
2. **Microsoft Defender for Endpoint**: Advanced threat protection with endpoint detection and response (EDR), behavioral analysis, and automated investigation and remediation.
3. **Attack surface reduction (ASR)**: Proactive defense mechanisms including ASR rules, exploit protection, and controlled folder access that prevent exploitation before threats execute.

---

## Unit 1 – Harden Device Security with Security Baselines

Security baselines are pre-configured groups of Windows settings recommended by Microsoft security teams to secure devices. These templates apply proven security configurations developed by security engineers and industry experts based on real-world threats, security research, and compliance requirements.

**Key characteristics**:
- Pre-configured templates with hundreds of settings
- Regularly updated for new threats
- Customizable for business needs
- Compliance-ready (CIS, NIST)
- Built-in conflict resolution

### Available Security Baselines in Intune

| Baseline Type | Primary Coverage | Key Settings |
|---|---|---|
| **Windows 10 and later** | Comprehensive OS security (most important baseline) | Account policies, firewall, BitLocker, Defender Antivirus, Application Guard, Exploit Guard, SmartScreen, Device Guard |
| **Microsoft Edge** | Browser security and privacy | Safe browsing, tracking prevention, extension controls, authentication, content restrictions |
| **Microsoft Defender for Endpoint** | Advanced threat protection (requires Defender Plan 2) | Attack surface reduction rules, exploit protection, network protection, controlled folder access, cloud-delivered protection |
| **Windows 365** | Cloud PC security | Virtual desktop-specific settings |

> **Versioning**: Baselines are versioned by Windows release (e.g., "21H1", "22H2", "23H2"). Always deploy the latest version compatible with your devices.

### Deploy Security Baselines

**Navigation path**: Intune admin center > **Endpoint security** > **Security baselines**

#### Deployment Strategy

| Decision | Recommendation | Rationale |
|---|---|---|
| **Baseline selection** | Deploy Windows baseline first, then Edge and Defender | Windows baseline provides foundation; others add specific protections |
| **Version** | Always use latest version for your OS | New versions include protections for emerging threats and new OS features |
| **Customization** | Keep 90%+ defaults; customize only for clear business needs | Microsoft settings represent security expertise and real-world research |
| **Assignment** | Assign to broad groups ("All Corporate Devices"); exclude test devices only | Ensures comprehensive coverage and consistent security posture |
| **Timeline** | Settings apply at next check-in (~8 hours); some require reboot | Plan for BitLocker and Device Guard requiring restarts |

**Common customizations (when justified)**:
- Password length: 12 characters instead of 14 for policy alignment
- BitLocker startup: TPM-only instead of TPM+PIN for user experience
- Defender sample submission: Adjust for data residency regulations

### Key Baseline Settings

| Setting Category | Key Configurations | Security Benefit |
|---|---|---|
| **Account Security** | Password: 14 characters, complexity required, 24 history, 60-90 day max age; Lockout: 5-10 attempts | Prevents brute-force attacks and credential reuse |
| **Defender Antivirus** | Real-time protection, cloud-delivered protection, automatic sample submission, removable drive scanning, PUA protection, ASR rules | Continuous threat monitoring and rapid response |
| **Firewall** | All network profiles enabled (domain, private, public), stealth mode, connection logging | Blocks unauthorized network access and tracks threats |
| **BitLocker** | Required encryption, XTS-AES 256-bit, TPM+PIN startup, cloud recovery key backup, all drives encrypted | Protects data at rest from physical theft |
| **Application Control** | Application Guard enabled, clipboard/printing blocked from isolated browser, SmartScreen required | Isolates untrusted content and blocks malicious downloads |

### Monitor Compliance

**Navigation**: Intune admin center > Endpoint security > Security baselines > Select baseline > Overview/Device status
<img width="703" height="958" alt="image" src="https://github.com/user-attachments/assets/7c429862-90b3-456f-845f-ae524fe4d2c9" />

**Deployment metrics tracked**:
- Devices assigned
- Succeeded
- Error
- Conflict
- Not applicable

Per-setting details show: setting name, current value, baseline value, and compliance status for each device.
<img width="1155" height="843" alt="image" src="https://github.com/user-attachments/assets/e13bcb67-c6fa-45f6-9341-23502e9ffc6e" />

### Policy Precedence

When multiple policies configure the same setting, **security baselines have lower priority** than:
- Device configuration profiles
- Administrative templates
- Endpoint security policies

**Resolution strategy**: Use baselines for foundational settings, override with specific policies for customizations, avoid redundant configurations.

### Best Practices – Security Baselines

- **Deploy latest version**: Always use the most recent baseline for your Windows version
- **Minimize customizations**: Modify only settings with clear business justification
- **Test before production**: Deploy to pilot group to identify compatibility issues
- **Monitor weekly**: Review compliance reports to catch configuration drift
- **Update promptly**: Adopt new baseline versions within 60 days
- **Layer policies**: Use baselines for foundation, endpoint security policies for specific controls
- **Document changes**: Maintain records of customizations for audit compliance

### Troubleshoot Security Baselines

| Issue | Cause | Resolution |
|---|---|---|
| Baseline not applying | Device not checking in | Force sync in Company Portal or check enrollment status |
| Settings showing conflict | Other policies override baseline | Review device configuration policies and adjust priorities |
| BitLocker setting fails | TPM not enabled or compatible | Verify hardware TPM presence and enable in BIOS/UEFI |
| Firewall settings error | Third-party firewall installed | Remove third-party firewall or exclude settings from baseline |
| Application Guard fails | Hardware requirements not met | Verify Hyper-V support and nested virtualization capability |
| Password policy not enforced | Domain policy takes precedence | Ensure devices use Microsoft Entra Join (not hybrid) or adjust domain GPO |

---

## Unit 2 – Enable Advanced Threat Protection with Microsoft Defender for Endpoint

Microsoft Defender for Endpoint is a comprehensive endpoint security platform providing advanced threat protection, EDR, threat and vulnerability management, and automated investigation and remediation. When integrated with Microsoft Intune, it delivers seamless onboarding and centralized security management.

**Key capabilities**:
- EDR (continuous monitoring and behavioral analysis)
- Threat and vulnerability management (risk-based prioritization)
- Attack surface reduction (ASR rules, exploit protection)
- Next-generation protection (AI-powered antivirus)
- Automated investigation and remediation
- Secure Score for Devices

### Defender for Endpoint Licensing

| Feature | Plan 1 (Microsoft 365 E3) | Plan 2 (Microsoft 365 E5) |
|---|---|---|
| Next-generation protection (antivirus, anti-malware) | ✅ | ✅ |
| Attack surface reduction rules | ✅ | ✅ |
| Device inventory | ✅ | ✅ |
| Centralized management via Intune | ✅ | ✅ |
| Basic reporting | ✅ | ✅ |
| Endpoint detection and response (EDR) | ❌ | ✅ |
| Automated investigation and remediation | ❌ | ✅ |
| Threat and vulnerability management | ❌ | ✅ |
| Threat hunting (KQL) | ❌ | ✅ |
| File and URL analysis | ❌ | ✅ |
| API access | ❌ | ✅ |

### Onboard Devices to Defender for Endpoint

**Recommended method**: Create EDR policy in Intune admin center > **Endpoint security** > **Endpoint detection and response**

#### Configuration Decisions

| Setting | Recommendation | Impact |
|---|---|---|
| **Sample sharing** | Enable | Shares suspicious files for cloud analysis; disabling reduces detection accuracy by 20-30% |
| **Telemetry frequency** | Expedite for high-security environments; Normal for general corporate | Expedite: 2-5 min detection delay, ~50MB/day; Normal: 10-15 min delay, ~20MB/day |
| **Assignment** | All corporate devices except third-party EDR devices | Full coverage prevents unmonitored breach entry points |

> **Onboarding timeline**: Devices appear in Defender portal within 2-4 hours after policy application.

### Verify Device Onboarding

**Microsoft Defender portal** (`https://security.microsoft.com`):  
Navigate to **Assets** > **Devices** — verify:
- **Health state**: Active
- **Onboarding status**: Onboarded
- **Sensor health**: Healthy

**Intune admin center**:  
Navigate to **Endpoint security** > **Endpoint detection and response** > Select policy > **Device status: Succeeded**

**On device**:  
Windows Security app > Settings > About — verify **Management by: Microsoft Defender for Endpoint**

### EDR Capabilities

#### Threat Detection

Defender generates alerts for:
- Malware (trojans, ransomware, rootkits)
- Suspicious behavior (credential dumping, lateral movement)
- Exploitation attempts (memory corruption, script attacks)
- File/registry manipulation
- Network anomalies

**Attack investigation** — each alert includes:
- Attack story timeline
- Process tree relationships
- File analysis
- User/device context
- Indicators of compromise (IOCs)

### Automated Investigation and Remediation (AIR)

**Process**:
1. Alert triggers investigation
2. AI collects evidence (files, processes, registry, network)
3. Determines verdict
4. Recommends/executes remediation (quarantine files, stop processes, block IPs, isolate device)

**Automation levels**:
- **Full**: Auto-remediate
- **Semi** (default): Require approval
- **None**: Alerts only

**Configuration path**: Microsoft Defender portal > Settings > Endpoints > Advanced features > Automated investigation

### Threat and Vulnerability Management (TVM)

**Dashboard** (Defender portal > Vulnerability management):
- **Exposure score** (0-1000 risk scale)
- **Secure Score for Devices** (0-100%)
- Vulnerable device count
- Prioritized security recommendations
<img width="3305" height="1909" alt="image" src="https://github.com/user-attachments/assets/e141af6a-fc8b-4533-b501-d7f09dcce30a" />

**Recommendations include**:
- Install security updates
- Enable security features (BitLocker, ASR rules, Credential Guard)
- Remove vulnerable software
- Fix misconfigurations

Each recommendation shows: exposed devices, threats mitigated, exposure impact, and remediation options.

### Integration with Microsoft Intune

**Defender risk in compliance**:
1. Enable Defender connector: Intune > Endpoint security > Microsoft Defender for Endpoint > Connect
2. Create compliance policy requiring device risk score ≤ Medium/Low/Clear
3. High-risk devices marked noncompliant and blocked by Conditional Access

**Remediation actions from Intune** (Devices > All devices > Select device > Device actions):
- Sync
- Run antivirus scan
- Update definitions
- Isolate device
- Collect investigation package

### Monitor Defender for Endpoint

#### Microsoft Defender Portal Dashboards

| Dashboard | Purpose | Key Metrics |
|---|---|---|
| **Incidents & alerts** (Security operations) | Active incidents requiring investigation | Active incidents, high severity alerts, incident trends over time |
| **Threat analytics** (Threat intelligence) | Emerging threat campaigns and threat actor profiles | Threat campaigns, threat actor profiles, industry-specific threats |
| **Secure Score** (Posture management) | Overall security posture score | Security posture score, improvement actions, score comparison to similar orgs |
| **Device inventory** (Asset management) | All onboarded devices | Device health and risk levels, sensor status |

#### Intune Reporting

**Navigation**: Endpoint security > Microsoft Defender for Endpoint

- **Unhealthy endpoints**: Devices with sensor issues or communication failures
- **Endpoint detection and response profile status**: Onboarding policy deployment success

### Best Practices – Defender for Endpoint

- **Onboard all endpoints**: Windows desktops, servers, and virtual machines
- **Enable automated investigation**: Use semi or full automation to reduce SOC workload
- **Monitor exposure score**: Track weekly and implement TVM recommendations
- **Integrate with Conditional Access**: Automatically block high-risk devices
- **Deploy attack surface reduction**: Enable ASR rules and exploit protection
- **Review incidents weekly**: Establish SOC triage and investigation processes
- **Enable cloud-delivered protection**: Ensure real-time threat intelligence

---

## Unit 3 – Prevent Exploitation with Attack Surface Reduction

Attack surface reduction (ASR) capabilities prevent exploitation by blocking common attack techniques before malware executes or establishes persistence.

### ASR Components

| Component | Description |
|---|---|
| **Attack surface reduction rules** | Block specific malicious behaviors (Office macros, script execution, credential theft) |
| **Exploit protection** | Prevent memory corruption exploits using DEP, ASLR, and CFG |
| **Network protection** | Block access to malicious domains, phishing sites, and C2 servers |
| **Controlled folder access** | Protect important folders from ransomware by allowing only trusted apps to modify files |
| **Web protection** | Block access to websites hosting exploits, phishing content, or malware |

### Available ASR Rules

Microsoft provides 17 ASR rules covering different attack vectors:

| Rule Name | Blocks |
|---|---|
| **Block executable content from email client and webmail** | Outlook and webmail launching executables |
| **Block all Office applications from creating child processes** | Office apps spawning processes (common macro technique) |
| **Block Office applications from creating executable content** | Office apps writing .exe files to disk |
| **Block Office applications from injecting code into other processes** | Office process injection attacks |
| **Block JavaScript or VBScript from launching downloaded executable content** | Scripts running downloaded files |
| **Block execution of potentially obfuscated scripts** | PowerShell, cmd.exe, wscript suspicious scripts |
| **Block Win32 API calls from Office macros** | Office macros calling Win32 APIs |
| **Block executable files from running unless they meet a prevalence, age, or trusted list criterion** | Uncommon/new executables |
| **Use advanced protection against ransomware** | Suspicious ransomware behaviors |
| **Block credential stealing from Windows local security authority subsystem (lsass.exe)** | Credential dumping (Mimikatz, etc.) |
| **Block process creations originating from PSExec and WMI commands** | Remote execution tools (lateral movement) |
| **Block untrusted and unsigned processes that run from USB** | USB-based malware |
| **Block Office communication application from creating child processes** | Outlook, Teams, Skype launching processes |
| **Block Adobe Reader from creating child processes** | PDF reader exploits |
| **Block persistence through WMI event subscription** | WMI-based persistence |
| **Block rebooting machine in Safe Mode** | Ransomware evasion technique |
| **Block use of copied or impersonated system tools** | Legitimate tool name spoofing |

**Most commonly deployed rules (high value, low false positives)**:
- Block executable content from email client and webmail
- Block all Office applications from creating child processes
- Block Office applications from creating executable content
- Block credential stealing from lsass.exe
- Use advanced protection against ransomware
- Block execution of potentially obfuscated scripts

### ASR Rule Modes

| Mode | Behavior | Recommended Use |
|---|---|---|
| **Block** | Prevent the behavior | Production environments after testing |
| **Audit** | Log the behavior but don't block | Testing and validation phase (2-4 weeks) |
| **Disabled** | Rule not active | Temporarily disable problematic rules |
| **Warn** | Prompt user with option to bypass | Use cautiously — users may bypass legitimate protections |

### Deployment Strategy for ASR Rules

1. **Phase 1 – Audit mode (2-4 weeks)**: Enable all rules in audit mode to assess impact.
2. **Review audit logs**: Identify legitimate applications triggering rules.
3. **Create exclusions**: Exclude trusted apps (use sparingly — each exclusion weakens security).
4. **Phase 2 – Block mode**: Enable rules in block mode incrementally (start with high-value, low-impact rules).
5. **Monitor and adjust**: Review block events and refine exclusions.

### Configure ASR Rules via Intune

**Navigation**: Intune admin center > Endpoint security > Attack surface reduction > **Create policy**  
*(Windows platform, Attack Surface Reduction Rules profile)*

**Configuration approach**:

1. **Name policy descriptively** (e.g., `ASR Rules - Corporate Devices - Block Mode`)
2. **Enable high-priority rules in Block mode** (proven low false-positive rates):
   - Block executable content from email client and webmail
   - Block all Office applications from creating child processes
   - Block Office applications from creating executable content
   - Block credential stealing from lsass.exe
   - Use advanced protection against ransomware
3. **Enable medium-priority rules in Audit mode initially** (test for application conflicts):
   - Block execution of potentially obfuscated scripts (may affect PowerShell automation)
   - Block Win32 API calls from Office macros (may affect legacy business macros)
   - Block untrusted and unsigned processes from USB
4. **Add exclusions sparingly**: Use file paths for specific trusted applications; avoid broad file extension exclusions.
5. **Assign to device groups** and deploy.

### Exploit Protection

Memory corruption exploits rely on predictable memory layouts and unchecked function calls. Exploit protection mitigations randomize memory, validate control flow, and isolate processes.

#### System-Wide Mitigations (applied to all processes)

- **Data Execution Prevention (DEP)**: Marks memory regions as non-executable, preventing shellcode execution in data buffers.
- **Address Space Layout Randomization (ASLR)**: Randomizes memory locations, making exploit code unable to predict where to jump.
- **Control Flow Guard (CFG)**: Validates function call targets, blocking return-oriented programming (ROP) attacks.

#### Application-Specific Mitigations (for high-risk apps: browsers, Office, PDF readers)

- **Child process blocking**: Prevents exploited applications from spawning malicious processes.
- **Code integrity validation**: Ensures only signed code loads into memory.
- **Extension point blocking**: Stops DLL injection and thread hijacking techniques.

#### Configure Exploit Protection

**Navigation**: Intune admin center > Endpoint security > Attack surface reduction > **Create policy**  
*(Windows platform, Exploit Protection profile)*

**Configuration**: Enable all system-wide mitigations (DEP, ASLR, CFG, SEHOP). Add application-specific settings for Microsoft Edge, Office apps, Adobe Reader, and browsers with child process blocking and code integrity validation enabled.

### Controlled Folder Access (Ransomware Protection)

Controlled folder access implements a "default deny" approach for file modifications — stopping both known and zero-day ransomware variants.

#### How It Works

1. **Protects critical folders**: Documents, Pictures, Videos, Music, Desktop, Favorites (plus custom additions).
2. **Allows only trusted applications**: Microsoft-signed apps and explicitly trusted applications can modify protected folders.
3. **Blocks everything else**: All other applications — including ransomware — cannot modify, delete, or encrypt protected files.
<img width="703" height="946" alt="image" src="https://github.com/user-attachments/assets/89eff144-6ab8-42c4-ad22-fb89c76b84b0" />

**Example**: Ransomware infects a device via malicious email attachment and attempts to encrypt files in the Documents folder. Controlled folder access blocks the encryption attempt because the ransomware executable isn't on the trusted application list, protecting user data even though the malware successfully executed.

#### Configure Controlled Folder Access

**Navigation**: Intune admin center > Endpoint security > Attack surface reduction > **Create policy**  
*(Windows platform, Attack Surface Reduction Rules profile)*

**Configuration settings**:
- **Enable Controlled Folder Access**: Enabled
- **Additional protected folders**:
  ```
  %USERPROFILE%\Projects
  D:\CompanyData
  \\FileServer\SharedDocs
  ```
- **Trusted applications**: Add only third-party and line-of-business apps requiring write access (backup software, development tools). Microsoft-signed applications are automatically trusted.

> **Important**: Test in audit mode first (2-4 weeks) to identify legitimate applications that need trusted status before enforcing block mode.

### Network Protection

| Threat Type | Description |
|---|---|
| **Malicious domains** | Known bad sites from Microsoft threat intelligence |
| **Phishing sites** | Credential harvesting pages |
| **Exploit kits** | Sites hosting browser and plugin exploits |
| **Command-and-control (C2)** | Malware callback servers |

### Monitor Attack Surface Reduction

#### ASR Rule Reports
**Navigation**: Microsoft Defender portal > **Reports** > **Endpoints** > **Attack surface reduction rules**

- **ASR rule detections**: Count of blocked behaviors per rule
- **Top triggered rules**: Most active rules
- **Devices with detections**: Devices encountering blocks
- **Audit mode detections**: Behaviors that would be blocked if rule were in block mode

#### Exploit Protection Reports
**Navigation**: Microsoft Defender portal > **Reports** > **Endpoints** > **Exploit protection**

- **Exploits blocked**: Count of exploitation attempts prevented
- **Mitigation techniques triggered**: DEP, ASLR, CFG activations
- **Affected applications**: Apps targeted by exploit attempts

#### Controlled Folder Access Reports
**Navigation**: Microsoft Defender portal > **Reports** > **Endpoints** > **Controlled folder access**

- **Blocked attempts**: Unauthorized apps attempting to modify protected folders
- **Protected folders**: List of monitored locations
- **Blocked applications**: Apps denied access (review to identify false positives)

### Troubleshoot ASR and Controlled Folder Access

| Issue | Cause | Resolution |
|---|---|---|
| ASR rule blocking legitimate app | App behaves like malware | Add app to ASR exclusions (file path or process name) |
| PowerShell scripts fail | "Block obfuscated scripts" rule | Audit rule first, add script signing, or exclude trusted scripts |
| Office macros don't work | ASR rules block macro behaviors | Enable only trusted macros (digital signatures), add exclusions sparingly |
| Controlled folder access blocks backup app | App not on trusted list | Add backup app executable to trusted applications list |
| Custom app can't write to Documents | Controlled folder access enabled | Add app to trusted list or disable CFA for specific users |
| Network protection blocks internal site | Internal site flagged as malicious | Submit false positive to Microsoft or create custom indicator (allow) |

---

## Unit 4 – Exercise: Harden and Monitor Endpoints with Security Policies

**Estimated time**: 10 minutes

### Exercise Prerequisites

- Conditional Access and compliance policies configured in Intune
- Administrative access to Microsoft Intune admin center (`https://intune.microsoft.com`)
- Administrative access to Microsoft Defender portal (`https://security.microsoft.com`)
- At least one Windows 10/11 device enrolled in Intune
- Microsoft Defender for Endpoint Plan 1 or Plan 2 license (included in Microsoft 365 E3/E5)

---

### Task 1: Deploy Windows 11 Security Baseline

1. Sign in to the **Microsoft Intune admin center** (`https://intune.microsoft.com`)
2. Navigate to **Endpoint security** > **Security baselines**
3. Select **Security Baseline for Windows 10 and later**
4. Select **Create profile**
5. Select the latest baseline version (e.g., **Version 24H2**) and select **Create**
6. **Basics** tab:
   - Name: `Windows 11 Security Baseline - Corporate Devices`
   - Description: `Microsoft-recommended security settings for Windows 11 corporate devices`
   - Select **Next**
7. **Configuration settings** tab — apply these customizations on top of defaults:

   **Above Lock** section:
   - Allow voice activation from locked screen: **Block**

   **App Runtime** section:
   - Microsoft accounts optional for Windows Store apps: **Enable**

   **BitLocker** section:
   - Configure encryption methods: **XTS-AES 256-bit**

   **Browser** section (Microsoft Edge):
   - Prevent bypassing Windows Defender SmartScreen prompts: **Enable**
   - Prevent bypassing Windows Defender SmartScreen prompts for files: **Enable**

   **Device Guard** section:
   - Credential Guard: **Enable with UEFI lock**
   - Virtualization Based Security: **Enable**
   - Turn on Virtualization Based Security: **Enable**

   **Microsoft Defender** section:
   - Scan removable drives during full scan: **Enable**
   - Turn on real-time protection: **Enable**
   - Turn on behavior monitoring: **Enable**
   - Turn on cloud-delivered protection: **Enable**

   **Windows Defender Firewall** section:
   - Review domain, private, and public network profiles (all enabled by default)

   > **Note**: Scroll through all categories to familiarize yourself with the 100+ settings the baseline configures. Keep default values unless specified above.

   Select **Next**

8. **Scope tags**: Skip or assign if using RBAC
9. **Assignments** tab:
   - **Include**: All devices (or "Windows 11 Corporate Devices")
   - Select **Next**
10. **Review + create**: Review all settings > Select **Create**

**Result**: Windows security baseline deploys to targeted devices. Settings apply at next sync (within 8 hours, or force sync from Company Portal).

---

### Task 2: Onboard Devices to Microsoft Defender for Endpoint

1. Navigate to **Endpoint security** > **Endpoint detection and response**
2. Select **+ Create policy**
3. **Platform**: Windows 10, Windows 11, and Windows Server
4. **Profile**: Endpoint detection and response
5. Select **Create**
6. **Basics** tab:
   - Name: `Defender for Endpoint Onboarding - Windows Devices`
   - Description: `Onboards Windows 10/11 devices to Microsoft Defender for Endpoint for EDR and advanced threat protection`
   - Select **Next**
7. **Configuration settings** tab:
   - **Sample Sharing**: **Enable** — shares suspicious files with Microsoft for analysis in cloud sandbox
   - **Telemetry Reporting Frequency**: **Expedite** (for corporate networks) or **Normal** (for low-bandwidth sites)
   - **Endpoint detection and response**: **Enable**
   
   Select **Next**

8. **Scope tags**: Skip for this exercise
9. **Assignments** tab:
   - **Include**: All devices (or "Windows Corporate Devices")
   - **Exclude**: Devices with third-party EDR solutions
   - Select **Next**
10. **Review + create**: Validate settings > Select **Create**

**Result**: Devices begin onboarding to Microsoft Defender for Endpoint within 2-4 hours.

---

### Task 3: Verify Defender for Endpoint Onboarding

1. Wait 5-10 minutes for policy to sync (or force device sync in Company Portal)
2. Navigate to **Microsoft Defender portal**: `https://security.microsoft.com`
3. Navigate to **Assets** > **Devices**
4. Locate your test device and verify:
   - **Onboarding status**: Onboarded ✓
   - **Health state**: Active
   - **Sensor health**: Healthy
   - **Risk level**: Low (or Informational for newly onboarded devices)
5. Select the device name to view: device timeline, TVM recommendations, discovered vulnerabilities, installed software

**Troubleshooting — if device doesn't appear**:
- Verify device is online and connected to the internet
- Check Intune policy deployment status (Endpoint security > EDR policy > Device status)
- Force device sync from Company Portal app
- Wait up to 4 hours for initial onboarding to complete

---

### Task 4: Configure Attack Surface Reduction Rules

1. Navigate to **Endpoint security** > **Attack surface reduction**
2. Select **+ Create policy**
3. **Platform**: Windows 10, Windows 11, and Windows Server
4. **Profile**: Attack Surface Reduction Rules
5. Select **Create**
6. **Basics** tab:
   - Name: `ASR Rules - Corporate Devices`
   - Description: `Attack surface reduction rules blocking Office macros, email threats, credential theft, and ransomware behaviors`
   - Select **Next**
7. **Configuration settings** tab:

   **Office and email protection** (Block mode):
   - Block executable content from email client and webmail: **Block**
   - Block all Office applications from creating child processes: **Block**
   - Block Office applications from creating executable content: **Block**
   - Block Win32 API calls from Office macros: **Block**

   **Credential protection** (Block mode):
   - Block credential stealing from lsass.exe: **Block**

   **Ransomware protection** (Block mode):
   - Use advanced protection against ransomware: **Block**

   **Script protection** (Audit mode — test first):
   - Block execution of potentially obfuscated scripts: **Audit only**

   **Controlled Folder Access**:
   - Enable Controlled Folder Access: **Enabled**
   - Additional protected folders:
     ```
     %USERPROFILE%\Projects
     D:\CompanyData
     ```
   - Trusted applications: (Leave empty — Windows trusts Microsoft-signed apps by default)

   Select **Next**

8. **Exclusions**: None for this exercise. In production, add only validated business applications.
9. **Assignments**: Include **All devices** > Select **Next**
10. **Review + create**: Validate all rules > Select **Create**

**Result**: ASR rules deploy to devices and begin blocking malicious behaviors immediately upon next sync.

---

### Task 5: Test Controlled Folder Access (Optional)

1. On your test Windows device, open **File Explorer**
2. Navigate to the **Documents** folder
3. Create a test file: Right-click > **New** > **Text Document** > Name it `TestFile.txt`
4. Attempt to modify the file using an untrusted application
5. **Expected behavior**: Controlled folder access blocks the modification:
   - "Unauthorized changes blocked"
   - "Contact your IT administrator"
6. View the block event: Windows Security app > Virus & threat protection > **Protection history** > Controlled folder access event

> **Note**: Microsoft-signed apps (Notepad, Microsoft Office) can modify files in protected folders. Only untrusted/unsigned apps are blocked.

---

### Task 6: Monitor Security Baseline Compliance

1. Navigate to **Endpoint security** > **Security baselines**
2. Select **Security Baseline for Windows 10 and later** > Select your profile
3. **Overview** tab shows deployment metrics:
   - Devices assigned / Succeeded / Error / Conflict / Not applicable
4. Select **Device status** to view per-device details; select a device for setting-level details
5. For settings showing conflicts:
   - Review other configuration profiles that might override baseline settings
   - Adjust policy precedence or remove conflicting settings

---

### Task 7: Monitor ASR Rule Detections

1. Navigate to **Microsoft Defender portal** (`https://security.microsoft.com`)
2. Select **Reports** > **Endpoints** > **Attack surface reduction rules**
3. Review the dashboard: Detection trends, top triggered rules, devices with detections, mode distribution
4. Select a specific rule and view detailed detections (date/time, device, file path, action)
5. Investigate detections:
   - Legitimate app blocked → Add exclusion in ASR policy
   - Malicious behavior → Investigate device for potential compromise

**Interpretation guide**:
- **High detection count**: Attack activity (rules working) OR legitimate app conflicts (needs exclusions)
- **No detections**: No attacks OR rules not yet applied (verify deployment)
- **Audit mode detections**: Review before enabling block mode

---

### Task 8: Review Security Recommendations in Defender

1. Navigate to **Vulnerability management** > **Dashboard**
2. Review:
   - **Exposure score** (target: below 300 for well-secured environment)
   - **Microsoft Secure Score for Devices** (target: above 80%)
   - Vulnerable devices count and security recommendations
3. Select **Recommendations** and review by exposure impact:
   - Install security updates, turn on ASR rules, enable CFA, enable network protection, update vulnerable applications
4. For each recommendation: review exposed devices, security impact, remediation effort
5. Apply recommendations via Intune policy link or app deployment

---

### Exercise Summary – Contoso's Enhanced Security Posture

| Security Control | Capability | Impact |
|---|---|---|
| **Configuration hardening** | 100+ security settings applied (BitLocker, firewall, Defender, Credential Guard) | Attack surface reduced by 70% |
| **Advanced threat detection** | EDR monitoring all endpoints for suspicious behaviors and attacks | Time to detect threats reduced from days to minutes |
| **Exploit prevention** | ASR rules blocking 6+ high-risk attack vectors | Office macro exploits eliminated, credential theft attempts blocked |
| **Ransomware protection** | Controlled folder access protecting Documents, Pictures, and custom folders | Ransomware encryption attempts prevented |
| **Visibility** | Real-time security monitoring via Defender portal and Intune reports | Comprehensive threat visibility across all endpoints |
| **Continuous improvement** | TVM recommendations guiding security enhancements | Proactive vulnerability remediation |

---

## Confirmed Scenarios, Detailed Solutions, and Sources

### Scenario 1 – Security Baseline Not Applying to Enrolled Devices

**Symptom**: After deploying a Windows security baseline, devices remain non-compliant; Intune dashboard shows "Error" or "Not applicable."

**Root Causes**:
- Device not checking in to Intune (offline, stale enrollment)
- Another policy (GPO, configuration profile) overrides the baseline
- TPM not enabled for BitLocker-related settings
- Hybrid Entra-joined devices where domain GPO takes precedence

**Detailed Solution**:

1. **Force device sync**: Company Portal app > Settings > Sync. Or Intune admin center: Devices > All devices > Select device > Sync.
2. **Verify enrollment**: Confirm the device shows Enrollment date and Last check-in within the past 24 hours.
3. **Check for conflicts**: Navigate to Endpoint security > Security baselines > Baseline profile > Device status. If "Conflict," identify the conflicting policy under the device's configuration profiles.
4. **Review policy precedence**: Device configuration profiles and endpoint security policies override baselines. Remove duplicate settings or configure them in the higher-priority policy.
5. **For BitLocker failures**: Confirm TPM 2.0 is present and enabled in BIOS/UEFI.
6. **For domain GPO conflicts**: Run `gpresult /r` on the device to identify GPO-applied settings. Adjust the GPO or move devices to Entra-joined model.

**Applicable Sources**:
- https://learn.microsoft.com/en-us/mem/intune/protect/security-baselines-monitor
- https://learn.microsoft.com/en-us/mem/intune/configuration/device-profile-troubleshoot

---

### Scenario 2 – Devices Not Appearing in Microsoft Defender Portal After Onboarding

**Symptom**: EDR onboarding policy shows "Succeeded" in Intune, but devices don't appear in Defender portal Assets > Devices.

**Root Causes**:
- Propagation delay (up to 4 hours)
- No internet connectivity to Microsoft Defender cloud
- License not assigned to the user/device
- Sensor health issue after onboarding

**Detailed Solution**:

1. **Wait the propagation window**: Check again after 2-4 hours.
2. **Force sync from device**: Company Portal > Sync.
3. **Verify on device**: Windows Security app > Settings > About. Confirm "Management by: Microsoft Defender for Endpoint."
4. **Check Intune policy status**: Endpoint security > EDR > Select policy > Device status should be "Succeeded."
5. **Verify license**: Microsoft 365 admin center > Users > Select user > Licenses. Confirm Defender for Endpoint Plan 1 or Plan 2 is assigned.
6. **Check connectivity**: Defender requires outbound HTTPS to specific Microsoft endpoints. Review network proxy/firewall rules.
7. **Review event log**: Event Viewer > Applications and Services Logs > Microsoft > Windows > Sense > Operational. Look for sensor communication errors.

**Applicable Sources**:
- https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/onboarding
- https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/run-detection-test

---

### Scenario 3 – ASR Rule Blocking a Legitimate Business Application

**Symptom**: After deploying ASR rules in Block mode, a business-critical application stops functioning.

**Root Causes**:
- Application behavior matches an ASR rule pattern
- Rule deployed in Block mode without an audit phase
- Application path not excluded from ASR policy

**Detailed Solution**:

1. **Identify the blocking rule**: Defender portal > Reports > Endpoints > Attack surface reduction rules. Filter by device or time.
2. **Switch rule to Audit mode temporarily**: Update the ASR policy for the specific rule to "Audit only."
3. **Collect audit logs via PowerShell**:
   ```powershell
   Get-WinEvent -LogName "Microsoft-Windows-Windows Defender/Operational" |
   Where-Object { $_.Id -eq 1121 -or $_.Id -eq 1122 } |
   Select-Object TimeCreated, Message | Format-List
   ```
   - Event ID 1121: ASR rule triggered in Block mode
   - Event ID 1122: ASR rule triggered in Audit mode
4. **Add an exclusion**: In Intune ASR policy, under Attack Surface Reduction Only Exclusions, add the executable path:
   ```
   C:\Program Files\BusinessApp\businessapp.exe
   ```
5. **Re-enable Block mode**: After confirming the exclusion resolves the issue.
6. **Document the exclusion** with business justification for auditing.

**Applicable Sources**:
- https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/enable-attack-surface-reduction
- https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/customize-attack-surface-reduction

---

### Scenario 4 – Controlled Folder Access Blocking Backup Software

**Symptom**: After enabling Controlled Folder Access, the backup agent can no longer access files in protected folders.

**Root Causes**:
- Backup agent executable is not Microsoft-signed and not on the trusted app list
- CFA was enabled in Block mode without audit testing

**Detailed Solution**:

1. **Identify blocked app**: Defender portal > Reports > Endpoints > Controlled folder access > Blocked applications. Note the exact executable path.
2. **Add backup app to trusted list**: Intune ASR policy > List of apps that have access to protected folders:
   ```
   C:\Program Files\BackupSolution\backupagent.exe
   ```
3. **Test and verify**: After policy sync, confirm the backup agent can now write to protected folders.
4. **Repeat for other tools**: Add development IDEs, sync clients, reporting tools as needed.
5. **Use Audit mode during future rollouts**: Enable CFA in Audit mode for 2-4 weeks to identify all apps requiring trusted status.

**Applicable Sources**:
- https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/customize-controlled-folders
- https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/controlled-folders

---

### Scenario 5 – High Exposure Score and Actionable TVM Recommendations

**Symptom**: Defender portal shows an Exposure Score above 300 and multiple critical security recommendations.

**Root Causes**:
- Missing Windows security updates
- Disabled or misconfigured Defender settings
- Vulnerable third-party software
- Missing ASR rules or BitLocker

**Detailed Solution**:

1. **Review prioritized recommendations**: Defender portal > Vulnerability management > Recommendations. Sort by "Exposure impact" descending.
2. **Address critical updates**: Deploy missing patches via Intune update rings or Windows Update for Business.
3. **Enable missing security features**: Link directly from TVM recommendation to create an Intune policy (ASR rules, BitLocker, network protection).
4. **Remove or update vulnerable software**: Use Intune app deployment to update or remove vulnerable third-party apps.
5. **Track progress weekly**: Target Exposure Score below 300 and Secure Score for Devices above 80%.

**Applicable Sources**:
- https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/next-gen-threat-and-vuln-mgt
- https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/tvm-security-recommendation

---

## Related Scenarios and Detailed Solutions

### Related Scenario A – Integrating Defender Risk Score with Conditional Access

**Solution Steps**:
1. Enable connector: Intune > Endpoint security > Microsoft Defender for Endpoint > **Connect**.
2. Create compliance policy: Set device risk score ≤ Medium (or Low for higher security).
3. Create Conditional Access policy in Entra admin center: Require compliant device for cloud app access.
4. Test: Simulate a high-risk detection on a test device and verify access is blocked.

**Sources**:
- https://learn.microsoft.com/en-us/mem/intune/protect/conditional-access-intune-common-ways-use
- https://learn.microsoft.com/en-us/mem/intune/protect/advanced-threat-protection-configure

---

### Related Scenario B – Investigating a Security Incident Using the Defender Portal

**Solution Steps**:
1. **Open incident**: Defender portal > Incidents & alerts.
2. **Review attack story**: Examine the timeline — initial process, child processes, files written, network connections.
3. **Isolate device if confirmed**: Device actions > **Isolate device**.
4. **Run antivirus scan**: Device actions > **Run antivirus scan**.
5. **Collect investigation package**: Device actions > **Collect investigation package** for forensic analysis.
6. **Review AIR results**: Approve recommended remediation actions.
7. **Block IOCs**: Add malicious file hashes or IP addresses: Defender portal > Settings > Endpoints > Indicators.
8. **Post-incident review**: Update ASR rules, exclusions, or baseline settings based on findings.

**Sources**:
- https://learn.microsoft.com/en-us/microsoft-365/security/defender/incidents-overview
- https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/respond-machine-alerts

---

### Related Scenario C – Updating to a New Security Baseline Version

**Solution Steps**:
1. Intune admin center > Endpoint security > Security baselines > Select baseline profile.
2. Select **Change version** > Select the new version.
3. Review the **Settings comparison** to identify what changed.
4. Adjust customizations if needed.
5. Deploy to a pilot group first and monitor for 1-2 weeks.
6. Roll out to all devices once validated.
7. Keep previous version active during migration; don't delete until all devices migrate.

**Sources**:
- https://learn.microsoft.com/en-us/mem/intune/protect/security-baselines-configure
- https://learn.microsoft.com/en-us/mem/intune/protect/security-baselines

---

### Related Scenario D – Deploying Exploit Protection for a Specific Application via Intune

**Solution Steps**:
1. On a reference machine, configure mitigations: Windows Security > App & browser control > Exploit protection > Program settings > Add the application.
2. Enable: DEP, ASLR (mandatory/bottom-up), CFG, child process blocking.
3. Export configuration as XML:
   ```powershell
   Get-ProcessMitigation -RegistryConfigFilePath "C:\ExploitProtection.xml"
   ```
4. Import into Intune: Endpoint security > Attack surface reduction > Create policy > Exploit Protection profile > Upload XML.
5. Assign to device groups and deploy.

**Sources**:
- https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/customize-exploit-protection
- https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/enable-exploit-protection

---

## Tools and Resources Used

| Tool / Resource | Description | URL |
|---|---|---|
| **Microsoft Intune admin center** | Central portal for deploying baselines, ASR policies, and EDR onboarding | https://intune.microsoft.com |
| **Microsoft Defender portal** | SOC for monitoring incidents, TVM, and ASR reports | https://security.microsoft.com |
| **Microsoft Entra admin center** | Conditional Access and identity management | https://entra.microsoft.com |
| **Company Portal app** | Forces device sync and shows enrollment status | Microsoft Store |
| **Windows Security app** | On-device status for Defender, CFA, exploit protection | Built into Windows 10/11 |
| **Security baselines (Intune)** | Pre-configured Windows, Edge, and Defender baseline templates | Intune > Endpoint security > Security baselines |
| **Endpoint detection and response (EDR)** | Intune policy type for onboarding to Defender for Endpoint | Intune > Endpoint security > EDR |
| **Attack surface reduction rules** | Intune policy type for ASR rules, exploit protection, CFA | Intune > Endpoint security > Attack surface reduction |
| **Threat & Vulnerability Management (TVM)** | Dashboard for exposure score and security recommendations | Defender portal > Vulnerability management |
| **Microsoft 365 E3 / E5** | Licensing required for Defender for Endpoint Plan 1/Plan 2 | https://www.microsoft.com/microsoft-365 |
| **PowerShell** | ASR event log queries and exploit protection config export | Built into Windows |
| **Windows Event Viewer** | Defender Operational log (Event IDs 1121/1122) | `eventvwr.msc` |
| **Module Overview** | Microsoft Learn landing page | https://learn.microsoft.com/en-us/training/modules/harden-endpoints-monitor-security-intune-defender-endpoint/ |
| **Security Baselines docs** | Official deployment and monitoring documentation | https://learn.microsoft.com/en-us/mem/intune/protect/security-baselines |
| **ASR rules reference** | Full list and GUIDs for all 17 ASR rules | https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/attack-surface-reduction-rules-reference |
| **Controlled Folder Access docs** | Configuration and trusted app guidance | https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/controlled-folders |
| **Exploit protection docs** | System-wide and app-specific mitigation details | https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/exploit-protection |
| **Defender for Endpoint onboarding** | Onboarding methods and verification guidance | https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/onboarding |
| **TVM documentation** | Threat and vulnerability management overview | https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/next-gen-threat-and-vuln-mgt |
| **Conditional Access with Intune** | Enforcing compliance with Defender risk scores | https://learn.microsoft.com/en-us/mem/intune/protect/advanced-threat-protection-configure |

---

