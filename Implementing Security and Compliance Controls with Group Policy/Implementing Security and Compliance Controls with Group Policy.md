# Project Phase: Implementing Security and Compliance Controls with Group Policy

# Background
In the ongoing NewVue Health Infrastructure Modernization Project, the consulting team has successfully deployed and configured the core Active Directory infrastructure within the newvue.local domain.
The Proof-of-Concept environment currently includes NV-DC1 as the primary Domain Controller and NV-CL1 as a domain-joined client workstation used for verification and policy testing.

Following a recent internal compliance audit, NewVue Health outlined several configuration standards that must be implemented to align with its security and operational policies. These controls are to be enforced using Group Policy Objects (GPOs) within the Active Directory environment.
Each configuration represents a key compliance requirement for domain-wide security consistency and user accountability.

# Security Requirement
The following configurations are required:

### 1. Password and Account Lockout Requirements:

- Minimum password length: 12 characters

- Enforce password complexity: Enabled

- Maximum password age: 90 days

- Minimum password age: 5 days

- Account lockout threshold: 5 invalid attempts (lockout duration: 15 minutes)

### 2. Logon Banner Requirement:

- Display a pre-logon message informing users that access is restricted to authorized NewVue Health personnel and that all activities are monitored.

### 3. Control Panel and Settings Restriction:

- Prevent non-administrative users from accessing or modifying Control Panel and PC Settings.

### 4. Desktop Wallpaper and Branding Requirement:

- Apply the official NewVue Health wallpaper across all domain computers to maintain a consistent and professional appearance.

All configurations will be implemented on NV-DC1 and verified on NV-CL1, demonstrating how Group Policy can be used to transform compliance requirements into enforceable technical standards within an enterprise domain.

# Purpose of the Assignment
This assignment demonstrates how Group Policy Management enforces compliance-driven configuration standards in a Windows Server domain.
Students will learn to design, apply, and verify both domain-wide and targeted policies to maintain security, standardization, and governance.

By completing this activity, students will:

- Create, edit, and link Group Policy Objects (GPOs) to domains and OUs.

- Enforce authentication, branding, and access-control standards using centralized management.

- Safely deploy targeted policies to specific departments.

- Verify policy application using GUI and command-line tools.

- Understand how GPOs convert compliance policies into enforceable technical configurations.

# Tasks and Steps
# Task 1 – Configure Password and Account Lockout Policy
1. Log on to NV-DC1 as newvue\Administrator.

2. Open Group Policy Management Console (GPMC) → expand Forest: newvue.local → Domains → newvue.local.

3. Right-click Group Policy Objects → New → name it Password and Account Policy.

4. Right-click the new GPO → Edit.

5. Navigate to Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy.

6. Configure:

- Minimum password length: 12

- Enforce password complexity: Enabled

- Maximum password age: 90 days

- Minimum password age: 5 days

7. Under Account Lockout Policy, set:

- Threshold: 5 invalid attempts

- Lockout duration: 15 minutes

- Reset counter after: 15 minutes

8. Close the editor.

9. In GPMC, right-click newvue.local → Link an Existing GPO… → select Password and Account Policy.

10. On NV-CL1, open Command Prompt → run `gpupdate /force`.

Evidence 1: Screenshot of the Password and Account Policy settings.
Evidence 2: Screenshot of `gpupdate /force` confirmation on NV-CL1.

# Task 2 – Configure the Logon Message (Interactive Logon Banner)
To meet legal and security communication requirements, a pre-logon banner will be configured to display before user authentication.

1. On NV-DC1, open Group Policy Management Console (GPMC).

2. Expand Forest: newvue.local → Domains → newvue.local → Group Policy Objects.

3. Right-click Group Policy Objects → New → name the policy Domain Logon Security Message.

4. Right-click the new GPO → Edit.

5. Navigate to Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Security Options.

6. Configure:

- Interactive logon: Message title = NewVue Health Network Access Policy

- Interactive logon: Message text = Access to this system is restricted to authorized NewVue Health personnel. All activities are monitored.

7. Close the editor.

8. In GPMC, right-click newvue.local → Link an Existing GPO… → select Domain Logon Security Message.

9. On NV-CL1, open Command Prompt → run `gpupdate /force`.

10. Sign out and verify the banner appears before logon.

Evidence 3: Screenshot showing the GPO under Group Policy Objects before linking.
Evidence 4: Screenshot of configured Security Options settings.
Evidence 5: Screenshot of the logon banner on NV-CL1.

# Task 3 – Configure Desktop Wallpaper and Corporate Branding
Some Group Policies depend on external files such as wallpapers, logon scripts, or software packages.
Before such policies can be applied, administrators must provision the required files and ensure they are stored in a network share accessible to all authenticated users and computers.
In this task, the wallpaper file will be created and shared from NV-DC1, then deployed through Group Policy.

### Step 1 – Prepare and Share the Wallpaper Folder
1. On NV-DC1, open File Explorer → navigate to Documents.

2. Create a folder named Branding.

3. Place the image file (NewVueWallpaper.jpg) inside the Branding folder.

4. Right-click the Branding folder → Properties → Sharing → Advanced Sharing.

5. Check Share this folder → name the share Branding.

6. Click Permissions → select Everyone → grant Read permission → OK.

7. On the Security tab, ensure Authenticated Users have Read permission → OK.

8. Confirm the share path:
```bash
\\NV-DC1\Branding
```

9. From NV-CL1, open File Explorer → test access to `\\NV-DC1\Branding`.

Checkpoint: Ensure NV-CL1 can open and view NewVueWallpaper.jpg from the share.

### Step 2 – Create and Apply the Wallpaper Policy
1. On NV-DC1, open Group Policy Management Console (GPMC).

2. Right-click newvue.local → Create a GPO in this domain and link it here… → name it Corporate Desktop Branding.

3. Edit the new GPO → navigate to
User Configuration → Policies → Administrative Templates → Desktop → Desktop.

4. Double-click Desktop Wallpaper → set to Enabled.

5. Configure:

- Wallpaper Name = `\\NV-DC1\Branding\NewVueWallpaper.jpg`

- Wallpaper Style = Fill

6. Enable Prevent changing desktop background → OK.

7. On NV-CL1, open Command Prompt → run:
```bash
gpupdate /force
```
8. Sign out and back in to verify that the wallpaper is applied.

Evidence 6: Screenshot of the shared Branding folder and image on NV-DC1.
Evidence 7: Screenshot of the Desktop Wallpaper configuration in the GPO Editor.
Evidence 8: Screenshot of the NV-CL1 desktop showing the applied NewVue Health wallpaper.

# Task 4 – Apply Targeted Policy to Restrict Control Panel and Settings
This task demonstrates targeted Group Policy application by restricting Control Panel access only for users within the Administration OU.
It models scoped policy deployment so that restrictive settings do not impact administrative or domain-wide operations.

1. On NV-DC1, open GPMC.

2. Right-click the Administration OU → Create a GPO in this domain and link it here… → name it Restrict Control Panel Access.

3. Edit the GPO → navigate to User Configuration → Policies → Administrative Templates → Control Panel.

4. Enable Prohibit access to Control Panel and PC Settings → OK.

5. On NV-CL1, log on as a user from the Administration OU → run `gpupdate /force`.

6. Attempt to open Control Panel or Settings; access should be denied.

Evidence 9: Screenshot of the restriction setting in GPO Editor.
Evidence 10: Screenshot from NV-CL1 showing the restriction message.

# Task 5 – Verify Group Policy Application using gpresult
1. On NV-CL1, open Command Prompt (Run as Administrator) → run:
   
```bash
gpresult /r
```

2. Verify that the following GPOs appear under Applied Group Policy Objects:

- Password and Account Policy

- Domain Logon Security Message

- Corporate Desktop Branding

- Restrict Control Panel Access

3. Capture the output showing the logged-in user and applied policies.

Evidence 11: Screenshot of `gpresult /r` output confirming all applied GPOs.

























