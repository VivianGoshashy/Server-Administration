# Project Phase: Optimizing Directory Services

# Background
In the ongoing NewVue Health Infrastructure Modernization Project, we now have two Domain Controllers—NV-DC1 and NV-DC2—in the newvue.local domain. Currently, NV-DC1 owns all Flexible Single Master Operations (FSMO) roles, which means it handles several critical, single-instance Active Directory operations.

To improve fault tolerance and distribute directory responsibilities, selected FSMO roles—particularly the Infrastructure Master—will be transferred to NV-DC2. After balancing these roles, we will enable the Global Catalog (GC) on NV-DC2 to provide forest-wide search and authentication capabilities.

This exercise advances the NewVue Health domain toward a fully redundant and balanced Active Directory environment.

# Purpose
This lab is designed to help students build strategic, administrative, and troubleshooting proficiency in managing Active Directory operations across multiple Domain Controllers.

By engaging in this exercise, students are not just performing technical steps—they are developing a practical understanding of how core directory roles and replication services sustain enterprise networks.

Through the process of transferring FSMO roles and enabling the Global Catalog, students learn:

- How to maintain operational continuity by distributing critical AD roles across multiple servers to prevent single points of failure.

- The importance of role placement strategy, ensuring that roles such as the Infrastructure Master operate efficiently within a multi-DC environment.

- How the Global Catalog supports logon authentication and directory searches across the forest, improving scalability and user experience.

- To think like system administrators—anticipating dependencies, balancing workloads, and planning for resilience and recovery.

By mastering these skills, students gain the ability to design and maintain highly available, fault-tolerant directory infrastructures—a key professional competency for modern enterprise environments like NewVue Health.

# Pre-Lab Requirements
Before starting:
- NV-DC1 and NV-DC2 must be powered on and replicating properly.

- You must be logged in as newvue\Administrator on NV-DC2.

# Tasks and Steps
# Task 1 – Verify Current FSMO Role Holders (10 points)
1. On NV-DC2, open Command Prompt or PowerShell and run:

```bash
netdom query fsmo
```
2. Confirm that NV-DC1 is listed as the owner of all five FSMO roles:

- Schema Master

- Domain Naming Master

- RID Master

- PDC Emulator

- Infrastructure Master

Evidence 1: Screenshot of the `netdom query fsmo` output showing NV-DC1 as the role holder for all FSMO roles.

# Task 2 – Transfer FSMO Roles to NV-DC2 and Rebalance Role Placement (35 points)
You will now transfer the domain-wide FSMO roles (RID Master, PDC Emulator, and Infrastructure Master) to NV-DC2, and then use PowerShell to move the RID Master and PDC Emulator back to NV-DC1, leaving only the Infrastructure Master role on NV-DC2.

## A. Transferring Domain-Wide Roles to NV-DC2 Using the GUI
1. Log on to NV-DC2 as newvue\Administrator.

2. Open Active Directory Users and Computers (ADUC).

3. Right-click the domain (newvue.local) → select Change Domain Controller… → choose NV-DC2 → click OK.

4. Right-click the domain name again → select Operations Masters.

5. In the Operations Masters window, three tabs are available: RID, PDC, and Infrastructure.

6. On each tab:

- Click Change, then confirm with Yes to transfer the role from NV-DC1 to NV-DC2.

- Verify that each tab now lists NV-DC2 as the current role holder before moving to the next tab.

7. Close the Operations Masters window when all three roles have been transferred.

Evidence 2a: Screenshot of the RID tab showing NV-DC2 as the current RID Master.
Evidence 2b: Screenshot of the PDC tab showing NV-DC2 as the current PDC Emulator.
Evidence 2c: Screenshot of the Infrastructure tab showing NV-DC2 as the current Infrastructure Master.

## B. Moving Roles Back to NV-DC1 Using PowerShell
Now that NV-DC2 holds all three domain-wide roles, use PowerShell to transfer the RID Master and PDC Emulator back to NV-DC1, leaving only the Infrastructure Master on NV-DC2.

1. Open Windows PowerShell on NV-DC2 (Run as Administrator).

2. Type and execute:
```bash
Move-ADDirectoryServerOperationMasterRole -Identity "NV-DC1" -OperationMasterRole RIDMaster, PDCEmulator
```
3. When prompted, confirm the transfer by typing Y and pressing Enter.

4. Re-run:
```bash
netdom query fsmo
```
to confirm that NV-DC1 now holds the RID Master and PDC Emulator, while NV-DC2 holds only the Infrastructure Master.

Evidence 3: Screenshot of the PowerShell window showing successful transfer of RID and PDC Emulator roles back to NV-DC1.
Evidence 4: Screenshot of the `netdom query fsmo` output showing NV-DC1 as the RID and PDC holder, and NV-DC2 as the Infrastructure Master.

# Task 3 – Enable the Global Catalog on NV-DC2 (30 points)
1. On NV-DC2, open Server Manager → Tools → Active Directory Sites and Services.

2. Expand Sites → Default-First-Site-Name → Servers → NV-DC2 → NTDS Settings.

3. Right-click NTDS Settings → Properties.

4. Check Global Catalog and click Apply → OK.

5. Wait a few minutes for replication to update and register NV-DC2 as a GC.

Evidence 5: Screenshot of NTDS Settings → Properties with Global Catalog checked.
Evidence 6: Screenshot of Active Directory Sites and Services showing NV-DC2 listed as a Global Catalog under NTDS Settings.

# Task 4 – Verify Global Catalog Functionality (10 points)
1. On NV-DC2, open Command Prompt and run:
```bash
nltest /dclist:newvue.local
```
Both NV-DC1 and NV-DC2 should appear, with NV-DC2 marked as GC.

2. Optionally, in Active Directory Users and Computers, perform a search for a user from another OU to confirm GC-based lookups succeed.

Evidence 7: Screenshot of `nltest /dclist:newvue.local` output showing NV-DC2 as a Global Catalog server.



















































