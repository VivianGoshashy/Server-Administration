# Project Phase: Microsoft Entra PowerShell Administration

# Background
As NewVue Health advances through its cloud modernization phase, administrators must develop the ability to manage Microsoft Entra ID using PowerShell. PowerShell enables efficient automation, consistent directory administration, and the ability to perform large-scale identity management tasks beyond what is possible through the graphical interface.

This activity focuses on establishing the foundational PowerShell skills required for cloud identity administration.
Students will prepare their workstation for scripting, install the Microsoft Entra PowerShell module, connect securely to the tenant, and perform introductory identity queries.

These skills form the basis for future work involving cloud identity lifecycle management, device onboarding, and hybrid integration.

# Purpose of the Assignment
The purpose of this lab is to build confidence and proficiency in using Microsoft Entra PowerShell for cloud identity operations.
Students will learn how to prepare their PowerShell environment, install necessary modules, authenticate to their tenant, and query core identity objects.

# Learning Objectives
Students will be able to:

- Prepare their PowerShell environment for Microsoft Entra scripting

- Install and verify the Microsoft.Entra PowerShell module

- Connect securely to their cloud tenant

- Retrieve tenant, domain, user, and group information

- Filter and format identity data using PowerShell

# Tasks and Steps
# Task 1 — Prepare the Workstation for Entra PowerShell
Before installing the Microsoft Entra PowerShell module, the workstation must meet the required prerequisites.

### Step 1 – Download and Install PowerShell 7

Download from Microsoft:
https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.5 Links to an external site. 

Install PowerShell 7 (x64) using default settings, then launch:

### PowerShell 7 (x64)

### Step 2 – Verify PowerShell Version
```bash
$PSVersionTable.PSVersion
```
Displays the version of PowerShell currently running.

### Step 3 – Check for Existing Entra Modules
```bash
Get-Module -Name Microsoft.Entra -ListAvailable
```
Checks whether the Microsoft.Entra module already exists on the system.

### Step 4 – Update PowerShellGet
```bash
Install-Module -Name PowerShellGet -Force -AllowClobber
```
Updates PowerShellGet to support module installation.

### Step 5 – Verify Execution Policy
```bash
Get-ExecutionPolicy -List
```
Displays execution policies configured for the session and system.

### Step 6 – Set Recommended Execution Policy
```bash
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```
Allows execution of locally created scripts within the current user context.

Checkpoint 1

Provide one screenshot showing:

- PowerShell 7 version

- Execution policy

- PowerShellGet update

# Task 2 — Install the Microsoft Entra PowerShell Module
### Step 1 – Install the Module
```bash
Install-Module -Name Microsoft.Entra -Repository PSGallery -Scope CurrentUser -AllowClobber
```
Downloads and installs the Microsoft.Entra PowerShell module.

### Step 2 – Verify Installation
```bash
Get-Module -ListAvailable Microsoft.Entra*
```
Displays the installed Microsoft Entra module packages.

### Checkpoint 2

Screenshot confirming installation and verification of the module.

# Task 3 — Connect to the Microsoft Entra Tenant
### Step 1 – Import the Module
```bash
Import-Module Microsoft.Entra
```
Loads the module into the PowerShell session.

### Step 2 – Connect to the Tenant
```bash
Connect-Entra -Scopes "User.Read.All","Directory.Read.All"
```
Prompts for authentication and connects to the Entra tenant.

### Step 3 – Verify the Connected Context
```bash
Get-EntraContext
```
Displays details about the connected tenant and identity context.

### Checkpoint 3

Screenshot of Get-EntraContext output.

Task 3 — Connect to the Microsoft Entra Tenant

# Task 4 — Retrieve Tenant and Domain Information
### Step 1 – Retrieve Tenant Details
```bash
Get-EntraTenantDetail
```
Returns information about the cloud tenant.

### Step 2 – Retrieve Domain List
```bash
Get-EntraDomain
```
Displays all domains configured in the tenant.

### Checkpoint 4

Submit two screenshots:

- Output from Get-EntraTenantDetail

- Output from Get-EntraDomain

# Task 5 — Query Cloud Users
### Step 1 – List All Users
```bash
Get-EntraUser
```
Returns all cloud users in the directory.

### Step 2 – Display Selected Properties
```bash
Get-EntraUser | Select-Object DisplayName, UserPrincipalName
```
Shows only names and sign-in identifiers.

### Step 3 – Filter Users by Keyword
```bash
Get-EntraUser -SearchString "admin"
```
Returns users whose attributes match the search term.

### Checkpoint 5

Screenshot showing a filtered or property-selected user query.

# Task 6 — Query Cloud Groups
 
### Step 1 – List All Groups
```bash
Get-EntraGroup
```
Returns all cloud groups in the tenant.

### Step 2 – List Groups with Selected Properties
```bash
Get-EntraGroup | Select-Object DisplayName, GroupType
```
Displays essential details for each group.

### Step 3 – Retrieve Group Membership
```bash
Get-EntraGroupMember -GroupId <GroupObjectId>
```
Displays group membership for the specified group.

### Checkpoint 6

Screenshot of group list or group membership details.



















































































































































