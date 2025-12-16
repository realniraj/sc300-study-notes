# SC-300: Microsoft Identity and Access Administrator Study Notes

**Author & Creator:** Niraj Kumar  
**LinkedIn:** [Niraj Kumar](https://www.linkedin.com/in/nirajkum)

This comprehensive guide is designed to assist you in mastering the concepts required for the **Microsoft Certified: Identity and Access Administrator Associate (SC-300)** exam. It covers the implementation of identity management solutions, authentication, access control, and identity governance using Microsoft Entra ID.

## Module 1: Identity Management & Configuration

This module establishes the core foundation of Microsoft Entra ID. It covers the essential tasks of configuring your tenant, managing the lifecycle of users and groups, securing device access, and implementing role-based access control (RBAC) to delegate administrative permissions effectively.

### 1.1 Tenant Setup

This section focuses on the initial configuration and management of your Microsoft Entra ID tenant. You will learn how to create a new tenant, manage its properties, configure custom domains, and customize the user experience with company branding. These foundational steps are critical for establishing a secure and professional identity environment.

---

#### 1.1.1 Introduction to Microsoft Entra ID (formerly Azure Active Directory)

*   **Management Interfaces:**
    *   **Azure Portal (`portal.azure.com`):** The primary visual interface for managing users, groups, applications, and security.
    *   **Automation:** Tasks can also be performed via PowerShell, Azure CLI, ARM Templates, or Bicep.
*   **Account Structure:**
    *   **User:** The identity used to log in. Every user must belong to at least one Tenant.
    *   **Tenant:** A dedicated instance of Entra ID representing an organization.
        *   **Default Domain:** Assigned by Microsoft upon creation (e.g., `username.onmicrosoft.com`).
        *   **Limits:** A single user can create up to 200 tenants and belong to up to 500.
        *   **Cost:** Creating a tenant is free.
    *   **Subscription:** The billing entity required to create Azure resources (like VMs). A tenant can exist without a subscription (purely for identity management).

#### 1.1.2 Create a New Entra ID Tenant

*   **Purpose:**
    *   Create a dedicated environment where you are the **Global Administrator**.
    *   Useful for testing, development, or separating organizational data.
*   **Navigation:**
    *   Go to **Microsoft Entra ID** in the Azure Portal.
    *   Select **Manage Tenants** (often found in the top menu or overview blade).
    *   Click **Create**.
*   **Configuration Steps:**
    1.  **Tenant Type:** Choose **Microsoft Entra ID** (Standard).
    2.  **Configuration:**
        *   **Organization Name:** The display name for the tenant (e.g., "Contoso Dev").
        *   **Initial Domain Name:** Must be globally unique. Format: `yourname.onmicrosoft.com`.
        *   **Country/Region:** Determines **Data Residency** (where data is physically stored). This **cannot be changed** after creation.
    3.  **Review + Create:** Complete the Captcha verification and wait for provisioning.

#### 1.1.3 Switch Tenants & Tenant Overview

*   **Tenant Overview Blade:**
    *   **Tenant ID:** A unique GUID required for scripting (PowerShell/CLI) and configuration.
    *   **License Information:** Displays current license level (e.g., Azure AD Free, Premium P1/P2).
*   **The Global Administrator Role:**
    *   **Assignment:** Automatically assigned to the user who creates the tenant.
    *   **Scope:** Highest level of access; full control over all identity resources (Users, Groups, Roles).
    *   **Requirement:** Every tenant must have at least one Global Administrator.
*   **Switching Contexts:**
    *   **Why Switch?** To manage different environments (e.g., Corporate vs. Dev/Test) where your permissions differ.
    *   **Methods:**
        1.  **Settings Menu:** Click the **Gear icon** > **Directories + subscriptions** > **Switch** next to the desired tenant.
        2.  **Profile Menu:** Click your **Avatar** (top right) > **Switch directory** > Select from Favorites or All Directories.
#### 1.1.4 Set a Custom Domain

*   **Why use a Custom Domain?**
    *   **Professionalism:** Allows users to sign in with their corporate email (e.g., `user@example.com`) instead of the default `user@tenant.onmicrosoft.com`.
    *   **User Experience:** Simplifies login credentials to match existing email addresses.
*   **Configuration Steps:**
    1.  **Add Domain:**
        *   Navigate to **Custom domain names** in the Entra ID blade.
        *   Click **Add custom domain** and enter your domain name (e.g., `getcloudskills.com`).
    2.  **Verify Ownership:**
        *   Entra ID provides a **TXT** or **MX** record.
        *   Add this record to your **Domain Registrar's DNS settings** (e.g., GoDaddy, Namecheap).
        *   **Propagation:** DNS changes can take time (minutes to hours).
        *   Click **Verify** in the Azure Portal.
    3.  **Make Primary (Optional):**
        *   Select the verified domain and click **Make primary**.
        *   This sets it as the default suffix for new users.
*   **Note:** You must have access to the DNS records of the domain to complete verification.

#### 1.1.5 Manage Azure AD Company Branding & Tenant Properties

*   **Company Branding:**
    *   **Purpose:** Enhances user trust by replacing default Microsoft styling with organization-specific visuals on sign-in pages and the My Account portal.
    *   **Key Settings:**
        *   **Visuals:** Background image, Banner logo, Custom color.
        *   **Text:** Sign-in page text, Username hint.
        *   **Localization:** Ability to configure branding based on user locale.
*   **User Settings:**
    *   **App Registrations:** Toggle to allow or block users from registering non-admin applications.
    *   **Portal Access:** "Restrict access to Azure AD administration portal" prevents non-admins from accessing the Entra ID administrative interface.
*   **Tenant Properties:**
    *   **Display Name:** Can be modified (e.g., renaming a dev tenant).
    *   **Region/Country:** Immutable once the tenant is created.
    *   **Global Admin Access:** Toggle to grant the current user management access to all Azure subscriptions (Access management for Azure resources).
    *   **Metadata:** Privacy statement and contact information.

### 1.2 Users, Groups, and Devices

This section covers the core identity objects within Microsoft Entra ID. You will learn how to manage the lifecycle of user identities, organize them into groups for efficient access management, and register devices to ensure secure access to organizational resources. Additionally, it touches on administrative units for delegated administration and license assignment.

---

#### 1.2.1 User Management

#### 1.2.2 Group Management

#### 1.2.3 Device Management

#### 1.2.4 Administrative Units

#### 1.2.5 Assign Azure AD Premium Licenses to Users

### 1.3 Roles and Administration

---
#### 1.3.1 Assign Admin Roles
#### 1.3.2 Define Custom Roles

## Module 2: External Identities & Hybrid Identity

---

### 2.1 External Collaboration (B2B & B2C)

---

#### 2.1.1 External Collaboration Settings
#### 2.1.2 Invite External Users
#### 2.1.3 Bulk Invite External Users
#### 2.1.4 Manage External Users
#### 2.1.5 External User Lifecycle Management
#### 2.1.6 B2C Social Media Users

### 2.2 Hybrid Identity
#### 2.2.1 Introduction to Hybrid Identity
#### 2.2.2 Setup Azure AD Connect

## Module 3: Authentication & Access Management

### 3.1 Authentication Methods
#### 3.1.1 Introduction to Azure MFA
#### 3.1.2 MFA Settings
#### 3.1.3 Passwordless Authentication
#### 3.1.4 Password Protection
#### 3.1.5 Self-Service Password Reset (SSPR)

### 3.2 Security & Conditional Access
#### 3.2.1 Azure AD Security Defaults
#### 3.2.2 Enable Tenant Restrictions
#### 3.2.3 Azure AD Conditional Access
#### 3.2.4 Test Conditional Access
#### 3.2.5 AD Identity Protection

### 3.3 Application Management
#### 3.3.1 Introduction to Enterprise Application Integration
#### 3.3.2 Application Controls

## Module 4: Identity Governance

### 4.1 Entitlement Management
#### 4.1.1 Introduction to Entitlement Management and Packages
#### 4.1.2 Create and Manage Access Packages
#### 4.1.3 Create and Require Terms of Use

### 4.2 Access Reviews
#### 4.2.1 Introduction to Access Reviews
#### 4.2.2 Create Access Reviews
#### 4.2.3 Perform an Access Review
#### 4.2.4 Access Review Licensing

### 4.3 Privileged Identity Management (PIM)
#### 4.3.1 Introduction to Privileged Identity Management
#### 4.3.2 Assigning Roles with PIM
#### 4.3.3 Emergency Break Glass Accounts
