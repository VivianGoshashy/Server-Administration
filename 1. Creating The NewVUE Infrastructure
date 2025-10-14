# Lab: Building the NewVUE Health Proof-of-Concept Infrastructure

This project documents the creation of a proof-of-concept virtualized infrastructure for NewVUE Health using VirtualBox. The lab involved building a foundational environment with two domain controllers, a file server, and a client machine to validate the network design and establish a platform for future service implementation.

# Key Skills Demonstrated:

- Virtual Network Creation (NAT Network)

- Virtual Machine (VM) Creation & Configuration

- Windows Server 2022 & Windows 11 Installation

- VM Cloning for Rapid Deployment

- Static IP Configuration & Network Troubleshooting

- Basic Network Connectivity and Firewall Management

## Executive Summary

This lab involved building a proof-of-concept virtual network for NewVUE Health to simulate the core infrastructure outlined in the initial proposal. The environment was constructed using VirtualBox and consists of the following virtual machines (VMs):

- NV-DC1 & NV-DC2: Windows Server 2022 Domain Controllers (for redundancy)

- NV-FS1: Windows Server 2022 File Server

- NV-CL1: Windows 11 Client

All VMs were connected via a custom NAT Network to facilitate both inter-VM communication and internet access. After deployment, initial connectivity tests failed due to Windows Firewall blocking ICMP (ping) requests. This was resolved by enabling the appropriate inbound firewall rules on all servers, after which full bidirectional connectivity was confirmed between all nodes and the internet.

This successful proof-of-concept validates the virtualized design and provides a stable foundation for deploying Active Directory and other core services in subsequent phases.

# Virtual Machine Configuration Summary

| VM Name | Operating System |  RAM | CPU Cores | IP Address | Default Gateway |
| :---    |       :---:      | :--: | :---:     | :---:      | :---:           |
