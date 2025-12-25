# Project Phase: NewVUE Hybrid Implementation

# Background
NewVue Health is transitioning from a fully on-premises Active Directory environment to a hybrid identity model that integrates on-prem identities with cloud services. As part of the Infrastructure Modernization Project, the organization now requires secure synchronization of on-premises directory objects to Microsoft Entra ID to support cloud authentication, Microsoft 365 access, and future Intune device management.

Up to this point, you have:

 - Designed and implemented NewVue Health’s Active Directory structure

 - Configured users, groups, file services, and policies

 - Deployed cloud identities and licensing

 - Established secure administrative connectivity to the Microsoft Entra tenant

The next milestone is to establish hybrid identity through directory synchronization.

This week’s activities will prepare on-prem user objects for cloud synchronization, correct directory inconsistencies using IdFix, update UPNs to match the cloud tenant domain, configure Microsoft Entra Connect using Password Hash Synchronization (PHS), and validate successful synchronization and cloud sign-in.

This lab marks the formal bridge between the on-premises environment you built and the cloud environment configured in Week 8.

# Purpose
The purpose of this assignment is to provide hands-on experience with the essential components of hybrid identity. Students learn how to prepare, repair, and synchronize on-premises Active Directory users with Microsoft Entra ID using tools and methods used in real enterprise hybrid configurations.

 - By completing this activity, students will:

 - Understand the role of identity cleansing in hybrid deployments

 - Learn the importance of properly formatted attributes such as UPNs

 - Gain practical experience running and interpreting IdFix results

 - Configure an Entra Connect deployment using Password Hash Synchronization

 - Validate synchronization and cloud authentication for on-premises users

These skills form the foundation of hybrid identity engineering and are essential for supporting Microsoft 365, Intune, Conditional Access, and password-policy integration across environments.

Objectives
After completing this lab, students will be able to:

 - Prepare on-premises Active Directory for hybrid identity by identifying and correcting directory issues using IdFix and updating user UPNs to match the cloud domain.

 - Install and configure Microsoft Entra Connect using Password Hash Synchronization and appropriate OU filtering.

 - Validate successful directory synchronization between on-prem Active Directory and Microsoft Entra ID.

 - Verify cloud authentication by signing in with a synchronized on-prem user account.

# Task 1 – Validate Cloud Domain & Create Pre-Sync Snapshot
### Step 1 – Identify Cloud Tenant Domain

1. Sign in to https://entra.microsoft.com Links to an external site.

2. On the Home page, under Default Directory, note your Primary Domain
(Example: louvrousgmail.onmicrosoft.com)
This will be added as a UPN suffix in AD.

### Step 2 – Create a Snapshot

1. Open VirtualBox

2. Select NV-DC1

3. Create a snapshot:
   ### Pre-Hybrid Sync – Week 9

### Checkpoint 1 – Submit:

Screenshot of Primary Domain

Screenshot of NV-DC1 snapshot

# Task 2 – Run IdFix & Remediate Directory Issues
### Step 1 – Download IdFix

Download IdFix directly from Microsoft GitHub:
https://github.com/microsoft/idfix/tree/master/MSIs Links to an external site.
Install and launch it on NV-DC1.

### Step 2 – Run IdFix and Review Results

Click Query to begin scanning directory objects for issues that could block synchronization.

IdFix returns results using the following fields:

  - DistinguishedName – Full AD path of the object
  - ObjectClass – User, group, or contact
  - Attribute – Attribute containing the issue
  - Error – Error type (e.g., format, illegalCharacter, duplicate)
  - Value – Current attribute value
  - Update – Suggested correction
  - Action – Action to take (usually EDIT or REMOVE)
  - Use this information to identify and understand directory issues.

### Fixing Errors in IdFix

For each row:

1. Review the Update field to confirm the correction
2. Click Accept to approve the suggested change
3. Once all changes are accepted, click Apply to commit them to Active Directory

This ensures all user objects meet Microsoft Entra synchronization requirements.

### Step 3 – Add Cloud UPN Suffix

To allow updated UPNs, the cloud domain must be added to Active Directory.

1. Open Active Directory Domains and Trusts
2. In the left pane, right-click “Active Directory Domains and Trusts”
3. Select Properties
4. Under Alternative UPN suffixes, enter your cloud domain:
5. yourtenant.onmicrosoft.com
6. Click Add → OK
   
This makes the cloud suffix available for all user accounts.

### Step 4 – Bulk Update UPNs Using PowerShell

Run the following script on NV-DC1 (PowerShell as Administrator):

```bash
Import-Module ActiveDirectory
```
 

# Set the new UPN suffix

```$newSuffix = "louvrousgmail.onmicrosoft.com"```   #Replace with your actual tenant domain

 
### #Define the list of OUs
```bash
$OUList = @(
"OU=Users,OU=Human_Resources,DC=newvue,DC=local",
"OU=Users,OU=IT_Operations,DC=newvue,DC=local",
"OU=Users,OU=Finance,DC=newvue,DC=local",
"OU=Users,OU=Clinical_Services,DC=newvue,DC=local",
"OU=Users,OU=Administration,DC=newvue,DC=local"
)
```
### #Update UPNs for all users in the specified OUs
```bash
foreach ($OU in $OUList) {
    Get-ADUser -Filter * -SearchBase $OU |
    ForEach-Object {
        $newUPN = $_.SamAccountName + "@" + $newSuffix
        Set-ADUser $_ -UserPrincipalName $newUPN
    }
}
```

### Step 5 – Verify Changes Using PowerShell

Verify UPN changes for one OU (Human_Resources) with:


```bash
Get-ADUser -Filter * -SearchBase "OU=Users,OU=Human_Resources,DC=newvue,DC=local" -Properties UserPrincipalName |
Select SamAccountName, UserPrincipalName
 ```

### Checkpoint 2 – Submit:

- Screenshot of IdFix before remediation
- Screenshot of IdFix after remediation
- Screenshot of AD UPN suffix in Domains & Trusts
- Screenshot of PowerShell bulk UPN update
- Screenshot of verification output
 
# Task 3 – Install & Configure Microsoft Entra Connect
### Step 1 – Download Entra Connect

Download from:
🔗 https://www.microsoft.com/en-us/download/details.aspx?id=47594 Links to an external site.
Run AzureADConnect.msi.

### Step 2 – Start Installation

- Accept license

- Click Customize

### Step 3 – Install Required Components

- Click Install

- Wait for prerequisite validation

### Step 4 – Choose Authentication Method

- Select Password Hash Synchronization (PHS)

- Click Next

Do NOT enable SSO for this lab

### Step 5 – Connect to Microsoft Entra Tenant

- Enter Global Administrator credentials

- Authenticate with MFA if required

- Click Next

### Step 6 – Connect to On-Prem AD

- Click Add Directory

- Choose Create new AD account

- Enter:

administrator@newvue.local

- Click OK → Next

Entra Connect will create the MSOL_xxxx AD DS connector account.

### Step 7 – UPN Suffix Mismatch Warning

Select:

### ✔ Continue without matching all UPN suffixes to verified domains
Then Next

Step 8 – Domain & OU Filtering

Select Sync Selected Domains and OUs, then choose ONLY:

- Administration

- Clinical_Services

- Human_Resources

- IT_Operations

- Finance

Click Next.

### Step 9 – Identity Settings

Keep default:

### - Users are represented once across all directories

### - SourceAnchor = ObjectGUID

Click Next

### Step 10 – Filtering Options

### - Select Synchronize all users and devices

### - Click Next

Step 11 – Optional Features

Enable:

### ✔ Password Hash Synchronization

Do NOT enable writeback or hybrid Exchange.

Click Next

### Step 12 – Configure & Start Sync

- Select: Start the synchronization process when configuration completes

- Click Install

### Step 13 – Finish

Once configuration is complete → click Exit

### Checkpoint 3 – Submit

- Screenshot of PHS selection

- Screenshot of selected OUs

- Screenshot of Entra Connect “Configuration Complete”

# Task 4 – Validate Directory Synchronization
### Step 1 – Verify Sync Using the Synchronization Service Manager

On NV-DC1, open:
### Start → Synchronization Service

Confirm:

 - Full Import (stage 1) ran

 - Full Synchronization ran

 - Export to Microsoft Entra ID completed

Click Export statistics → verify the list of exported users

### Step 2 – Verify Sync in Entra Admin Center

1. Go to https://entra.microsoft.com Links to an external site.

2. Navigate to:
### Microsoft Entra ID → Connect Sync

Verify:

- Sync Status = Healthy

- No synchronization errors

### Step 3 – Verify Users in Cloud

Go to:

### Microsoft Entra ID → Users → All users

Confirm:

- Users have appeared

- Source = Windows Server AD

- UPN suffix uses the cloud domain

### Checkpoint 4 – Submit

- Screenshot of Synchronization Service showing successful Import/Sync/Export

- Screenshot of Entra “Connect Sync” status

- Screenshot of synced users list

# Task 5 – Validate Cloud Sign-In
### Step 1 – Reset Password

Use ADUC to reset a pilot user’s password.

### Step 2 – Sign In to Microsoft 365

1. In a private browser:

2. Go to https://portal.office.com Links to an external site.

3. Sign in using cloud UPN

Complete password reset if prompted

### Step 3 – Validate Cloud Access

Confirm the user can access:

- Outlook

- OneDrive

- Teams

- SharePoint

### Checkpoint 5 – Submit

- Screenshot of Microsoft 365 homepage after login

- Screenshot of App Launcher showing cloud apps
