# Project Phase: Implementing DNS Redundancy and Publishing Internal Web Services

#Background
In the ongoing NewVue Health Infrastructure Modernization Project, you have implemented critical infrastructure services such as Active Directory, Group Policy, and File Services. The next phase of the project focuses on enhancing name resolution reliability and internal resource accessibility.

Currently, NV-DC1 functions as the primary DNS server for the newvue.local domain, handling all internal hostname resolution.
To strengthen availability and provide fault tolerance, you will install and configure DNS on NV-DC2 to serve as a secondary DNS server.
This ensures that if NV-DC1 becomes unavailable, name resolution continues seamlessly across the network.

In addition, you will publish internal web resources through DNS:

- Windows Admin Center (WAC), accessible via a friendly name: `https://wac.newvue.local`

- An internal intranet website, accessible via: `http://intranet.newvue.local`

The intranet site will be deployed to NV-DC2 using Internet Information Services (IIS).
You will use a prebuilt NewVue Health intranet package provided to you as part of this assignment.

# Purpose of the Assignment
This assignment helps you understand how DNS supports enterprise reliability and service accessibility.
By completing this activity, you will:

- Learn how to install, configure, and validate a secondary DNS server.

- Gain practical experience creating custom DNS records for internal services.

- Deploy and test a local website hosted on IIS and published through DNS.

- Strengthen your understanding of redundancy, fault tolerance, and internal name resolution in Windows Server environments.

Completing this exercise builds your confidence in managing DNS and web services—core skills for a systems administrator supporting enterprise networks.

# Tasks and Steps
# Task 1 – Install and Configure DNS on NV-DC2
### Step 1 – Add the DNS Role

1. Log on to NV-DC2 as newvue\Administrator.

2. Open Server Manager → Manage → Add Roles and Features.

3. Select DNS Server → click Next → Install.

4. Wait for the installation to complete.

Checkpoint 1: Screenshot showing DNS Server role selected before installation.
Checkpoint 2: Screenshot showing installation completion summary on NV-DC2.

### Step 2 – Create a Secondary Zone

1. On NV-DC2, open DNS Manager.

2. Expand the server node → right-click Forward Lookup Zones → New Zone.

3. Select Secondary Zone → click Next.

4. Zone Name: `newvue.local` → click Next.

5. Specify the Master DNS Server as NV-DC1 (enter its IP address).

6. Click Finish to complete the wizard.

7. Verify that the zone replicates from NV-DC1 and displays host records.

Checkpoint 3: Screenshot of the `newvue.local` zone on NV-DC2 showing replicated records.

# Task 2 — Publish DNS Records for Internal Web Services
### Step 1 – Create an A Record for the Intranet Site

1. On NV-DC1, open DNS Manager → Forward Lookup Zones → newvue.local.

2. Right-click and select New Host (A or AAAA).

3. Name: `intranet`

4. IP Address: IP address of NV-DC2 (hosting IIS).

5. Check Create associated pointer (PTR) record → click Add Host.

Checkpoint 4: Screenshot showing the `intranet` A record in the DNS zone.

# Step 2 – Create an A Record for WAC

If you installed Windows Admin Center on NV-DC2, use NV-DC2’s IP address. If it is hosted elsewhere, use that host’s IP address.

1. On NV-DC1, right-click the newvue.local zone → select New Host (A or AAAA).

2. Name: `wac`

3. IP Address: IP address of the host running Windows Admin Center.

4. Check Create associated pointer (PTR) record → click Add Host.

Checkpoint 5: Screenshot showing the `wac` A record in the DNS zone.

### Step 3 – Validate DNS Resolution

1. On both NV-DC1 and NV-DC2, open Command Prompt and run:
```bash
ipconfig /flushdns nslookup intranet.newvue.local nslookup wac.newvue.local
```

2. Confirm both names resolve correctly.

Checkpoint 6: Screenshot showing `nslookup` results for both hostnames.

# Task 3 – Deploy the Intranet Site on NV-DC2
# Step 1 – Install IIS

1. Log on to NV-DC2 as newvue\Administrator.

2. Open Server Manager → Manage → Add Roles and Features.

3. Select Web Server (IIS) → click Next → Install.

4. Verify that installation completes successfully.

Checkpoint 7: Screenshot showing IIS installation completion summary.

### Step 2 – Deploy the Provided Website

1. Download the provided intranet site package:
newvue_intranet_site.zip Download newvue_intranet_site.zip

2. Copy the zip file to NV-DC2.

3. Extract contents to C:\inetpub\wwwroot (replacing any default files).

4. Open a browser on NV-DC2 and navigate to:
```bash
http://localhost
```

Confirm that the NewVue Health – Internal Web Portal loads successfully.

Checkpoint 8: Screenshot of the intranet homepage on NV-DC2.

### Step 3 – Test Access via DNS

1. From NV-DC1, open a web browser and go to:
```bash
http://intranet.newvue.local
```
Confirm the same homepage loads via DNS.

2. Also test the WAC record by entering:
```bash
https://wac.newvue.local
```
Accept the certificate warning if prompted.

Checkpoint 9: Screenshot showing both the intranet and WAC pages accessed by DNS name.

# Task 4 – Validate DNS Redundancy
### Step 1 – Simulate Primary DNS Outage

1. Temporarily disable NV-DC1’s network adapter or shut down NV-DC1.

2. On NV-DC2, verify that DNS services continue running and zone data remains available.

Checkpoint 10: Screenshot of DNS Manager on NV-DC2 showing the active `newvue.local` zone while NV-DC1 is offline.

### Step 2 – Verify Name Resolution

1. On NV-DC2, open Command Prompt and run:
```bash
ipconfig /flushdns nslookup intranet.newvue.local nslookup wac.newvue.local
```

2. Confirm both names resolve successfully from NV-DC2 while NV-DC1 is unavailable.

Checkpoint 11: Screenshot showing `nslookup` results resolving via NV-DC2.





















































































