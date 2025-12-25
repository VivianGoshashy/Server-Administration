# Project Phase: Creating The NewVUE Infrastructure

# Background
In Week 1, you submitted a proposal for the server infrastructure of NewVue Health. After careful review, NewVue Management has accepted your proposal and approved the initial phase of implementation.

To begin, you’ve been tasked with building a proof-of-concept virtual network using VirtualBox. This network will simulate the core infrastructure and demonstrate how the proposed design will function in practice.

The virtual network will include:

- Two Domain Controllers (NV-DC1 and NV-DC2) for redundancy and authentication

- One File Server (NV-FS1) for centralized storage

- Due to resource limitations, additional storage will be added to NV-DC2 to support the file server role

- A Windows 11 Client (NV-CL1) to test domain services and file access

# Purpose of the assignment
This lab focuses on the fundamentals of building a virtual environment. By completing it, you will:

- Learn how to create and configure new virtual machines

- Perform windows server and windows client operating system installations

- Use cloning to save time when creating multiple servers.

- Configure static IP addresses and verify basic network communication.

- Test connectivity between multiple VMs and confirm internet access.

# Your Tasks
You will complete the following tasks:

1. Pre-Network Setup: Create a NAT Network in VirtualBox

2. Create and configure NV-DC1 (Windows Server 2022)

3. Clone NV-DC1 to create NV-DC2 and NV-FS1

4. Create a Windows 11 client VM (NV-CL1)

5. Configure network settings for all VMs

6. Test connectivity and ensure internet access

# Task 0: Pre-Network Setup (VirtualBox NAT Network – Recommended)
To ensure both inter-VM communication and internet access, you will create a NAT Network in VirtualBox.

### Steps:
1.	Open VirtualBox Manager
2.	Go to File → Tools → Network → NAT Networks
3.	Click the Create (+) button to create a new NAT Network
4.	Name it: NV-NAT
5.	Ensure DHCP is enabled and IPv4 is supported
6.	Leave other settings as default
7.	Click Apply to save
   
Checkpoint 0: Screenshot of NAT Network settings showing the network name (NV-NAT) and DHCP enabled

# Task 1: Create and Configure NV-DC1

### Phase 1: Create New VM
- Open VirtualBox Manager → Click New
- Name: NV-DC1
- Type: Microsoft Windows
- Version: Windows Server 2022 (64-bit)
  
Checkpoint 1.1: Screenshot of New VM dialog with name and OS version

### Phase 2: Assign Resources
- Memory (RAM): 4096 MB
- CPU: 2 cores
- Hard Disk: VDI, dynamically allocated, 50 GB
  
Checkpoint 1.2: Screenshot of RAM, CPU, and disk settings

### Phase 3: Attach Server ISO Image
- Go to Settings → Storage
- Under Controller: IDE, click the empty disk icon
- Click Choose a disk file → Select Windows Server 2022 ISO
  
Checkpoint 1.3: Screenshot of Storage settings showing ISO attached

### Phase 4: Start and Install Windows Server
- Start the VM and follow the installation wizard:
     - Language: English
     - Edition: Windows Server 2022 Standard (Desktop Experience)
     - Installation type: Custom
     - Select the virtual disk
- Set Administrator password (example: NewVue@2025)
  
Checkpoint 1.4: Screenshot of setup/login screen

### Phase 5: Post-Installation Setup
1. Change hostname to NV-DC1 and restart
2. Run ipconfig to observe DHCP IP details
3. Manually assign the same IP address as static
   - Subnet: 255.255.255.0
   - Gateway: same as DHCP default
   - DNS: Preferred = Gateway, Alternate = (leave blank for now)
4. Run Windows Update
   
Checkpoint 1.5: Screenshot of ipconfig output
Checkpoint 1.6: Screenshot of manual IP configuration
Checkpoint 1.7: Screenshot of Windows Update screen
Task 2: Clone NV-DC1 to Create NV-DC2 and NV-FS1

### Phase 1: Prepare NV-DC1 for Cloning
- Confirm hostname and IP are set
- Shut down VM
  
Checkpoint 2.1: Screenshot of VirtualBox showing NV-DC1 powered off

### Phase 2: Clone to Create NV-DC2
- Right-click NV-DC1 → Clone
- Name: NV-DC2
- MAC Address Policy: Generate new MAC
- Clone Type: Full Clone
  
Checkpoint 2.2: Screenshot of Clone dialog and VirtualBox showing NV-DC2

### Phase 3: Clone to Create NV-FS1
- Right-click NV-DC1 → Clone
- Name: NV-FS1
- MAC Address Policy: Generate new MAC
- Clone Type: Full Clone
  
Checkpoint 2.3: Screenshot of Clone dialog and VirtualBox showing NV-FS1

### Phase 4: Post-Cloning Configuration
- Start each clone, log in, and rename hostnames appropriately (NV-DC2, NV-FS1)
- Run ipconfig to note DHCP-assigned address
- Configure the same address as static, with:
     - Subnet: 255.255.255.0
     - Gateway: DHCP default gateway
     - DNS: Preferred = Gateway, Alternate = NV-DC1’s IP
       
Checkpoint 2.4: Screenshots of hostname change, ipconfig, and static IP

# Task 3: Create Windows 11 Client (NV-CL1)
### Phase 1: Create New VM
- Name: NV-CL1
- Type: Microsoft Windows
- Version: Windows 11 (64-bit)
  
Checkpoint 3.1: Screenshot of New VM dialog

### Phase 2: Assign Resources
- RAM: 4096 MB
- CPU: 2 cores
- Disk: 40 GB
  
Checkpoint 3.2: Screenshot of resource settings

### Phase 3: Attach Windows 11 ISO
- Settings → Storage → Attach Windows 11 ISO
  
Checkpoint 3.3: Screenshot of ISO attached

### Phase 4: Install Windows 11
- Install OS
- Set hostname: NV-CL1
- Create a local user (e.g., NewVueUser)
- Run ipconfig to observe DHCP IP, then configure it as static
- Use NV-DC1’s IP as Alternate DNS

Checkpoint 3.4: Screenshots of desktop, ipconfig, and static IP

# Task 4: Configure Networking and Test Connectivity
1. Verify all VMs are attached to the NV-NAT Network.
2. On each server (NV-DC1, NV-DC2, NV-FS1):
     - Run ipconfig to note DHCP IP.
     - Reconfigure that same address as a static IP.
     - Subnet: 255.255.255.0
     - Gateway: DHCP default gateway
     - DNS: Preferred = Gateway, Alternate = NV-DC1’s IP
3. On NV-CL1:
     - Either leave DHCP enabled or assign a static IP in the same range.
     - Ensure NV-DC1’s IP is set as Alternate DNS.
4. Test connectivity:
- From NV-DC1 → NV-DC2 (ping <NV-DC2_IP>)
- From NV-DC1 → NV-FS1 (ping <NV-FS1_IP>)
- From NV-DC2 → NV-FS1 (ping <NV-FS1_IP>)
- Ping the NAT gateway (ping <Gateway_IP>)
- Ping an external address (ping www.bing.com)
 
Checkpoints:
- Checkpoint 4.1: Screenshot of ping results between:
      - NV-DC1 and NV-DC2
      - NV-DC1 and NV-FS1
      - NV-DC2 and NV-FS1
      - NV-DC1 → Gateway
- Checkpoint 4.2: Screenshot of successful ping to internet (or browser open to www.bing.comLinks to an external site.)

