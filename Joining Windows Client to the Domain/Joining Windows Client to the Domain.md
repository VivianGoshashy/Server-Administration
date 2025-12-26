# Project Phase: Joining Windows Client to the Domain

# Background
With the Active Directory Domain Services (AD DS) infrastructure for NewVue Health now fully established — including users, groups, and departmental OUs — it’s time to connect a Windows 11 client workstation to the new newvue.local domain.

In a real-world enterprise environment, domain membership allows centralized management of users, computers, policies, and security settings.
By joining a client to the domain, employees can log in using their AD credentials, and administrators can apply domain-based policies and access controls consistently across the organization.

In this activity, you will join a Windows 11 client computer to the newvue.local domain and verify successful domain connectivity.

# Purpose of the Assignment
This lab is designed to teach students the practical process of joining a workstation to an Active Directory domain—a critical skill for every System Administrator.

By completing this exercise, students will:

- Understand how domain membership enables centralized authentication and policy enforcement.

- Learn how to configure DNS and network settings to ensure the client can locate the domain controller.

- Practice troubleshooting typical domain join errors, such as DNS misconfiguration or time synchronization issues.

- Gain confidence managing devices in an enterprise environment and preparing them for Group Policy and remote management.

Mastering this skill allows future administrators to manage large environments efficiently and ensure seamless integration between client machines and domain resources.

# Steps and Task
# Task 1 – Verify Network Connectivity
1. On NV-CL1, open Command Prompt.

2. Type the following command and press Enter:
```bash
ping nv-dc1 ip
```
3. You should receive replies from the Domain Controller’s IP address.

- If ping fails, verify that both machines are on the same network and that the firewall allows ICMP.

Evidence 1: Screenshot of the Command Prompt showing successful pings to nv-dc1.

# Task 2 – Configure DNS Settings
1. On NV-CL1, open Settings → Network & Internet → Ethernet → Edit DNS settings.

2. Set DNS configuration to Manual, entering the IP address of NV-DC1 (for example, 192.168.10.10).

3. Open Command Prompt and run both commands below:
```bash
ipconfig /all ping newvue.local
```
- `ipconfig /all` confirms that the DNS server is pointing to the Domain Controller.

- `ping newvue.local` ensures that the domain name resolves correctly to the DC.

Evidence 2: One screenshot showing the results of both commands (`ipconfig /all` and `ping newvue.local`) in the same Command Prompt window.

# Task 3 – Join the Client to the Domain
1. On NV-CL1, open Settings → System → About → Advanced system settings.

2. Under the Computer Name tab, click Change.

3. Select Domain and enter newvue.local.

4. When prompted for credentials, enter:

- Username: newvue\Administrator

- Password: (your domain admin password)

5. A message will appear welcoming you to the domain.

6. Restart the computer when prompted.

Evidence 3: Screenshot of the Computer Name/Domain Changes dialog box showing Domain: `newvue.local` and the “Welcome to the newvue.local domain” message.

# Task 4 – Verify Domain Membership
1. Log in to NV-CL1 using a domain user account (e.g., newvue\lgyamfi).

2. Once logged in, open Settings → System → About, and verify that Domain: newvue.local is displayed.

Evidence 4: Screenshot of either Settings → About or systeminfo output showing Domain: `newvue.local`.

# Task 5 – Verify Computer Object in Active Directory
1. On NV-DC1, open Active Directory Users and Computers (ADUC).

2. Expand `newvue.local` → Computers.

3. Confirm that the NV-CL1 computer object appears in the list.

4. Optionally, right-click NV-CL1 → Properties → Member Of to verify domain association.

Evidence 5: Screenshot of ADUC → Computers showing NV-CL1 listed as a domain member.
















