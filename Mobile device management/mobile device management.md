# Project Phase: Mobile Device Management

# Background
With Entra Connect fully deployed in the previous phase of the NewVue Health Infrastructure Modernization Project, the on-premises Active Directory environment is now synchronized with the NewVue cloud tenant.
The next step in the modernization journey is to transition NewVue Health’s Windows endpoints into Hybrid Azure AD Join, enabling centralized device management and cloud-based security enforcement through Microsoft Intune.

Hybrid Azure AD Join allows a domain-joined Windows 11 device to remain fully integrated with traditional on-prem identity services while simultaneously registering in Microsoft Entra ID.
Once registered, Intune’s MDM automatic enrollment activates, placing the device under cloud management for configuration profiles, compliance policies, application deployments, and update governance.

In this lab, you will configure Hybrid Azure AD Join, configure automatic MDM enrollment, apply the required Group Policy settings, and validate device registration through both the Windows client and the Intune Admin Center.
This is a core real-world workflow performed by modern enterprises transitioning to cloud-first endpoint management.

# Purpose of This Assignment
This assignment strengthens your understanding of cloud-based endpoint management and equips you with skills essential for modern systems administration roles. By completing this lab, you will:

Learn how hybrid Azure AD join enables seamless cloud management of domain-joined devices.

Configure automatic MDM enrollment so devices flow directly into Intune after synchronization.

Deploy enterprise apps (Microsoft 365) from the cloud to enrolled devices.

Apply compliance and security baselines aligned with organizational policy.

Manage updates, monitor device health, and execute remote actions such as restart or wipe.

Build confidence administering enterprise device fleets in Microsoft Intune.

This is a core competency for modern system administrators working in hybrid or cloud-centric environments.

# Learning Objectives
After completing this lab, you will be able to:

Enable and validate automatic MDM enrollment for hybrid Azure AD joined devices.

Confirm that a domain-joined Windows 11 device successfully enrolls into Intune.

Deploy Microsoft 365 Apps from the Intune Admin Center.

Create and assign compliance policies.

Apply Microsoft Security Baselines.

Configure Windows Update Rings.

Perform remote device actions and verify reporting.
