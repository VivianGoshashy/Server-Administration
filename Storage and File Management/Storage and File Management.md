# Project Phase: Storage and File Management

# Background
As part of the ongoing NewVue Health Infrastructure Modernization Project, additional storage capacity is being provisioned to support secure departmental file management.
To accommodate growing data needs, a new 50 GB virtual hard disk will be attached to both NV-DC1 and NV-FS01.

- NV-DC1 will use the additional disk for future services such as DFS replication, shadow copies, and backup storage.

- NV-FS01 will serve as the organization’s central file server, hosting the departmental data repository that mirrors NewVue Health’s organizational structure.

Once the new disk is attached and initialized, students will create a folder hierarchy under NV-FS01 that aligns with the Active Directory organizational units (OUs) and group structure previously configured.
Each department and its respective functional units will have isolated access to their folders through NTFS permissions, demonstrating proper implementation of the principle of least privilege.

This activity ensures that NewVue Health’s storage infrastructure is logically aligned, scalable, and ready for data sharing and protection configurations to be completed in subsequent labs.

# Purpose of the Assignment
This assignment provides an opportunity to strengthen practical skills in server storage management and data access control within an enterprise network environment.
By completing this exercise, students will gain firsthand experience configuring and preparing additional storage in a virtualized Windows Server environment, building logical folder hierarchies that mirror organizational structure, and securing data through effective use of NTFS permissions.

Through this activity, students will:

- Develop a deeper understanding of how storage provisioning supports enterprise data management.

- Learn how to translate an organization’s business and security model into a functional file system design.

- Build confidence in setting up and verifying role-based access control for departmental and user-level resources.

- Recognize how proper folder design and permission planning lay the foundation for reliable and secure file services in real-world deployments.

This exercise builds foundational skills for managing enterprise file servers and applying least-privilege access models in professional IT environments.

Objectives
- Add and initialize new storage disks in VirtualBox.

- Configure partitions and format the drives using NTFS.

- Create a folder hierarchy that reflects the organization’s departmental and functional structure.

- Apply NTFS permissions to control access by department and functional role.

- Verify access using domain user accounts.

# Tasks and Steps
# Task 1 – Attaching and Initializing New Storage Drives
### Step 1 – Add a New 50 GB Virtual Hard Disk in VirtualBox
Perform the following steps for both NV-DC1 and NV-FS01:

1. Open Oracle VirtualBox Manager.

2. Select the virtual machine (e.g., NV-DC1).

3. Click Settings → Storage.

4. Under the SATA Controller, click the Add Hard Disk icon (disk with a plus sign).

5. In the Hard Disk Selector, click Create.

6. In the “Create Virtual Hard Disk” window:

- Leave the file type and dynamically allocated settings as defaults.

- Set the size to 50.00 GB.

- Click Finish.

7. The new disk will appear under the SATA Controller.

8. Select the disk, click Choose, then OK to attach it.

Checkpoint 1: Screenshot of the Create Virtual Hard Disk window showing 50 GB selected for NV-DC1.
Checkpoint 2: Screenshot of the Storage Settings window showing the new disk attached to NV-DC1.
Checkpoint 3: Screenshot of the Create Virtual Hard Disk window showing 50 GB selected for NV-FS01.
Checkpoint 4: Screenshot of the Storage Settings window showing the new disk attached to NV-FS01.

### Step 2 – Initialize and Format the New Disk on NV-DC1
1. Start NV-DC1 and log in as newvue\Administrator.

2. Open Server Manager → Tools → Computer Management → Disk Management.

3. When prompted, select GPT (GUID Partition Table).

4. Right-click the unallocated disk → New Simple Volume.

5. Assign drive letter E: → format with NTFS → label the volume DC1_Data → click Finish.

6. Verify the new volume in File Explorer.

Checkpoint 5: Screenshot of Disk Management on NV-DC1 showing Disk 1 (E:) initialized and formatted as DC1_Data.

### Step 3 – Initialize and Format the New Disk on NV-FS01
1. Log on to NV-FS01 as newvue\Administrator.

2. Open Disk Management.

3. Initialize the new disk as GPT.

4. Right-click unallocated space → New Simple Volume → assign drive letter D:.

5. Format with NTFS, label as NewVueData, and click Finish.

Checkpoint 6: Screenshot of Disk Management on NV-FS01 showing Disk 1 (D:) initialized and formatted as NewVueData.

# Task 2 – Creating the Departmental Folder Structure
### Step 1 – Create the Base Directory

1. On NV-FS01, open File Explorer → D:\.

2. Create a new folder named Departments.

### Step 2 – Create Department Folders

Inside D:\Departments, create the following five folders:

- Administration

- Clinical_Services

- Human_Resources

- IT_Operations

- Finance

### Step 3 – Create Functional Unit Folders

Within each department folder, create subfolders for the corresponding functional units:

| Department	| Functional Unit Folders |
| :---: | :---: |
| Administration	| Admin_Managers, Admin_Clerks, Admin_Executives |
| Clinical_Services | 	Clinical_Doctors, Clinical_Nurses, Clinical_Assistants |
| Human_Resources	| HR_Managers, HR_Recruiters, HR_Assistants |
| IT_Operations	| IT_Network, IT_Security, IT_Applications | 
| Finance	| Finance_Accountants, Finance_Auditors, Finance_Analysts | 

NewVue Folder Structure

Checkpoint 7: Screenshot of D:\Departments showing all department folders.
Checkpoint 8: Screenshot of one department folder (e.g., Finance) showing its subfolders.

# Task 3 – Sharing the Departments Folder
1. Right-click D:\Departments → Properties → Sharing → Advanced Sharing.

2. Check Share this folder → share name: Departments.

3. Click Permissions → remove Everyone.

4. Add Authenticated Users → assign Read permission → click OK.

5. Click Apply → OK to confirm.

Checkpoint 9: Screenshot of Advanced Sharing window showing Read permissions for Authenticated Users.

# Task 4 – Setting NTFS Permissions
### Step 1 – Root Folder Permissions

1. Right-click D:\Departments → Properties → Security → Advanced.

2. Add:

- Principal: Domain Users

- Type: Allow

- Applies to: This folder only

- Permissions: Read & Execute

3. Click OK.

### Step 2 – Department Folder Permissions

Each department folder should allow users to browse into it, but not store files directly.
To achieve this, apply Read & Execute permissions to the corresponding departmental group so that users can view their department folder and access only their functional subfolders.

1. Right-click a department folder (e.g., D:\Departments\Finance) → Properties → Security → Advanced → Add.

2. Add the matching departmental group (e.g., Finance_Grp).

3. Set Permissions: Read & Execute.

4. Set Applies to: This folder only → click OK.

Repeat this configuration for all departments:

- Administration_Grp

- Clinical_Services_Grp

- Human_Resources_Grp

- IT_Operations_Grp

- Finance_Grp

Checkpoint 10: Screenshot showing one department folder (e.g., Finance) with Read & Execute permissions for its departmental group.

### Step 3 – Functional Role Folder Permissions

Within each departmental folder, assign permissions to the functional unit groups so that only members of each unit can access their corresponding subfolder.
All functional unit groups should have Modify permission on their own folder and no access to other units’ folders.

Example pattern:

- Admin_Managers_Grp → Modify on D:\Departments\Administration\Admin_Managers

- Clinical_Nurses_Grp → Modify on D:\Departments\Clinical_Services\Clinical_Nurses

- Finance_Analysts_Grp → Modify on D:\Departments\Finance\Finance_Analysts

Note: Access should become progressively restrictive — all users can see the root, departments are read-only entry points, and functional groups can modify only their assigned subfolder.

Checkpoint 11: Screenshot showing NTFS permissions for one functional folder with Modify access for the correct functional group.

# Task 5 – Verifying Access
1. Log on to NV-CL1 using different test user accounts from various functional groups.

2. Open File Explorer → access:
```bash
\\NV-FS01\Departments
```

3. Verify:

- All users can open the Departments root folder.

- Each user can open only their department’s folder.

- Within that department, each user can open only their own functional folder.

Checkpoint 12: Screenshot showing access results for at least one user (successful or denied as expected).




















































