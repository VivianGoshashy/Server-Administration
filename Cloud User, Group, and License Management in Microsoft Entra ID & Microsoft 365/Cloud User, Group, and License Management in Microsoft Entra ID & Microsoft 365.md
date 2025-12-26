# Project Phase: Cloud User, Group, and License Management in Entra ID & MS365

# Background
In preparation for Phase 2 of the NewVue Health Infrastructure Modernization Project, cloud identity services must now be configured within Microsoft Entra ID and Microsoft 365.
With the tenant established and licensing activated, the next step is to create cloud users, organize them into department-based groups, and assign Microsoft 365 E5 licenses to prepare these accounts for cloud services.

This activity focuses on establishing a pilot identity set for NewVue Health and introduces both Assigned and Dynamic group membership — allowing students to observe how cloud identity automation works in a real enterprise environment.

# Purpose of the Assignment
This assignment provides hands-on experience with foundational cloud identity management tasks.
Students will create user accounts, configure departmental security groups, assign licenses, and validate group membership — including automated membership via dynamic rules.
These skills are essential for systems administrators supporting modern cloud-based enterprise services.

# Learning Objectives
Upon completing this activity, students will be able to:

- Create cloud-only user accounts in Microsoft Entra ID

- Create Assigned and Dynamic security groups

- Configure attribute-based membership rules

- Add users manually to Assigned groups and verify automatic membership in Dynamic groups

- Assign Microsoft 365 E5 licenses

- Verify identity configuration using both Entra ID and Microsoft 365 Admin Center

# Pilot Users for NewVue Health
Use the following pilot users for this activity. Replace <yourtenant> with your actual tenant domain.

|Department	| Display Name |	UPN	| Job Title |
| :---: | :---: | :---: |:---: |
| Administration |	Alice Morgan	| alice.morgan@<yourtenant>.onmicrosoft.com	| Admin Coordinator |
| Clinical Services	| Brian Patel| 	brian.patel@<yourtenant>.onmicrosoft.com	| Registered Nurse | 
| Human Resources |	Carla Johnson |	carla.johnson@<yourtenant>.onmicrosoft.com |	HR Generalist |
| IT Operations |	Daniel Kim	| daniel.kim@<yourtenant>.onmicrosoft.com	| Systems Administrator | 
| Finance |	Evelyn Rodriguez |	evelyn.rodriguez@<yourtenant>.onmicrosoft.com | Financial Analyst | 

The Table shows NewVue Pilot Cloud USers

# Task 1 – Create Cloud Users in Microsoft Entra ID
### Step 1 – Open Entra Admin Center

Sign in to the Microsoft Entra Admin Center Links to an external site. using your cloud admin account.

### Step 2 – Create the First User

Navigate as follows:

### Microsoft Entra ID → Users → All users → New user → Create new user

Enter the required details for Alice Morgan (UPN, display name, job title, department).
Allow Entra to auto-generate a password.

Step 3 – Create Remaining Users

Repeat the steps for:

- Brian

- Carla

- Daniel

- Evelyn

using the table provided above.

### Checkpoint 1 — User Creation

Upload a screenshot showing all five pilot users under All users.

# Task 2 – Create Department-Based Security Groups (Assigned & Dynamic)
You will create five groups total—three Assigned and two Dynamic—to model NewVue Health’s departmental structure in the cloud.

### Part A — Assigned Groups

Create these groups manually:

- Administration_Cloud

- Human_Resources_Cloud

- IT_Operations_Cloud

# Steps:

1. Go to Microsoft Entra ID → Groups → All groups → New group

2. Group type: Security

3. Membership type: Assigned

4. Enter the group name

5. Click Create

### Part B — Dynamic Groups

Create two Dynamic User groups:

- Clinical_Services_Cloud

- Finance_Cloud

### Steps:

1. Go to Microsoft Entra ID → Groups → All groups → New group

2. Group type: Security

3. Membership type: Dynamic User

4. Add a dynamic rule:

Clinical Services Rule
```bash
(user.department -eq "Clinical Services")
```
Finance Rule
```bash
(user.department -eq "Finance")
```
5. Save and create each group.

### Checkpoint 2 — Group Creation

Upload:

- One screenshot showing all five groups

- One screenshot showing the dynamic rule for either Clinical or Finance

# Task 3 – Add Users to Groups
###Part A — Assigned Groups

Add the users manually:


| **Group Name**	| **Member** |
| :---: | :---: |
| Administration_Cloud	| Alice Morgan |
| Human_Resources_Cloud	| Carla Johnson |
| IT_Operations_Cloud	| Daniel Kim |

Table shows Assigned Groups

### Steps:

1. Go to Groups → [Group Name] → Members → Add members

2. Search and select the appropriate user

3. Save

### Part B — Dynamic Groups (Automatic Membership)

These groups populate automatically based on the user’s Department attribute.


|**Dynamic Group** |	**Auto Member** |
| :---: | :---: |
| Clinical_Services_Cloud	| Brian Patel |
| Finance_Cloud	Evelyn |  Rodriguez |

Table shows Dynamic Groups

### Checkpoint 3 — Group Membership

Upload:

- Screenshot of the Members tab of one Assigned group

- Screenshot of the Members tab of one Dynamic group showing the auto-added user

# Task 4 – Assign Microsoft 365 E5 Licenses
### Step 1 – Open Microsoft 365 Admin Center

Go to Microsoft 365 Admin Center Links to an external site.

### Step 2 – Verify License Availability

Navigate to Billing → Licenses and confirm E5 licenses are available.

### Step 3 – Assign Licenses

1. Go to Users → Active users

2. Select each pilot user

3. Open the Licenses and apps tab

4. Assign Microsoft 365 E5

5. Save changes

### Checkpoint 4 — License Assignment

Upload at least two screenshots showing pilot users with E5 licenses assigned.

# Task 5 – Verify Cloud Identity Configuration
### Step 1 – Open User Profile in Entra ID

Go to Microsoft Entra ID → Users → All users, select a user (e.g., Daniel Kim).

### Step 2 – Verify License Assignment

Open the Licenses tab and confirm Microsoft 365 E5 is assigned.

### Step 3 – Verify Group Membership

Open the Groups tab and confirm:

- The Assigned group (e.g., IT_Operations_Cloud)

- Any Dynamic group memberships (if applicable)

### Checkpoint 5 — Identity Verification

Upload:

- Screenshot of the user’s Licenses tab

- Screenshot of the user’s Groups tab

# Task 6 – Sign-In Validation
This step ensures the created identities can authenticate into Microsoft 365 after license assignment.

### Step 1 – Use a Private Browser Window

Open an InPrivate/Incognito session.

### Step 2 – Sign in as a Pilot User

Go to the Azure Portal Links to an external site.
Sign in as one user (e.g., daniel.kim@<yourtenant>.onmicrosoft.com).

### Step 3 – Complete Password Reset

Follow the prompts to set a new password.

### Step 4 – Validate Access

Confirm that Microsoft 365 services load (Outlook, OneDrive, Teams, SharePoint, etc.).

### Checkpoint 6 — Sign-In Verification

Upload a screenshot of the Microsoft 365 home page after successful sign-in.






































































































