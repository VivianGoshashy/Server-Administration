# Project Phase: Creating Users

# Scenario Overview
With NewVue Health’s Active Directory domain and departmental Organizational Units (OUs) now in place, the next phase of the project focuses on creating user accounts.
The Human Resources department has provided a list of 100 employees, including their first and last names and their respective departments. Each employee will be created within the correct OU based on departmental assignment.
As the Systems Administrator, you will demonstrate three different user-creation methods to understand both graphical and command-line approaches to account provisioning.

# Purpose of the Assignment
This lab demonstrates practical methods for user management in a domain environment.
You will learn how to:

- Add users manually through ADUC.

- Create accounts via the command line using dsadd.

- Automate account creation using batch scripts.

- Perform bulk user creation from HR data using Excel-driven DSADD scripts.

# Tasks and Steps to Complete the Activity
# Task 1: Creating a User Account with Active Directory Users and Computers (ADUC)
In this task, you will create a single user account manually using the ADUC interface.

# Steps
1. Log on to NV-DC1 as newvue\Administrator.

2. Open Server Manager → Tools → Active Directory Users and Computers (ADUC).

3. Expand the newvue.local domain and navigate to Administration → Users.

4. Right-click Users → New → User.

5. Complete the New Object – User wizard as follows:

| **User creation** | **parameters** |
| :---: |  :---: |
| Field	| Value | 
| First Name	| Louis |
| Last Name	| Gyamfi | 
| Full Name	| Louis Gyamfi |
| User Logon Name	| LGyamfi |
| Password	 | NewVue@123 |
| Password Options	| User must change password at next logon | 

User creation parameters

6. Click Next → Finish to create the account.

7. Refresh ADUC (F5) and confirm that LGyamfi appears under Administration → Users.

### Checkpoint 1 – Manual User Creation
Take two screenshots:

- One of the New Object – User wizard before completion.

- One of ADUC showing LGyamfi under Administration → Users.

# Task 2: Creating User Accounts Using Command Prompt (DSADD Command)
You will now create five user accounts for the Finance Department using the dsadd command.

### User List – Finance Department

| **Username** | **First Name** |	**Last Name** |
| :---: | :---: | :---: |
| ejohnson	| Emma |	Johnson | 
| cbrown	| Charles	| Brown |
| smiller |	Sophia	| Miller |
| dclark	| Daniel	| Clark |
| awoode	| Abigail	| Woode |

Table containing finance department user list
### Steps

1. Log on to NV-DC1 as newvue\Administrator.

2. Open Command Prompt as Administrator.

3. Type and execute the following commands one by one:

```bash
dsadd user "cn=ejohnson,ou=Users,ou=Finance,dc=newvue,dc=local" -fn Emma -ln Johnson -pwd NewVue@123 -mustchpwd yes

dsadd user "cn=cbrown,ou=Users,ou=Finance,dc=newvue,dc=local" -fn Charles -ln Brown -pwd NewVue@123 -mustchpwd yes

dsadd user "cn=smiller,ou=Users,ou=Finance,dc=newvue,dc=local" -fn Sophia -ln Miller -pwd NewVue@123 -mustchpwd yes

dsadd user "cn=dclark,ou=Users,ou=Finance,dc=newvue,dc=local" -fn Daniel -ln Clark -pwd NewVue@123 -mustchpwd yes

dsadd user "cn=awoode,ou=Users,ou=Finance,dc=newvue,dc=local" -fn Abigail -ln Woode -pwd NewVue@123 -mustchpwd yes
```
4. Verify that each command completes successfully.

5. Open ADUC, expand Finance → Users, and confirm that the new accounts appear.

##3 Checkpoint 2 – DSADD Command Execution

- Screenshot of Command Prompt showing all five executed DSADD commands.

- Screenshot of ADUC → Finance → Users with all new accounts visible.

# Task 3: Semi-Automated User Creation Using a Batch File
You will now automate the creation of another five user accounts for the Administration Department using a batch file named AdministrationList.bat.

### User List – Administration Department


| **Username**	| **First Name** | 	**Last Name** | 
| :---: | :---: | :---: |
| Pgyamfi	 | Pius | 	Gyamfi | 
|tparker	| Tracy	| Parker | 
|mroberts	| Michael	| Roberts | 
| nfrimpong	| Naomi |	Frimpong |
| kdoe	| Kevin	| Doe |

Table shows Administration department user list

### Steps

1. Open Notepad.

2. Type the following command on a single line:

```bash
dsadd user "cn=%1,ou=Users,ou=Administration,dc=newvue,dc=local" -fn %2 -ln %3 -pwd NewVue@123 -mustchpwd yes
```

3. Save the file:

- Go to File → Save As.

- Change Save as type to All Files (.).

- Name it AdministrationList.bat and save it in C:\Scripts.

4. Open Command Prompt as Administrator.

5. Navigate to the script’s folder:

```bash
cd C:\Scripts
```

6. Run the following commands:

```bash
AdministrationList.bat Pgyamfi Pius Gyamfi

AdministrationList.bat tparker Tracy Parker

AdministrationList.bat mroberts Michael Roberts

AdministrationList.bat nfrimpong Naomi Frimpong

AdministrationList.bat kdoe Kevin Doe
```

7. Open ADUC, expand Administration → Users, and confirm that all accounts are present.

### Checkpoint 3 – Batch Automation Evidence

- Screenshot of Command Prompt showing all five executed AdministrationList.bat commands.

- Screenshot of ADUC → Administration → Users displaying the new accounts.

# Task 4: Bulk User Account Creation Using Excel and Command Automation
You will now automate creation of all remaining HR users Download HR users using the provided Bulk User Creation.xlsx  Download Bulk User Creation.xlsx workbook.

### Workbook Structure

| **Worksheet** |	**Purpose** |
| :---: | :---: |
| AddUserInfoHere	| Paste HR user data (First Name, Last Name, Child OU, Primary OU). |
| MasterCreationScriptSource	| Auto-generates DSADD commands. Verify domain paths and OU accuracy here. |
| SaveThisAsATextFile	| Produces the final DSADD commands to be exported as a batch script. |

Table showing workbook structure

### Steps

### Step 1 – Import HR Data

1. Open Bulk User Creation.xlsx.

2. Go to the AddUserInfoHere sheet.

3. Copy all user data from NewVUE User List.xlsx and paste it starting in row 2.

4. Verify column alignment: First Name, Last Name, Child OU, Primary OU.

### Checkpoint 4 – HR Data Import
Screenshot of the AddUserInfoHere sheet showing the pasted HR data.

### Step 2 – Verify Script Generation

1. Switch to the MasterCreationScriptSource sheet.

2. Review the generated DSADD commands for accuracy.

- Ensure all entries include dc=newvue,dc=local.

- Confirm all OU paths match their departments.

### Checkpoint 5 – Master Script Validation
Screenshot of MasterCreationScriptSource with several valid DSADD command lines visible.

### Step 3 – Export and Convert Script

1. Go to SaveThisAsATextFile sheet.

2. Review the completed DSADD command list.

3. Save the file:

- File → Save As → Browse → Save as type: Formatted Text (Space Delimited) (*.prn ).

- Name it BulkUserCreation.txt and save it to C:\Scripts.

4. In File Explorer, rename BulkUserCreation.prn to BulkUserCreation.bat.

### Checkpoint 6 – Script Conversion
Screenshot showing BulkUserCreation.bat in C:\Scripts with the .bat extension visible.

### Step 4 – Execute the Batch Script

1. Open Command Prompt as Administrator.

2. Navigate to C:\Scripts.

```bash
cd C:\Scripts
```

3. Run the script:
```bash
BulkUserCreation.bat
```

4. Observe account creation as each DSADD command executes.

### Checkpoint 7 – Script Execution
Screenshot of Command Prompt showing successful execution messages.

### Step 5 – Validate Accounts

1. Open ADUC.

2. Expand each department (Administration, Clinical Services, Finance, Human Resources, IT Operations).

3. Confirm that all users appear under their Users OU.

### Checkpoint 8 – Directory Validation
Screenshot of ADUC showing IT_Operations expanded with new accounts visible.
