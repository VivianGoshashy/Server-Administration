# Project Phase: Deploy Active Directory Domain Controller
# Scenario Overview
In Week 2, you successfully designed and implemented a proof-of-concept virtual infrastructure for NewVue Health, consisting of:

Two servers designated as domain controllers (NV-DC1 and NV-DC2)

One file server (NV-FS1)

One Windows 11 client (NV-CL1)

Each virtual machine was configured within a VirtualBox NAT Network, assigned static IP addresses, and verified for inter-VM communication.
With the foundation in place and networking tested, NewVue Health is now ready to begin deploying core server roles—starting with Active Directory Domain Services (AD DS) on NV-DC1.

This step will formally establish the NewVue Health domain (newvue.local), which will provide centralized authentication, identity management, and secure resource access across the organization.
As the Systems Administrator, you will lead this implementation.

⚙️ NV-DC1 will serve as the first domain controller. NV-DC2 will be introduced in Week 4 for replication and fault tolerance.

# Purpose of the Assignment
This lab represents the first major service deployment in the NewVue Health modernization project. This will serve as the foundation for centralizing user authentication, enforcing security policies, and organizing company resources. By completing this assignment, you will:

- Install and configure the Active Directory Domain Services (AD DS) role.

- Promote NV-DC1 to the primary domain controller for newvue.local.

- Verify service installation, functionality, and supporting files.

- Document your implementation with screenshots and explanations.

# Tasks and Steps 
# Task 1: Install the AD DS Role
### 1. Open Server Manager
Log in to NV-DC1 as Administrator and launch Server Manager.

### 2. Add Roles and Features
Select Manage → Add Roles and Features → Next on Before You Begin.

### 3. Select Installation Type
Choose Role-based or feature-based installation → Next.

### 4. Select Destination Server
Confirm NV-DC1 is selected → Next.

### 5. Select Server Roles
Check Active Directory Domain Services (AD DS).
When prompted, click Add Features → Next.

### 6. Select Features
Accept defaults → Next.

### 7. Confirm Installation Selections
Review your choices → Install.
Wait for installation to complete → Close.

### Checkpoint – Take the Following Screenshots

- Take a screenshot of the Installation Progress window showing AD DS installation.

- After completion, open Server Manager → Dashboard and capture a screenshot showing Active Directory Domain Services listed under Roles and Server Groups with a green checkmark.

- Confirm that AD DS now appears in the left navigation pane.

# Task 2: Promote the Server to a Domain Controller
1. In Server Manager, click the Notifications flag → Promote this server to a domain controller.

2. Deployment Configuration
Choose Add a new forest.
Enter domain name newvue.local → Next.

### Checkpoint  – Capture Domain Configuration
Take a screenshot of the Deployment Configuration page showing the domain name newvue.local.

3. Domain Controller Options
Set both functional levels to Windows Server 2022.
Keep DNS Server checked.
Enter and confirm a DSRM password → Next.

### Checkpoint - Capture the Domain controller options screen

4. DNS Options
Ignore delegation warning → Next.

5. Additional Options
Verify NetBIOS name = NEWVUE → Next.

6. Paths
Accept default locations → Next.

7. Review Options
Review summary → Next.

8. Prerequisites Check
Wait for verification to complete.

### Checkpoint  – Validate Prerequisites
Capture the Prerequisites Check window showing all checks passed with green status.

9. Click Install to begin promotion. The server will restart automatically after promotion.

###Checkpoint  – Confirm Domain Promotion Post-Reboot
After reboot, log in as newvue\Administrator.
Capture the Server Manager Dashboard showing the domain controller role with green status icons for AD DS and DNS.

# Task 3: Verifying the Deployment
### Test Case 1 – Verify that the AD DS Role is Installed
1. Open Server Manager
- Log in to NV-DC1 using the newvue\Administrator account.
- Open Server Manager from the Start menu.

2. Check the AD DS Role
- In the left pane, click Dashboard.
- Under Roles and Server Groups, confirm that Active Directory Domain Services appears in the list.
- A green check mark indicates that the role is installed and running.

3. Verify AD DS and DNS Services
- In Server Manager, click Tools → Services.
- In the Services window, locate the following:
     - Active Directory Domain Services
     - DNS Server

- Ensure that both show a Status = Running.

### Checkpoint 5 – AD DS and DNS Verification

- Take a screenshot of the Server Manager Dashboard showing AD DS listed and running.

- Take a screenshot of the Services window showing both Active Directory Domain Services and DNS Server in the Running state.

### Test Case 2 – Verify the AD DS Database Files
1. Open File Explorer
- While logged in to NV-DC1, open File Explorer from the taskbar or press Windows + E.

2. Navigate to the AD DS Database Location
- Browse to:
  C:\Windows\NTDS

3. Locate Key AD DS Files
- Confirm that the following files exist:
     - ntds.dit – Main Active Directory database.
     - edb.log – Transaction log for AD DS.
     - edb.chk – Checkpoint file for transaction tracking.

### Checkpoint 6 – Database Verification

- Capture a screenshot of the NTDS folder showing ntds.dit, edb.log, and edb.chk.

- Ensure file extensions are visible in your screenshot for clarity.
