# NewVUE Health IT Infrastructure Modernization
A comprehensive proposal and design for a modern, secure, and highly available IT infrastructure for NewVUE Health, a fictional healthcare organization. This project involved assessing an outdated peer-to-peer network and designing a scalable solution to meet HIPAA compliance, improve operational efficiency, and support future growth.

The proposal outlines a phased approach, starting with an on-premises deployment at the Spokane headquarters, featuring a virtualized Windows Server 2022 environment. The design emphasizes high availability, centralized management, and readiness for hybrid cloud integration with Microsoft Azure.

# Project Overview

Organization: NewVUE Health (Nonprofit Healthcare)
Challenge: Modernize an outdated, non-compliant peer-to-peer network that failed a HIPAA audit and could not support organizational growth.
Solution: Design a robust, on-premises foundation as the first phase of a larger modernization initiative.

# Key Design Objectives

- Centralized Management: Implement Active Directory Domain Services (AD DS) to replace local workgroup accounts.

- High Availability: Eliminate single points of failure for critical services like Domain Controllers, DNS, DHCP, and file servers.

- Security & Compliance: Establish a framework for HIPAA compliance through Group Policy, centralized auditing, and patch management.

- Operational Efficiency: Automate user provisioning, standardize configurations, and provide reliable file/print services.

- Hybrid Cloud Readiness: Design an identity foundation prepared for seamless future integration with Microsoft Entra ID (Azure AD) and Microsoft 365.

# Proposed Infrastructure Architecture

The core infrastructure is built on a virtualized Hyper-V cluster hosting eight virtual servers deployed in redundant pairs:

- Domain Controllers (x2): Active Directory, DNS, DHCP

- File & Print Servers (x2): File Services, DFS Replication, Print Services

- Application Servers (x2): Electronic Health Record (EHR) system, Patient Portal

- Web Servers (x2): Corporate website, web-based applications

# Technical Highlights

- Server Roles: Justified server role placement and combination based on security and performance best practices.

- High Availability: Implemented via Hyper-V Failover Clustering, DFS-R, and Network Load Balancing (NLB).

- OU Structure: Designed an Organizational Unit (OU) structure to mirror departments (HR, Admin, Medical) for granular Group Policy application.

- Hybrid Identity: Prepared for Entra ID Connect synchronization with tools like ADFix for directory health checks.

# Skills Demonstrated

- Windows Server 2022 Infrastructure Design

- Active Directory Domain Services (AD DS) Planning

- High Availability (HA) and Disaster Recovery Strategies

- Network Services (DHCP, DNS) Configuration

- Group Policy Object (GPO) Planning

- Hybrid Identity Integration Readiness

- Virtualization with Hyper-V

- Technical Documentation and Diagramming

















