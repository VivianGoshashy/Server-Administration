# Project Phase: Provisioning an Azure Tenant & Activating Microsoft 365 E5

# Background
In preparation for Phase 2 of the NewVue Health Infrastructure Modernization Project, the organization must establish a secure cloud identity foundation to support hybrid identity, cloud services, and device management.

In this lab, you will provision a new Azure tenant and activate a Microsoft 365 E5 trial subscription. This process enables key Microsoft cloud technologies including Microsoft Entra ID (formerly Azure AD) for identity management and Microsoft Intune for device and endpoint management.

The tenant you create in this lab will serve as the central cloud environment for all upcoming Week 8 and Week 9 activities, including cloud identity, hybrid integration, Intune configuration, and cloud-based access policies. This lab forms the foundation on which all additional cloud services will be deployed.

# Purpose of the Assignment
This lab provides practical exposure to the mechanics of establishing a cloud identity environment using Microsoft Azure and Microsoft 365.
Students gain hands-on experience setting up a tenant, activating cloud services, configuring sign-in branding, and exploring administrative portals.

By completing this activity, you will learn:

- How organizations establish a cloud tenancy

- How Microsoft 365 licensing is activated and managed

- How cloud admin centers integrate to support identity and device management

- How branding reinforces trust and identity during authentication

- How to validate foundational cloud readiness before onboarding users and devices

This experience mirrors real-world onboarding of cloud identity services in enterprise environments.

# Learning Objectives
By the end of this lab, you will be able to:

- Create an Azure tenant using the free trial

- Activate a Microsoft 365 E5 subscription

- Navigate the Microsoft Entra Admin Center, Microsoft 365 Admin Center, and Intune Admin Center

- Configure Company Branding in Entra ID

- Verify tenant identity properties, subscription status, and administrative capability

# Task and Steps
# Task 1 – Create the Microsoft Azure Free Trial Tenant
### Step 1 – Begin Registration

1. Navigate to:
https://azure.microsoft.com/free Links to an external site.

2. Sign in using a personal Microsoft account (Outlook, Hotmail, Gmail, etc.).

3. Complete the phone number verification step.

4. Enter credit card verification details
(No charges will apply unless you manually upgrade the account.)

### Important Note About Tenant Domains

Azure automatically generates your tenant’s default domain based on the email address used to create the account.

### Example:
If you register with `louis@gmail.com`, your tenant domain becomes:
### louisgmail.onmicrosoft.com

This is expected.
Continue using your automatically generated tenant domain for all activities.

### Step 2 – Confirm Access to the Azure Portal

Once registration is complete, you are redirected to:
https://portal.azure.com Links to an external site.

This confirms that your Azure environment is active and accessible.

### Step 3 – View Directory and Subscription Information

In the Azure Portal:

1. Locate and click the Directory + Subscription filter at the top-right corner.

2. A panel will open displaying:

- Directory Name

- Default Domain

- Tenant ID

- Subscription Name

Record this information for your submission report.

### Step 4 – Open the Microsoft Entra Admin Center

Navigate to:
https://entra.microsoft.com Links to an external site.

Select:

### Identity → Overview

Review the following details:

- Tenant Name

- Primary Domain

- Tenant ID

- Available licensing information

### Task 1 — Checkpoints

Checkpoint 1: Screenshot of the Azure Portal home page showing successful portal access.
Checkpoint 2: Screenshot of the Directory + Subscription panel showing directory and subscription information.
Checkpoint 3: Screenshot of Entra Admin Center → Identity → Overview displaying tenant details.

# Task 2 – Activate the Microsoft 365 E5 Trial
### Step 1 – Access the Microsoft 365 Admin Center

Go to:
https://admin.microsoft.com Links to an external site.

Navigate to:

### Billing → Purchase Services

From All Products, locate Microsoft 365 E5.

### Step 2 – Activate the Trial

1. Select Details → Try E5 for 1 month.

2. Complete the activation form.

3. Wait for confirmation that the subscription is active.

### Step 3 – Verify the Subscription

Navigate to:

### Billing → Your Products

Verify that:

- Microsoft 365 E5 Trial appears in the product list

- The status is Active

- The license count is visible

### Task 2 — Checkpoints

Checkpoint 4: Screenshot showing confirmation of the Microsoft 365 E5 trial activation.
Checkpoint 5: Screenshot of Billing → Your Products showing the E5 trial listed as Active.

# Task 3 – Explore Cloud Administrative Portals
### Step 1 – Admin Centers Overview

In the Microsoft 365 Admin Center:

Select:

### Show All → All Admin Centers

### Step 2 – Access Entra Admin Center

Open Microsoft Entra Admin Center.

Navigate through:

- Users

- Groups

- Devices

- Identity Overview

Confirm you can access directory objects and identity configuration.

### Step 3 – Access Intune Admin Center

Return to All Admin Centers → Intune.

On the Overview page, verify:

- Tenant Name

- MDM and MAM status

- Enrollment settings (default values)

- Intune license enabled via the E5 trial

### Task 3 — Checkpoints

Checkpoint 6: Screenshot of the Microsoft 365 Admin Center dashboard.
Checkpoint 7: Screenshot of Intune Admin Center → Overview.

# Task 4 – Configure Custom (Company) Branding
NewVue Health requires a consistent authentication experience that reflects its organizational identity, reinforces trust, and communicates security expectations.
You will configure the Default Sign-In Experience using the updated Entra ID branding interface.

### Step 1 – Access Branding

1. Go to: https://entra.microsoft.com Links to an external site.

2. Select Identity → Customize

3. Under Default sign-in, select Customize

This opens six tabs:

- Basic

- Layout

- Header

- Footer

- Sign-in form

- Review

Step 2 – Configure Branding Across All Tabs

### Basic
Controls core visual identity.

Configure:

- Favicon – small logo appearing in browser tabs

- Background image – professional healthcare-themed or NewVue-branded image

- Page background color – use NewVue blue: #003D79

Click Next.

### Layout
Controls sign-in screen structure.

Configure:

- Visual Template – choose the template positioning the sign-in form on the right

- Menu behavior – default

- Color theme – set accent color to #003D79

- High contrast – off (unless required)

Click Next.

### Header
Configure:

- Header Logo – NewVue Health full logo (PNG recommended)

- Header text (optional) – NewVue Health Secure Access Portal

Click Next.

### Footer
Configure:

- Footer text:
© NewVue Health – Authorized Access Only

- Hyperlinks:
Optional placeholders for Acceptable Use or Privacy

- Footer images:
Optional

Click Next.

### Sign-In Form
Configure:

- Banner logo (light theme) – NewVue Health banner

- Banner logo (dark theme) – white/contrast version

- Square logos – optional but recommended

- Help text:
Welcome to NewVue Health – Authorized Users Only

Click Next.

### Review
Verify all branding elements.
Click Create or Save.

Allow 2–5 minutes for branding to propagate.

### Step 3 – Validate the Branding

1. Open a private/incognito browser window.

2. Navigate to: https://login.microsoftonline.com Links to an external site.

3. Enter your tenant domain.

4. Verify:

- Background image

- Logo placements

- Branding color

- Footer text

- Banner/logo in the sign-in form

### Task 4 — Checkpoints

Checkpoint 8: Screenshot of one branding configuration tab (Basic, Layout, Header, Footer, or Sign-in Form).
Checkpoint 9: Screenshot of the customized sign-in page.












































 
