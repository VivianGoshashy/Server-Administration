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

Test connectivity and ensure internet access
