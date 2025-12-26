# Project Phase: Configuring Storage Replica for Data Resiliency.

# Background
In the ongoing NewVue Health Infrastructure Modernization Project, you have successfully configured centralized file storage and access control on NV-FS01.
The next stage of the project focuses on ensuring data resiliency and business continuity in the event of hardware failure or service disruption.

To achieve this, you will implement Storage Replica to replicate the contents of the NewVueData (D:) volume on NV-FS01 to the DC1_Data (E:) volume on NV-DC1.
This configuration ensures that both servers maintain identical copies of the departmental data hosted under D:\Departments, allowing recovery with minimal downtime if the primary file server becomes unavailable.

In this setup:

- NV-FS01 will function as the source server, hosting the active departmental data.

- NV-DC1 will serve as the destination server, maintaining the synchronized replica of that data.

This implementation represents an essential component of the proof-of-concept network, demonstrating how replication can be used to enhance reliability and fault tolerance in enterprise environments.

# Purpose of the Assignment
This assignment is designed to deepen your understanding of how organizations maintain data integrity and business operations in the face of system failures.
By implementing Storage Replica, you will gain hands-on experience with one of the core technologies that support high availability and disaster recovery in modern enterprise networks.

Completing this activity will help you:

- Build the technical confidence to configure server replication in real-world IT environments.

- Understand how synchronous replication ensures data consistency between servers.

- Strengthen your ability to design and maintain systems that support business continuity and fault tolerance.

- Recognize the importance of data resiliency as part of a complete infrastructure management strategy.

This experience reinforces your readiness as a systems administrator to implement solutions that protect organizational data, minimize downtime, and maintain service reliability.

# Objectives
By the end of this activity, you will be able to:

- Install and verify the Storage Replica feature on Windows Server 2022.

- Prepare and configure dedicated data and log volumes on each server.

- Create WinRM HTTPS listeners on both servers for secure remote management.

- Export and import self-signed certificates between NV-FS1 and NV-DC1 to establish mutual trust.

- Install and configure Windows Admin Center (WAC) for centralized management.

- Configure synchronous replication between the two servers.

- Validate data synchronization and replication health.

- Simulate and observe system recovery during server downtime.

# Tasks and Steps
# Task 1 – Install and Verify the Storage Replica Feature
### Step 1 – Install on NV-FS01

1. Log on to NV-FS01 as newvue\Administrator.

2. Open Server Manager → Manage → Add Roles and Features.

3. In the wizard, navigate to the Features section.

4. Select Storage Replica → click Next → Install.

5. Wait for installation to complete.

Checkpoint 1: Screenshot showing Storage Replica selected in the wizard on NV-FS01.
Checkpoint 2: Screenshot showing installation completed successfully.

### Step 2 – Install on NV-DC1

1. Repeat the same installation steps on NV-DC1.

2. After completion, confirm that Storage Replica appears under Installed Features in Server Manager.

Checkpoint 3: Screenshot showing Storage Replica listed under installed features on NV-DC1.

# Task 2 – Prepare Volumes for Replication
Storage Replica requires two volumes per server — one for data and one for log storage.

# Step 1 – Attach and Initialize Additional Disks

In VirtualBox, attach two new 50 GB disks to each server:

- DataDisk

- LogDisk

2. Start each VM, open Disk Management, initialize both disks (GPT), and create the following volumes:

| Server	Volume	| Drive Letter |	Label |	Purpose |
| :---: | :---: | :---: | :---: |
| NV-FS1	| NewVueData	| E:	| NewVueData	Source data volume |
| NV-FS1	| FS1_Log | 	F:	| FS1_Log	Log volume |
| NV-DC1	| DC1_Data | 	E: |	DC1_Data	Destination data volume |
| NV-DC1	| DC1_Log	 | F:	 | DC1_Log	Log volume |

Table showing Storage for Storage Replica

3. Ensure all volumes are NTFS-formatted and show Healthy status.

Checkpoint 4: Screenshot of Disk Management on both servers showing data and log volumes.

### Step 2 – Verify Data Folder Structure on NV-FS1

1. Confirm that E:\Departments exists with departmental subfolders.

2. This folder will serve as the dataset to replicate.

Checkpoint 5: Screenshot of E:\Departments on NV-FS1.

# Task 3 – Configure WinRM over HTTPS and Mutual Certificate Trust
Before installing Windows Admin Center, both servers must be configured to communicate securely using WinRM over HTTPS.
You will create self-signed certificates, set up WinRM listeners, and establish mutual trust between NV-FS1 and NV-DC1.

### Step 1 – Create a Self-Signed Certificate and HTTPS Listener on Each Server

### On NV-DC1
```bash
New-SelfSignedCertificate -DnsName "nv-dc1.newvue.local" -CertStoreLocation "Cert:\LocalMachine\My"
```

### Retrieve the certificate thumbprint
```bash
Get-ChildItem -Path Cert:\LocalMachine\My | Where-Object { $_.Subject -like "*nv-dc1*" }
```
### Create the WinRM HTTPS listener
```bash
winrm create winrm/config/Listener?Address=*+Transport=HTTPS '@{Hostname="nv-dc1.newvue.local";CertificateThumbprint="THUMB"}'
```

Replace THUMB with your actual certificate thumbprint.

### Verify the listener
```bash
winrm enumerate winrm/config/Listener
```

### Open firewall port 5986
```bash
New-NetFirewallRule -Name "WinRM-HTTPS" -DisplayName "WinRM over HTTPS" -Protocol TCP -LocalPort 5986 -Action Allow
```
### On NV-FS1
```bash
New-SelfSignedCertificate -DnsName "nv-fs1.newvue.local" -CertStoreLocation "Cert:\LocalMachine\My"
```
### Retrieve the certificate thumbprint
```bash
Get-ChildItem -Path Cert:\LocalMachine\My | Where-Object { $_.Subject -like "*nv-fs1*" }
```
### Create the WinRM HTTPS listener
```bash
winrm create winrm/config/Listener?Address=*+Transport=HTTPS '@{Hostname="nv-fs1.newvue.local";CertificateThumbprint="THUMB"}'
```
Replace THUMB with your actual certificate thumbprint.

### Verify the listener
```bash
winrm enumerate winrm/config/Listener
```

### Open firewall port 5986
```bash
New-NetFirewallRule -Name "WinRM-HTTPS" -DisplayName "WinRM over HTTPS" -Protocol TCP -LocalPort 5986 -Action Allow
```
### Step 2 – Export Self-Signed Certificates

Use MMC to export the self-signed certificates from each server.

### On NV-DC1

1. Run mmc → File → Add/Remove Snap-in → Certificates → Computer account → Finish → OK.

2. Navigate to Personal → Certificates.

3. Locate the certificate for nv-dc1.newvue.local.

4. Right-click → All Tasks → Export → No private key → DER (.cer).

5. Save to:
```bash
C:\Temp\NV-DC1-WinRM.cer
```
### On NV-FS1

Repeat the same steps and export nv-fs1.newvue.local to:

```bash
C:\Temp\NV-FS1-WinRM.cer
```

### Step 3 – Exchange and Import Certificates

Copy `NV-DC1-WinRM.cer` to NV-FS1 and `NV-FS1-WinRM.cer` to NV-DC1.

### On NV-FS1

Import NV-DC1’s certificate into Trusted Root Certification Authorities:

1. Run mmc → Certificates → Computer account.

2. Right-click Trusted Root Certification Authorities → Certificates → All Tasks → Import.

3. Browse to and import:
```bash
C:\Temp\NV-DC1-WinRM.cer
```
### On NV-DC1

Import NV-FS1’s certificate into Trusted Root Certification Authorities:

1. Run mmc → Certificates → Computer account.

2. Right-click Trusted Root Certification Authorities → Certificates → All Tasks → Import.

3. Browse to and import:
```bash
C:\Temp\NV-FS1-WinRM.cer
```
### Step 4 – Verify HTTPS Connectivity

### On NV-FS1
```bash
Test-NetConnection nv-dc1.newvue.local -Port 5986
Test-WSMan nv-dc1.newvue.local -UseSSL
```
### On NV-DC1
```bash
Test-NetConnection nv-fs1.newvue.local -Port 5986
Test-WSMan nv-fs1.newvue.local -UseSSL
```

Checkpoint

- Screenshot of the HTTPS listener on both servers.

- Screenshot showing each server’s certificate imported into the other’s Trusted Root Certification Authorities store.

- Screenshot showing successful `Test-WSMan -UseSSL` results on both servers.

# Task 4 – Install and Configure Windows Admin Center on NV-FS1 
1. Download WAC from https://aka.ms/WACDownload Links to an external site..

2. Install on NV-FS1 and accept default HTTPS settings.

3. Browse to
```bash
https://nv-fs1.newvue.local
```
and log in as `newvue\administrator`.

Checkpoint 7: Screenshot showing successful WAC login.

### Add Servers in WAC

- Click + Add → Windows Server.

- Choose Search Active Directory, type nv-dc1, then Add.

- Add nv-fs1 as well.

- Verify both show Status: Online.

Checkpoint 8: Screenshot showing both servers connected in WAC.

# Task 5 – Create and Configure the Replication Partnership
In WAC on NV-FS1 → Storage Replica → Set up new partnership

- Source Server: NV-FS1

- Source Data Volume: E:\

- Source Log Volume: F:\

- Destination Server: NV-DC1

- Destination Data Volume: E:\

- Destination Log Volume: F:\

- Credentials: `newvue\administrator`

- Partnership Name: NewVueReplica

Checkpoint 9: Screenshot of configuration summary.
Checkpoint 10: Screenshot confirming successful creation.

# Task 6 – Verify and simulate failover
 
### Step 1 – Monitor Replication Health

Confirm Healthy / Continuously Replicating in WAC.
Checkpoint 11: Screenshot of replication health.

### Step 2 – Validate Replication

1. On NV-FS1, create
```bash
E:\Departments\Replication_Test
```
2. In WAC → NV-FS1 → Storage Replica → Partnerships, verify:

- Replication Status = Continuously Replicating

- Mode = Synchronous

- Direction = NV-FS1 → NV-DC1

- No errors reported.

3. (Optional – View replicated data)

- In Partnerships, click Switch direction.

- NV-DC1 becomes the source; its E: volume becomes accessible.

- Open E:\Departments on NV-DC1 and confirm Replication_Test is present.

- Switch direction again to restore NV-FS1 as source.

4. (PowerShell Verification)
```bash
Get-SRGroup
Get-SRPartnership
```
Confirm: State = Continuously Replicating, Mode = Synchronous, Error = 0.

Checkpoint 12:

- Screenshot of Partnerships view.

- Screenshot of `E:\Departments` on NV-DC1 after switch.

- Screenshot of `Get-SRGroup and Get-SRPartnership` output.

### Step 3 – Simulate Failure and Confirm Read-Only Access

1. Simulate failure: Disable NV-FS1’s network adapter or shut down the VM.

2. Verify replica accessibility on NV-DC1:

- In Disk Management, ensure E: (DC1_Data) is Online.

- Open File Explorer → E:\Departments and verify data is visible.

3. Test read-only state:

- Open existing files – they should open.

- Attempt to create, rename, or delete a file – you should receive Access Denied.

4. PowerShell confirmation:
```bash
Get-SRGroup
```
Check that replication is Paused (if NV-FS1 is offline) or still Continuously Replicating.

5. Restore normal operation:

- Power on or reconnect NV-FS1.

- In WAC → Storage Replica → Partnerships, verify status returns to Continuously Replicating with NV-FS1 as source.

Checkpoint 13:

- Screenshot showing E: online on NV-DC1.

- Screenshot of Access Denied message.

- Screenshot of Get-SRGroup output.

- Screenshot showing replication resumed after recovery.






























































