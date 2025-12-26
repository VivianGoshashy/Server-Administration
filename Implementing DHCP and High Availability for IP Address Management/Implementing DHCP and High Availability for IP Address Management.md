# Project Phase: Implementing DHCP and High Availability for IP Address Management

# Background
As the modernization of NewVue Health’s network infrastructure continues, we now move on to the implementation of Dynamic Host Configuration Protocol (DHCP) services to streamline and automate IP address management across the internal network.
The goal is to centralize control of IP allocation and improve reliability by configuring DHCP failover between NV-DC1 and NV-DC2.
The client workstation (NV-CL1) will serve as a test endpoint to verify that address leasing and failover function as expected.

Currently, the VirtualBox NAT Network DHCP service provides IP addresses to virtual machines.
To transition to an enterprise-style configuration managed entirely within Windows Server, this external DHCP service will first be disabled, allowing NV-DC1 to function as the primary DHCP server.
Once configured, DHCP failover will be implemented with NV-DC2 to provide redundancy, ensuring uninterrupted service even if one server becomes unavailable.

# Purpose of the Assignment
This assignment provides hands-on experience in deploying and managing DHCP services within a Windows Server environment.
It builds on prior infrastructure configurations to demonstrate how enterprises automate network addressing and implement high-availability solutions for critical services.

By completing this activity, students will:

- Understand the role of DHCP in network automation and management.

- Learn to configure, authorize, and verify DHCP scopes.

- Implement DHCP failover for continuous IP lease delivery.

- Validate client address assignment and lease renewal within a fault-tolerant configuration.

# Learning Objectives
After completing this lab, students will be able to:

1. Disable external DHCP services to prevent address conflicts.

2. Install and configure the DHCP Server role on Windows Server 2022.

3. Create and authorize an internal DHCP scope.

4. Configure DHCP failover between two servers for redundancy.

5. Verify DHCP lease assignment and renewal on a domain-joined client.

# Tasks and Steps
# Task 1 – Disable VirtualBox NAT Network DHCP
Before deploying Windows DHCP, ensure that the VirtualBox network no longer distributes IP addresses.

1. Open VirtualBox Manager → File → Tools → Network Manager.

2. Select your NAT Network (e.g., NewVue_NAT).

3. Uncheck Enable DHCP.

4. Click Apply or Save to confirm.

💡 Disabling the VirtualBox DHCP service ensures that the Windows Server DHCP role becomes the sole authority for IP address assignment within the network.

Checkpoint 1: Screenshot of VirtualBox NAT Network settings showing DHCP disabled.

# Task 2 – Install and Authorize DHCP Server on NV-DC1
1. Log in to NV-DC1 as newvue\Administrator.

2. Open Server Manager → Manage → Add Roles and Features.

3. In the wizard:

- Choose Role-based or feature-based installation.

- Select DHCP Server.

- When prompted, click Add Features → Next → Install.

4. After installation, click Complete DHCP Configuration from the notifications area.

5. In the wizard:

- Confirm credentials (newvue\Administrator).

- Authorize the server in Active Directory.

- Finish configuration.

Checkpoint 2: Screenshot of DHCP installation and authorization summary on NV-DC1.

# Task 3 – Create a New DHCP Scope on NV-DC1
1. Open Server Manager → Tools → DHCP.

2. Expand NV-DC1 → IPv4.

3. Right-click IPv4 → New Scope.

4. Configure the following parameters in the New Scope Wizard:

- Scope Name: NewVue_Internal

- Start IP Address: 10.0.2.1

- End IP Address: 10.0.2.200

- Subnet Mask: 255.255.255.0

- Default Gateway: 10.0.2.1

- Lease Duration: 8 days

5. When prompted to add Exclusions, specify:

- Start IP: 10.0.2.1

- End IP: 10.0.2.20

These addresses are reserved for servers, network infrastructure, and static resources.

6. Continue to the Router (Default Gateway) screen and confirm 10.0.2.1.

7. On the Domain Name and DNS Servers page, verify that both DNS servers are listed:

- NV-DC1: 10.0.2.2

- NV-DC2: 10.0.2.3

If they are not automatically detected, add them manually.

8. Activate the scope once created.

Checkpoint 3:

- Screenshot showing the Scope Range (10.0.2.1–10.0.2.200).

- Screenshot showing the Exclusion Range (10.0.2.1–10.0.2.20).

- Screenshot showing both DNS Servers (10.0.2.2 and 10.0.2.3) configured in the DHCP scope options.

# Task 4 – Install and Authorize DHCP Server on NV-DC2
To enable DHCP failover, NV-DC2 must also have the DHCP Server role installed and authorized.

1. Log in to NV-DC2 as `newvue\Administrator`.

2. Open Server Manager → Manage → Add Roles and Features.

3. Install the DHCP Server role.

4. After installation, click Complete DHCP Configuration.

5. Authorize NV-DC2 in Active Directory using `newvue\Administrator` credentials.

6. Open Server Manager → Tools → DHCP to verify authorization.

Checkpoint 4: Screenshot showing DHCP installation and authorization summary on NV-DC2.

# Task 5 – Configure DHCP Failover Between NV-DC1 and NV-DC2
1. On NV-DC1, open the DHCP Management Console.

2. Expand IPv4, right-click the NewVue_Internal scope → Configure Failover.

3. Select the NewVue_Internal scope → click Next.

4. Choose NV-DC2.newvue.local as the partner server.

5. Configure failover parameters:

- Relationship Name: NewVue_DHCP_Failover

- Mode: Load Balance (50/50)

- Shared Secret: NewVue123

6. Click Finish → Close.

7. Confirm replication by opening the DHCP console on both servers.

Checkpoint 5:

- Screenshot of the DHCP failover relationship summary showing both servers.

- Screenshot of NV-DC2 console displaying the replicated scope.

# Task 6 – Verify DHCP Client Lease on NV-CL1
1. Log in to NV-CL1 as a domain user.

2. Open Command Prompt → Run as Administrator.

3. Run:
```bash
ipconfig /release ipconfig /renew
```

4. Confirm that the client receives an IP address within the range 10.0.2.21–10.0.2.200.

6. Run:
```bash
ipconfig /all
```
Verify:

- The DHCP Server address matches NV-DC1 or NV-DC2.

- The DNS Server points to NV-DC1 and NV-DC2.

On NV-DC1, open Server Manager → Tools → DHCP → IPv4 → NewVue_Internal → Address Leases.
Verify that the lease record for NV-CL1 is listed, confirming that the DHCP server is tracking and managing the client’s lease allocation.

Checkpoint 6:

- Screenshot of `ipconfig /all` on NV-CL1 showing lease details from DHCP.

- Screenshot of NV-DC1 DHCP console showing NV-CL1 listed under Address Leases.

# Task 7 – Test DHCP Failover
1. On NV-DC1, temporarily stop the DHCP Server service:
```bash
net stop dhcpserver
```

2. On NV-CL1, open Command Prompt and run:
```bash
ipconfig /release ipconfig /renew
```

3. Confirm that NV-DC2 successfully issues the new IP address.

4. Restart the DHCP service on NV-DC1:
```bash
net start dhcpserver
```

5. Verify that scope synchronization resumes.

Checkpoint 7:

- Screenshot of NV-CL1 receiving a lease from NV-DC2 during NV-DC1 downtime.

- Screenshot showing replication restored after NV-DC1 comes back online.


















































































