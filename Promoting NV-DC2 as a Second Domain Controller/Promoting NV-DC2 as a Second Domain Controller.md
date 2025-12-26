# Project Phase: Promoting NV-DC2 as a Second Domain Controller

# Background
In our ongoing NewVue Health Infrastructure Modernization Project, we have successfully installed, configured, and tested the first Domain Controller (NV-DC1) for the newvue.local domain. This server currently manages user authentication, security policies, DNS, and organizational units across the network.

Having a single Domain Controller works well for a small setup, but it creates a single point of failure — if NV-DC1 goes offline, users will not be able to sign in or access directory services.
To ensure fault tolerance, load balancing, and high availability, we will now deploy and promote a second Domain Controller (NV-DC2) within the same domain.

NV-DC2 will provide replication, redundancy, and continuity for the Active Directory environment.
Once configured, both Domain Controllers will synchronize changes automatically, ensuring that directory data, user accounts, and policies remain consistent across the network even if one server becomes unavailable. This step strengthens the overall stability and reliability of the NewVue Health network, forming the foundation for future enterprise-level scalability.

# Purpose
This lab is designed to help students develop the skills needed to implement directory service redundancy and understand how Active Directory replication works between multiple Domain Controllers.

By completing this activity, students will:

- Learn how to promote an existing member server (NV-DC2) to a Domain Controller within an existing domain.

- Understand the process of replication and how directory changes are synchronized across Domain Controllers.

- Observe how Global Catalogs, DNS roles, and FSMO operations contribute to domain health and performance.

- Practice verifying replication status and ensuring that both controllers maintain consistent and synchronized data.

Mastering this skill equips students with the ability to design and support resilient AD DS infrastructures, ensuring high availability in enterprise environments like NewVue Health.

# Pre-Lab Requirements
Before starting this lab:

- Ensure NV-DC1 is powered on and functioning correctly.

- NV-DC2 is installed with Windows Server 2022, assigned a static IP, and connected to the same VirtualBox NAT Network.

- Tasks and Steps

# Task 1 – Join NV-DC2 to the Domain
Before promoting NV-DC2, it must first become a member of the existing newvue.local domain.

1. On NV-DC2, open Network & Internet Settings → Ethernet → Edit DNS Settings.

2. Set the Preferred DNS Server to the IP address of NV-DC1 (e.g., 192.168.10.10).

3. Open Command Prompt and verify configuration and name resolution:
```bash
ipconfig /all ping newvue.local
```

4. Open System Properties → Computer Name → Change, select Domain, and enter newvue.local.

5. When prompted, enter the domain admin credentials:
Username: newvue\Administrator

6. A confirmation message (“Welcome to the newvue.local domain”) should appear. Restart NV-DC2.

Evidence 1: Screenshot showing both `ipconfig /all` and ping `newvue.local` results from NV-DC2, confirming proper DNS and successful domain join.

# Task 2 – Install the Active Directory Domain Services (AD DS) Role
1. Log on to NV-DC2 as newvue\Administrator.

2. Open Server Manager → Manage → Add Roles and Features.

3. Select Role-based or feature-based installation → Next.

4. Confirm NV-DC2 as the destination server → Next.

5. Select Active Directory Domain Services (AD DS).

- When prompted, click Add Features → Next → Install.

6. Wait for the installation to complete → Close.

Evidence 2: Screenshot showing the installation progress or completion of Active Directory Domain Services on NV-DC2.

# Task 3 – Promote NV-DC2 to a Domain Controller
1. In Server Manager, click the Notifications flag → Promote this server to a domain controller.

2. Under Deployment Configuration, select Add a domain controller to an existing domain.

3. Enter `newvue.local`, click Change, and authenticate with newvue\Administrator.

4. On the Domain Controller Options screen:

- Uncheck both Domain Name System (DNS) and Global Catalog (GC).

- Leave Read-only Domain Controller (RODC) unchecked.

- Enter a Directory Services Restore Mode (DSRM) password.

5. Accept default paths → Next → Install.

6. NV-DC2 will reboot automatically after the promotion completes.

Evidence 3: Screenshot of the Deployment Configuration screen showing “Add a domain controller to an existing domain.”
Evidence 4: Screenshot of the Domain Controller Options screen with both DNS and Global Catalog unchecked.
Evidence 5: Screenshot of the Review Options or Installation Summary screen before reboot.

# Task 4 – Verify Domain Controller Promotion
1. Log back in to NV-DC2 as newvue\Administrator.

2. Open Server Manager → Dashboard and verify that Active Directory Domain Services (AD DS) appears as an installed role.
(DNS Server should not appear yet.)

3. Open Tools → Active Directory Users and Computers (ADUC) and confirm access to newvue.local.

4. Open Active Directory Sites and Services → Default-First-Site-Name → Servers.

- Both NV-DC1 and NV-DC2 should be listed under the same site.

Evidence 6: Screenshot of Server Manager → Dashboard showing AD DS installed (no DNS role).
Evidence 7: Screenshot of Active Directory Sites and Services showing NV-DC1 and NV-DC2 under the same site.

# Task 5 – Verify Replication Between Domain Controllers
1. On NV-DC2, open Command Prompt and run the following commands:
```bash
repadmin /replsummary
repadmin /showrepl
```
Confirm that replication completed successfully and there are no errors.

2. In Active Directory Users and Computers, confirm that both Domain Controllers can access the same set of OUs and users.
(You may add a test OU on NV-DC1 and verify its appearance on NV-DC2.)

Evidence 8: Screenshot showing successful results of `repadmin /replsummary` or `repadmin /showrepl`.
Evidence 9: Screenshot showing synchronized directory structure between both Domain Controllers in ADUC.

# Task 6 – Post-Replication Review
1. On NV-DC2, open Event Viewer → Windows Logs → Directory Service.

2. Review recent events confirming replication success and domain controller promotion completion.

Evidence 10: Screenshot of Event Viewer → Directory Service log showing successful replication or promotion messages for NV-DC2.




































































































