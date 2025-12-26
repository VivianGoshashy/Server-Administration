# Project Phase: Creating Organizational Units

#Background
Following the successful deployment of the newvue.local domain on NV-DC1, NewVue Health is now ready to establish a logical directory structure for managing users, computers, and groups.
To support centralized management, each department within NewVue Health will be represented as an Organizational Unit (OU) within Active Directory.
This structure will help administrators apply policies, manage permissions, and delegate control more efficiently.

As the Systems Administrator, your task is to design and create an OU hierarchy that mirrors the organizational structure of NewVue Health.

# Purpose of the Assignment
This lab introduces Organizational Units (OUs) — containers that group related objects within Active Directory.
Creating OUs is a critical step toward structured management, security, and policy application. You will Practice multiple admin workflows for creating OUs.

By completing this activity, you will:

- Create OUs for NewVue Health’s core departments.

- Establish consistent sub-OUs for each department.

- Verify the OU hierarchy in Active Directory Users and Computers (ADUC).

- Document and validate the structure for future administrative tasks.

# NewVue Departmental Structure
The table below represents the organizational structure that will guide the creation of your Organizational Units.
Each main OU corresponds to a department and contains three child OUs named Users, Computers, and Functional Units.

| **Department Name** | **OU Name (Main OU)** | **Child OUs** |
| :--- | :---: | :---: |
| Administration | Administration | Users, Computers, Functional Units |
| Clinical Services | Clinical_Services | Users, Computers, Functional Units |
| Finance | Finance | Users, Computers, Functional Units |
| Human Resources   | Human_Resources | Users, Computers, Functional Units |
| IT Operations | IT_Operations | Users, Computers, Functional Units |

Table showing NewVUE Departmental Structure

# Tasks and Steps to Complete the Activity
# Task 1: Create OUs with PowerShell (Administration, Clinical Services)
In this task, you will create the Administration and Clinical_Services OUs—including their Users, Computers, and Functional Units child OUs—using the Active Directory Module for Windows PowerShell, then verify in ADUC.

### Steps

1. Open Windows PowerShell (or “Windows PowerShell ISE”) as Administrator.

2. Start the AD module (usually auto-loaded on a DC):

`Import-Module ActiveDirectory` 

3. Create the Administration OU and child OUs:

```bash
New-ADOrganizationalUnit -Name "Administration" -Path "DC=newvue,DC=local" -ProtectedFromAccidentalDeletion $true

New-ADOrganizationalUnit -Name "Users" -Path "OU=Administration,DC=newvue,DC=local" -ProtectedFromAccidentalDeletion $true

New-ADOrganizationalUnit -Name "Computers" -Path "OU=Administration,DC=newvue,DC=local" -ProtectedFromAccidentalDeletion $true

New-ADOrganizationalUnit -Name "Functional Units"-Path "OU=Administration,DC=newvue,DC=local" -ProtectedFromAccidentalDeletion $true
```

4. Create the Clinical_Services OU and child OUs:

```bash
New-ADOrganizationalUnit -Name "Clinical_Services" -Path "DC=newvue,DC=local" -ProtectedFromAccidentalDeletion $true New-ADOrganizationalUnit -Name "Users" -Path "OU=Clinical_Services,DC=newvue,DC=local" -ProtectedFromAccidentalDeletion $true

New-ADOrganizationalUnit -Name "Computers" -Path "OU=Clinical_Services,DC=newvue,DC=local" -ProtectedFromAccidentalDeletion $true

New-ADOrganizationalUnit -Name "Functional Units" -Path "OU=Clinical_Services,DC=newvue,DC=local" -ProtectedFromAccidentalDeletion $true
```
5. (Optional) Quick PS check:

```bash
Get-ADOrganizationalUnit -LDAPFilter "(ou=Administration)" -SearchBase "DC=newvue,DC=local"

Get-ADOrganizationalUnit -LDAPFilter "(ou=Clinical_Services)" -SearchBase "DC=newvue,DC=local"
```
6. Open ADUC (Server Manager → Tools → Active Directory Users and Computers),  refresh, and expand newvue.local to confirm the new OUs.

### Checkpoints (PowerShell Method)
- Evidence 1: Screenshot of the PowerShell console showing successful New-ADOrganizationalUnit commands for Administration and Clinical_Services (include child OUs).

- Evidence 2: Screenshot of ADUC with newvue.local expanded showing Administration and Clinical_Services (and their Users/Computers/Functional Units sub-OUs).

Tip: Keep Protected from accidental deletion enabled (default above). You can toggle in OU properties later when needed.

# Task 2: Create OUs with ADUC (Finance, Human Resources)
In this task, you’ll use Active Directory Users and Computers (ADUC) to create the Finance and Human_Resources Organizational Units and then add their corresponding child OUs underneath each department.
Each department’s OU will contain three sub-OUs named Users, Computers, and Functional Units.
This nested structure ensures that user accounts, workstations, and departmental resources can be logically separated within each department.

### Steps

1. Open ADUC (Server Manager → Tools → Active Directory Users and Computers).

2. In the left pane, expand newvue.local.

3. Right-click newvue.local → New → Organizational Unit → name it Finance, then click OK.

4. Repeat the process to create another top-level OU named Human_Resources.

5. Expand Finance in the left pane.

- Right-click Finance → New → Organizational Unit, and create the following sub-OUs:

  - Users

  - Computers

  - Functional Units

6. Repeat Step 5 for Human_Resources.

- Right-click Human_Resources → New → Organizational Unit, and create the same three sub-OUs:

  - Users

  - Computers

  - Functional Units

7. When finished, press F5 to refresh the view and confirm that both Finance and Human_Resources have the correct sub-OU structure nested underneath them.

### Checkpoints (ADUC Method)

- Evidence 3: Screenshot of the New Object – Organizational Unit wizard in ADUC while creating one of the OUs (e.g., Finance or Human_Resources).

- Evidence 4: Screenshot of the ADUC console showing the expanded directory tree, clearly displaying the Finance and Human_Resources OUs and their three sub-OUs (Users, Computers, and Functional Units) under each.

# Task 3: Create OUs with ADAC (IT Operations)
In this task, you’ll use the Active Directory Administrative Center (ADAC) to create the IT_Operations Organizational Unit and its three child OUs.
Each sub-OU will be nested directly under IT_Operations to organize the department’s user accounts, computers, and specialized teams.

### Steps

1. Open ADAC (Server Manager → Tools → Active Directory Administrative Center).

2. In the left-hand pane, select your domain (newvue.local).

3. In the middle pane, right-click within the domain workspace or select New → Organizational Unit.

4. In the Create Organizational Unit dialog box, enter the name IT_Operations and click OK.

5. Once the IT_Operations OU is created, double-click to open it.

6. Inside the IT_Operations container, create three additional OUs by repeating the previous step:

- Users – for user accounts and administrative staff in IT Operations.

- Computers – for departmental workstations and servers.

- Functional Units – for specialized IT divisions (e.g., Networking, Security, Support).

7. Refresh the view in ADAC to confirm that the Users, Computers, and Functional Units sub-OUs are nested directly under IT_Operations.

8. Open Active Directory Users and Computers (ADUC) to double-check that the OU hierarchy also appears correctly there.

### Checkpoints (ADAC Method)

- Evidence 5: Screenshot of the ADAC – Create Organizational Unit dialog box or details pane showing the creation of the IT_Operations OU.

- Evidence 6: Screenshot of the ADAC console (or ADUC, if preferred) showing the expanded OU tree under newvue.local, with IT_Operations and its three sub-OUs (Users, Computers, and Functional Units) clearly visible.

### Expected Outcome
- All five departmental OUs exist under newvue.local: Administration, Clinical_Services, Finance, Human_Resources, IT_Operations.

- Each departmental OU contains Users, Computers, Functional Units.

- You captured six evidences: PS console + ADUC view (Task 1), ADUC wizard + expanded tree (Task 2), ADAC create + expanded tree (Task 3).
















