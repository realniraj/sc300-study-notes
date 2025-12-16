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

*   **Core Concepts:**
    *   **Authentication:** The process of verifying identity (User ID + Password + MFA). Once validated, Azure AD issues a token to the application.
    *   **Authorization:** The process of determining what permissions the user has after they are authenticated.
*   **Creating a New User:**
    *   **Navigation:** Go to **Users** > **New user** > **Create new user**.
    *   **Mandatory Fields:**
        *   **User name (UPN):** The login ID (e.g., `user@domain.com`). You can choose the default `onmicrosoft.com` domain or a verified custom domain.
        *   **Name:** The display name (e.g., "Test User Two").
    *   **Optional Settings:**
        *   **Password:** Choose to auto-generate or manually set a password.
        *   **Groups/Roles:** Can be assigned during creation (though often done later).
        *   **Settings:** Block sign in (Enable/Disable account), Usage location (Important for licensing).
        *   **Job Info:** Job title, Department, Manager.
*   **Default Permissions:**
    *   A newly created user has **no access rights** to Azure resources or applications by default. They must be explicitly granted access via roles or groups.
*   **User Management & Lifecycle:**
    *   **Source of Authority:** Users can be **Cloud-only** (created in Azure Portal) or **Synced** (from on-premises AD via Azure AD Connect).
    *   **Administrative Actions:**
        *   **Reset Password:** Allows admins to reset credentials if a user is locked out.
        *   **Revoke Sessions:** Invalidates existing tokens, forcing the user to sign in again (useful for compromised accounts).
        *   **Edit Properties:** Update biographical data, contact info, and job details.
*   **Limits:** There are limits to the number of directory objects (users, groups) a tenant can hold, though these limits are high.

#### 1.2.2 Group Management

*   **Purpose:** Groups allow administrators to manage users collectively rather than individually. They are used for assigning permissions to resources or applications in bulk.
*   **Group Types:**
    *   **Security:** Used for managing access to resources (apps, data). Can contain users, devices, groups, and service principals.
    *   **Microsoft 365:** Used for collaboration (Teams, SharePoint, Outlook). Includes a shared mailbox, calendar, etc.
*   **Creating a Group:**
    *   **Azure AD Roles Assignment:**
        *   When creating a group, you can enable **"Azure AD roles can be assigned to the group"**.
        *   **Important:** Once set to **Yes**, this cannot be changed back to No. This allows the group to hold admin roles (e.g., Helpdesk Administrator), simplifying role management.
*   **Group Roles:**
    *   **Owners:** Users who can manage the group settings and membership. Owners do not have to be members of the group (i.e., they don't necessarily inherit the group's access rights).
    *   **Members:** The users (or devices) that belong to the group and inherit permissions assigned to it.
*   **Membership Types:**
    *   **Assigned:**
        *   Administrators manually select and add specific users to the group.
        *   Membership only changes when an admin manually adds or removes a user.
    *   **Dynamic User:**
        *   Membership is determined by a **Dynamic Query** based on user properties (e.g., `user.department -eq "Sales"` or `user.userPrincipalName -contains "student"`).
        *   **Automation:** When a user's attributes change to match the rule, they are automatically added. If they no longer match, they are removed.

#### 1.2.3 Device Management

*   **Overview:**
    *   Device management enables organizations to secure and manage devices (Windows, macOS, iOS, Android, Linux) accessing corporate resources.
    *   It is a prerequisite for implementing **Conditional Access** policies based on device compliance.
*   **Device Identity Types:**
    *   **Azure AD Registered:**
        *   **Scenario:** Bring Your Own Device (BYOD).
        *   **Description:** Personal devices where the user signs in with a personal account but adds a Work/School account to access apps (e.g., Teams, Outlook).
        *   **Authentication:** User authenticates to the app, not the OS.
    *   **Azure AD Joined:**
        *   **Scenario:** Corporate-owned devices.
        *   **Description:** The device is owned by the organization. The user signs in to the OS using their Entra ID credentials.
        *   **Benefit:** Full control over the device, SSO to cloud and on-premises resources.
*   **Registration Process (Windows Example):**
    1.  Navigate to **Settings** > **Accounts** > **Access work or school**.
    2.  Click **Connect**.
    3.  Enter the corporate email (UPN) and password.
    4.  The device is registered, and policies are applied.
    5.  **Verification:** In Azure Portal > **Devices**, the device appears with the status "Azure AD Registered".
*   **Device Settings:**
    *   Located under **Devices** > **Device settings** in the portal.
    *   **Users may join devices to Azure AD:** Controls who can perform Azure AD Join.
    *   **Require Multi-Factor Authentication (MFA) to join devices:** Enhances security during the join process.
    *   **Maximum number of devices per user:** Limits the number of devices a user can register (e.g., 20 or 50).

#### 1.2.4 Administrative Units

*   **Concept:**
    *   An **Administrative Unit (AU)** is a container used to logically group Azure AD resources (users, groups, devices) for the purpose of delegating administrative permissions.
    *   It allows you to restrict the scope of an administrative role to a specific subset of the organization.
*   **Use Case:**
    *   **Regional Administration:** Useful for organizations with independent divisions or geographic regions (e.g., "Central Region", "Northeast Region").
    *   **Delegation:** Instead of giving a Help Desk Administrator global access to reset passwords for *everyone* in the tenant, you can restrict them to reset passwords only for users in the "Central Region" AU.
*   **Configuration Steps:**
    1.  **Create Administrative Unit:**
        *   Navigate to **Administrative units** > **Add**.
        *   Provide a **Name** (e.g., "Central Unit") and **Description**.
    2.  **Add Members:**
        *   Open the created AU.
        *   Select **Members** > **Add member**.
        *   Select the specific users, groups, or devices that belong to this logical unit.
    3.  **Assign Roles (Scoped Administration):**
        *   Select **Roles and administrators** within the AU blade.
        *   Choose a role (e.g., **Helpdesk Administrator**, **User Administrator**, **Password Administrator**).
        *   Assign a user (e.g., a regional IT staff member) to this role.
        *   **Result:** This admin can now perform their role's actions *only* on the members of this specific Administrative Unit.
*   **Licensing Requirement:**
    *   Assigning roles at the scope of an administrative unit requires an **Azure AD Premium P1 or P2** license for the administrator being assigned the role.

#### 1.2.5 Assign Azure AD Premium Licenses to Users

*   **Overview:**
    *   Azure AD (Entra ID) operates on a tiered licensing model (Free, Premium P1, Premium P2).
    *   Upgrading to Premium tiers unlocks advanced security and governance features.
*   **License Tiers & Features:**
    *   **Azure AD Free:**
        *   Basic identity management.
        *   **MFA:** Limited to Mobile App authenticator. (Global Admins get SMS/Call).
        *   **Object Limit:** 500,000 directory objects.
    *   **Premium P2:**
        *   **Advanced MFA:** SMS/Phone call for all users, fraud alerts, MFA reports, custom greetings.
        *   **Identity Governance:** Privileged Identity Management (PIM), Access Reviews.
        *   **Self-Service Password Reset (SSPR):** With on-premises writeback.
        *   **No object limit.**
*   **Managing Licenses:**
    *   **Navigation:** Go to **Billing** > **Licenses** > **All products**.
    *   **View Availability:** Shows total licenses purchased vs. assigned.
*   **Assignment Methods:**
    1.  **Direct Assignment:** Manually assign a license to an individual user profile.
    2.  **Group-Based Licensing:**
        *   Assign a license to a security group (e.g., "Teachers").
        *   **Benefit:** All current and future members of the group automatically inherit the license. Removing a user from the group removes the license.
        *   **Efficiency:** Greatly simplifies license management for large organizations.
*   **Strategic Licensing:**
    *   **Selective Assignment:** You do not need to license every user in the tenant.
    *   **Cost Optimization:** Assign Premium licenses only to users who require specific features (e.g., Administrators needing PIM, or specific departments needing SSPR), while keeping others on the Free plan.

### 1.3 Roles and Administration

This section focuses on the Role-Based Access Control (RBAC) model within Microsoft Entra ID. You will learn how to assign built-in administrative roles to users to delegate tasks effectively while adhering to the principle of least privilege. Additionally, it covers the creation of custom roles to meet specific organizational requirements that built-in roles may not cover.

---
#### 1.3.1 Assign Admin Roles

*   **Azure AD Roles vs. Azure Resource Roles:**
    *   **Azure AD Roles:** Purely administrative roles used to grant access to manage Azure AD resources (users, groups, apps) and other Microsoft services (Office 365, Intune, Dynamics).
    *   **Azure Resource Roles (RBAC):** Used to manage Azure resources like Virtual Machines, Web Apps, and Storage Accounts.
*   **Built-in Roles:**
    *   Azure AD comes with over 75 built-in roles to cover various administrative tasks.
*   **Key Roles:**
    *   **Global Administrator:**
        *   The highest level of access (Company Administrator).
        *   Has permissions to manage all aspects of Azure AD and Microsoft services.
        *   Can read, write, create, delete, and update all resources.
    *   **Granular Roles:** Specific roles designed for limited tasks.
        *   **Guest Inviter:** Can only invite external users (guests).
        *   **Password Administrator:** Can reset passwords for non-admins.
        *   **License Administrator:** Can manage product licenses.
        *   **Intune Administrator:** Manages device configuration and compliance.
*   **Security Best Practice: Principle of Least Privilege:**
    *   Do not assign Global Administrator to everyone in the IT team.
    *   Assign users the specific role(s) required to perform their job functions.
    *   **Benefit:** Reduces the attack surface (if an account is compromised) and prevents accidental changes to critical configurations.
    *   Users can be assigned **multiple roles** if their job requires permissions from different areas (e.g., Directory Writers + Exchange Administrator).

#### 1.3.2 Define Custom Roles

*   **Prerequisites:**
    *   Creating custom roles requires an **Azure AD Premium P1 or P2** license.
    *   If you are on the Free tier, the "New custom role" button will be grayed out.
*   **Why use Custom Roles?**
    *   While there are over 70 built-in roles, they may be too broad for specific security requirements.
    *   **Principle of Least Privilege:** Custom roles allow you to grant *only* the specific permissions needed for a job function, without including dangerous permissions (like Delete).
*   **Strategy for Creation:**
    *   Start by analyzing a built-in role that is similar to what you need (e.g., Group Administrator).
    *   Identify which permissions to keep and which to remove (e.g., Keep "Update", Remove "Create" and "Delete").
*   **Example Scenario: "Group Updater" Role**
    *   **Goal:** Create a role that can update group properties and membership but **cannot create or delete groups**.
    *   **Steps:**
        1.  Navigate to **Roles and administrators**.
        2.  Select **New custom role**.
        3.  **Basics:** Enter a name (e.g., "Group Updater") and description.
        4.  **Permissions:**
            *   Search for the resource (e.g., "groups").
            *   Select specific permissions from the list (e.g., `microsoft.directory/groups/update`, `microsoft.directory/groups/members/update`).
            *   Ensure "Create" and "Delete" permissions are **not** selected.
        5.  **Review + Create:** Finalize the role.
    *   **Result:** Users assigned to this role can manage group day-to-day operations without the risk of accidental deletion or unauthorized creation of new groups.
*   **Cloning:** You cannot clone a built-in role directly to create a custom role; you must start from scratch or clone an existing custom role.

## Module 2: External Identities & Hybrid Identity


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
