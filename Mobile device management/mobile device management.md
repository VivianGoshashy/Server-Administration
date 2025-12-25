# Project Phase: Mobile Device Management

# Background
With Entra Connect fully deployed in the previous phase of the NewVue Health Infrastructure Modernization Project, the on-premises Active Directory environment is now synchronized with the NewVue cloud tenant.
The next step in the modernization journey is to transition NewVue Health’s Windows endpoints into Hybrid Azure AD Join, enabling centralized device management and cloud-based security enforcement through Microsoft Intune.

Hybrid Azure AD Join allows a domain-joined Windows 11 device to remain fully integrated with traditional on-prem identity services while simultaneously registering in Microsoft Entra ID.
Once registered, Intune’s MDM automatic enrollment activates, placing the device under cloud management for configuration profiles, compliance policies, application deployments, and update governance.

In this lab, you will configure Hybrid Azure AD Join, configure automatic MDM enrollment, apply the required Group Policy settings, and validate device registration through both the Windows client and the Intune Admin Center.
This is a core real-world workflow performed by modern enterprises transitioning to cloud-first endpoint management.

# Purpose of This Assignment
This assignment strengthens your understanding of cloud-based endpoint management and equips you with skills essential for modern systems administration roles. By completing this lab, you will:

Learn how hybrid Azure AD join enables seamless cloud management of domain-joined devices.

Configure automatic MDM enrollment so devices flow directly into Intune after synchronization.

Deploy enterprise apps (Microsoft 365) from the cloud to enrolled devices.

Apply compliance and security baselines aligned with organizational policy.

Manage updates, monitor device health, and execute remote actions such as restart or wipe.

Build confidence administering enterprise device fleets in Microsoft Intune.

This is a core competency for modern system administrators working in hybrid or cloud-centric environments.

# Learning Objectives
After completing this lab, you will be able to:

Enable and validate automatic MDM enrollment for hybrid Azure AD joined devices.

Confirm that a domain-joined Windows 11 device successfully enrolls into Intune.

Deploy Microsoft 365 Apps from the Intune Admin Center.

Create and assign compliance policies.

Apply Microsoft Security Baselines.

Configure Windows Update Rings.

Perform remote device actions and verify reporting.

# Task and Steps
# Task 0  - Configuring Hybrid Azure AD Join (SCP Configuration Prerequisite)
Before device enrollment can succeed, Azure AD Connect must be configured to enable Hybrid Azure AD Join. This step ensures that domain-joined Windows devices can automatically register with Microsoft Entra ID by using a Service Connection Point (SCP) published in Active Directory.

On the server running Azure AD Connect:

Open Azure AD Connect.

Select Configure to open the configuration wizard.

### Step 2 – Select Device Options

On the Additional Tasks page:

Select Configure device options.

Click Next.

### Step 3 – Authenticate to Microsoft Entra ID

When prompted:

Enter credentials for a Global Administrator or Hybrid Identity Administrator.

Click Next to continue.

### Step 4 – Choose Hybrid Azure AD Join

On the Device Options page:

Select Configure Hybrid Azure AD Join.

Click Next.

### Step 5 – Select Device Operating Systems

Under Device Operating System Selection:

Check Windows 10 or later domain-joined devices.

(Optional) Select Down-level devices only if needed.

Click Next.

### Step 6 – Configure the Service Connection Point (SCP)

Azure AD Connect will prepare to publish the SCP into Active Directory.

On the SCP Configuration page:

Confirm Authentication Service: Azure Active Directory.

Provide Enterprise Admin credentials for the on-premises AD forest.

Click Next.

This step creates/updates the SCP under:

```bash
CN=Device Registration Configuration, CN=Services, CN=Configuration, DC=newvue, DC=local
The SCP contains your tenant ID and metadata that domain-joined devices use to discover Microsoft Entra ID during registration.
```
### Step 7 – Complete the Configuration

Review the summary.

Click Configure to apply the changes.

When completed, click Exit.

# TASK 1 — Configure MDM Automatic Enrollment in Intune
### Step 1 — Configure MDM Enrollment Settings

1. Sign in to:
https://intune.microsoft.com Links to an external site.

2. Navigate to:
Devices → Enrollment → Automatic Enrollment

3. Under MDM User Scope, select:

All
(or a security group containing device owners)

4. Click Save.

Checkpoint 1:
Screenshot – Automatic MDM Enrollment configuration showing "MDM user scope: All".

# TASK 2 — Enable Hybrid Azure AD Join Using Group Policy
### Step 1 — Create GPO

1. On NV-DC1, open Group Policy Management.

2. Right-click the OU containing NV-CLI → Create a GPO named:
Hybrid Azure AD Join – Auto MDM Enrollment

3. Right-click the new GPO → Edit

### Step 2 — Configure Automatic MDM Enrollment

Navigate:

### Computer Configuration → Administrative Templates → Windows Components → MDM

Enable:

### Enable automatic MDM enrollment using default Azure AD credentials

Set to: Enabled

Select: User Credential

Click OK.

### Step 3 — Apply the Policy

On NV-CLI:

1. Run Command Prompt as Administrator

2. Execute:

```bash
gpupdate /force
```

3. Restart NV-CLI.

Checkpoint 2:
Screenshot – GPO setting (“Enable automatic MDM enrollment…”) set to Enabled.

# TASK 3 — Verify Hybrid Azure AD Join and Enrollment
### Step 1 — Validate Hybrid Join

On NV-CLI, open Command Prompt and run:
```bash
dsregcmd /status
```
Confirm the following:

### AzureAdJoined : YES

### DomainJoined : YES

### MDMUrl is populated

### NgcSet shows YES

### Step 2 — Verify Enrollment in Intune

Go to:

### Intune Admin Center → Devices → All Devices

You should now see NV-CLI listed and marked as:

### . Hybrid Azure AD Joined

### . Compliant / Not compliant (will change after policies)

Checkpoint 3:
Screenshot – `dsregcmd /status` output.
Screenshot – NV-CLI appearing in Intune under All Devices.

# Task 4 – Deploy Microsoft 365 Apps (Office)
### Step 1 — Create the Application Deployment

In the Intune Admin Center:

1. Go to:

### Apps → Windows → Create

2. Choose:

### Microsoft 365 Apps for Windows 10 and later

3. Configure:

Architecture: 64-bit

Update Channel: Current Channel

Included Apps: Word, Excel, PowerPoint, Outlook, Teams

Languages: Match Operating System

Click Next.

### Step 2 — Assign the App

On Assignments page:

 . Under Required, add the group containing NV-CLI (e.g., “Lab Devices”).

Click Create.

### Step 3 — Monitor Deployment

Navigate to:

### Apps → Monitor → Installation Status

On NV-CLI:

Confirm Office apps appear in the Start Menu.

Checkpoint 4:
Screenshot – Deployment configuration summary.
Screenshot – Microsoft 365 Apps visible on NV-CLI.

# TASK 5 — Configure Device Compliance Policy
### Step 1 — Create the Policy

Navigate to:

# Devices → Compliance policies → Create policy

Choose Windows 10 and later.

### Step 2 — Configure Settings

Enable:

### . Require BitLocker

### . Require password complexity

Click Next.

### Step 3 — Assign the Policy

Under Assignments:

Click Add all users

Click Create.

Checkpoint 5

Provide:

Screenshot of compliance policy assignment

Screenshot showing NV-CLI compliance status (Compliant/Pending)

# TASK 6 — Assign Windows Security Baseline
### Step 1
Navigate to:
Endpoint security → Security baselines → Windows 10 and later
Select Create policy
Name: NewVue – Windows Security Baseline
Select Next through configuration pages.

### Step 2
Under Assignments, select:
Add all users
Add all devices
Select Create.

### Step 3 (Validate in Intune)
Navigate to:
Devices → All devices → NV-CLI → Security baselines
Confirm the baseline appears and shows Success, Pending, or In progress.

### Step 4 (Validate on NV-CLI using Registry Editor)
On NV-CLI, open Registry Editor and navigate to:
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PolicyManager\current\Device
Expand folders such as:
DeviceLock
Browser
SmartScreen
System
Security
These appear only after baseline application.

Checkpoint 6
Screenshot of NV-CLI Security Baseline status in Intune.
Screenshot of the PolicyManager registry keys.

# TASK 7 — Configure Windows Update Ring
Step 1

In the Intune Admin Center, navigate to:

### Devices → Windows → Windows Update → Update rings for Windows 10 and later

Select Create update ring.

Enter the following under Basics:

• Name: NewVue – Update Ring
• Description: (optional)

Select Next.

### Step 2

You will now review Windows Update settings.

Locate the following settings and configure:

###  • Feature update deferral period (days): 7
###  • Quality update deferral period (days): 2

Select Next.

### Step 3 – Assignments

Under Assignments:

• Select Add all users
• Select Add all devices

Select Next, then Create.

Checkpoint 7

Provide:

• Screenshot showing the NewVue – Update Ring configuration summary
• Screenshot showing deferral settings (7 days feature, 2 days quality)

# TASK 8— Perform Remote Actions
Navigate to:
Devices → All devices → NV-CLI

Test one or two safe remote actions:
Sync
Restart
Rename

Do not use Wipe or Fresh Start.

Checkpoint 8
Screenshot of remote actions menu.
Screenshot of a completed action such as Sync or Restart.

# TASK 9 — Review Device Reports
### Step 1
Navigate to:
Devices → Windows → Windows devices
Select NV-CLI
The Overview page shows Essential details including:
• OS version
• Device model
• Serial number
• Compliance state
• Last check-in

### Step 2
For detailed hardware information:
Device configuration → Hardware
View CPU, RAM, storage, TPM version, and system details.

### Step 3
For compliance details:
Device compliance
Review compliance status and evaluated policies.

Checkpoint 9
Screenshot of Hardware information page.
Screenshot from Device compliance page.













