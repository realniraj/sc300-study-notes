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

This module explores how to securely collaborate with external users (partners, customers) using Azure AD B2B and B2C. It also covers the implementation of hybrid identity solutions, synchronizing on-premises Active Directory with Microsoft Entra ID using Azure AD Connect to provide a seamless single sign-on experience.

### 2.1 External Collaboration (B2B & B2C)

This section details the mechanisms for enabling secure collaboration with external parties. It covers Azure AD Business-to-Business (B2B) for partner collaboration and Azure AD Business-to-Consumer (B2C) for customer-facing identity management, including configuration settings and user lifecycle management.

---

#### 2.1.1 External Collaboration Settings

*   **Overview of External Identities:**
    *   **B2B (Business-to-Business):** Designed for collaborating with partners and vendors. These users are invited as **Guest Users** into the directory. They can access resources like Teams, SharePoint, and enterprise apps.
    *   **B2C (Business-to-Consumer):** Designed for customer-facing applications. Users sign in with social identities (Google, Facebook) or local accounts to access custom applications.
*   **External Collaboration Settings:**
    *   Located under **External Identities** > **External collaboration settings**. These settings control how guest users interact with the tenant.
*   **1. Guest User Access:**
    *   Determines the level of visibility guest users have regarding other users and groups in the directory.
    *   **Same as members:** Guests have full read access to all directory data (not recommended).
    *   **Limited access:** Guests can see their own profile but have limited visibility into other users (cannot see group memberships of others).
    *   **Restricted access:** Guests can only see their own profile and absolutely nothing else in the directory.
*   **2. Guest Invite Settings:**
    *   Controls who is authorized to invite external users.
    *   **Anyone in the organization:** Includes Member users and even Guest users (if enabled).
    *   **Member users and admins:** Standard users can invite guests.
    *   **Only users with specific admin roles:** Restricts invitations to roles like **Guest Inviter** or **Global Administrator**.
    *   **No one:** Disables the ability to invite guests entirely.
*   **3. Collaboration Restrictions (Domain Restrictions):**
    *   Controls which domains are permitted or blocked for invitations.
    *   **Allow invitations to any domain:** (Default) No restrictions.
    *   **Deny invitations to specified domains:** Block specific domains (e.g., competitors or public email providers like `gmail.com`).
    *   **Allow invitations only to specified domains:** Whitelist approach. Invitations can *only* be sent to the listed domains.

#### 2.1.2 Invite External Users

*   **Concept:**
    *   Inviting external users (B2B) allows them to access your organization's resources using their existing credentials (BYOID - Bring Your Own Identity).
    *   They are added as **Guest** users in the directory.
*   **Step-by-Step: Inviting a User:**
    1.  **Navigation:** Go to **Users** > **New user** > **Invite external user**.
    2.  **Basics:**
        *   **Email:** Enter the external email address (e.g., `partner@gmail.com` or `colleague@othercompany.com`).
        *   **Display Name:** Name of the user.
        *   **Personal Message:** (Optional) A custom message included in the email invitation.
    3.  **Review + Invite:** Sends the email.
*   **The Redemption Process (Guest Experience):**
    1.  **Email:** The user receives an email with a link to "Accept invitation".
    2.  **Consent:** Clicking the link triggers a consent prompt, asking permission for the organization to read their profile.
    3.  **Access:** Once accepted, the user is redirected to the My Apps portal (`myapplications.microsoft.com`) or the specific resource URL.
    4.  **Initial State:** By default, the user may not see any applications until access is explicitly granted.
*   **Granting Access to Applications:**
    *   Guest users do not automatically get access to apps.
    *   **Steps to Assign an App:**
        1.  Navigate to **Enterprise applications**.
        2.  Select (or add) an application (e.g., Adobe Creative Cloud, Salesforce, or a custom app).
        3.  Go to **Users and groups** > **Add user/group**.
        4.  Select the Guest User and assign them.
    *   **Result:** The application will now appear in the guest user's My Apps dashboard.

#### 2.1.3 Bulk Invite External Users

*   **Overview:**
    *   Azure AD provides **Bulk Operations** to manage large sets of users efficiently, avoiding manual entry for each individual.
    *   **Available Operations:** Bulk Create, Bulk Invite, Bulk Delete, and Download Users.
*   **Bulk Invite via CSV (Portal):**
    *   **Scenario:** Inviting multiple external partners at once.
    *   **Process:**
        1.  Navigate to **Users** > **Bulk operations** > **Bulk invite**.
        2.  **Download Template:** Azure provides a specific CSV template.
        3.  **Edit CSV:**
            *   Fill in the required columns (e.g., **Email address to invite**).
            *   Optional columns include **Redirection url** and custom messages.
            *   *Important:* Do not modify the file structure or headers.
        4.  **Upload:** Upload the saved CSV file to trigger the bulk invitation process.
*   **Programmatic Invitation (PowerShell):**
    *   **Scenario:** When user data is in a database or requires logic/transformation before inviting.
    *   **Cmdlet:** `New-AzureADMSInvitation` (Azure AD PowerShell module).
    *   **Method:** Write a script to iterate through a list of users and execute the invitation command for each.

#### 2.1.4 Manage External Users

*   **Management Parity:**
    *   Managing external (Guest) users in the Azure Portal offers a similar experience to managing internal (Member) users.
    *   **Common Administrative Tasks:**
        *   **Security:** Revoke sessions (force sign-out), Reset password (if applicable to the account type), and Delete the account.
        *   **Monitoring:** View **Sign-in logs** and **Audit logs** to track user activity and access history.
*   **Access & Authorization:**
    *   **Zero Trust Start:** Guest users start with no access (blank "My Apps" portal) until resources are explicitly assigned.
    *   **Assignments:**
        *   **Groups:** Add guests to groups for easier management.
        *   **Applications:** Assign guests to Enterprise Applications (e.g., Adobe, Salesforce).
        *   **Admin Roles:** Guests *can* be assigned administrative roles (e.g., **Helpdesk Administrator**) to manage resources or support users within your tenant.
*   **On-Premises Limitations:**
    *   Guest users are **Cloud-only** objects in the inviting tenant.
    *   They are **not** synced back to the on-premises Active Directory.
    *   **Result:** They cannot log in to on-premises domain-joined Windows devices or authenticate against local AD domain controllers.
*   **Bulk Operations:**
    *   Guest users can be managed via bulk operations (e.g., Bulk Delete) using CSV templates, just like member users.

#### 2.1.5 B2C Social Media Users

*   **Overview:**
    *   **B2C (Business-to-Consumer):** Focuses on "end users" or customers who register for applications using their existing social identities or email addresses, rather than a partner organization account.
    *   **Goal:** Simplify the sign-up and sign-in process by allowing users to bring their own identity (BYOID).
*   **Identity Providers (IdPs):**
    *   Located under **External Identities** > **All identity providers**.
    *   **Default Providers:**
        *   **Azure Active Directory:** Standard authentication.
        *   **Microsoft Account:** Allows users with Outlook, Hotmail, or Live accounts to sign in.
        *   **Email One-Time Passcode (OTP):**
            *   **Mechanism:** Users without a supported account receive a code via email to sign in (Magic Link).
            *   **Configuration:** Can be enabled, disabled, or scheduled for future enforcement.
    *   **Social Identity Providers:**
        *   **Google / Facebook:**
            *   Allows users to sign in with their Google or Facebook accounts.
            *   **Setup:** Requires creating a developer application in the respective platform (Google/Facebook) to obtain a **Client ID** and **Client Secret**, which are then configured in Azure AD.
    *   **Generic Providers (SAML / WS-Fed):**
        *   **Purpose:** Integrate with any Identity Provider that supports SAML or WS-Federation protocols (e.g., LinkedIn, Twitter, or a custom IdP).
        *   **Federation:** Establishes a trust relationship where Azure AD accepts authentication tokens from the external provider.

### 2.2 Hybrid Identity

This section focuses on integrating on-premises Active Directory environments with Microsoft Entra ID. You will explore the concepts of hybrid identity, including directory synchronization using Azure AD Connect, and the various authentication methods available to provide a seamless single sign-on experience for users across on-premises and cloud resources.

---

#### 2.2.1 Introduction to Hybrid Identity

*   **Concept:**
    *   **Hybrid Identity:** The integration of on-premises identity infrastructure (Windows Server Active Directory) with Microsoft Entra ID (cloud).
    *   **Goal:** Provide a common user identity for authentication and authorization to all resources, regardless of location (on-premises or cloud).
*   **Azure AD Connect:**
    *   **Role:** The bridge between on-premises AD and Azure AD. It is an agent installed on a Windows Server in the local network.
    *   **Function:** Synchronizes identity objects (Users, Groups, Contacts) from on-premises AD to Azure AD.
    *   **Direction:** On-premises AD is the **Source of Authority** (Master). Changes made on-premises are pushed to the cloud during synchronization cycles (default every 30 minutes).
    *   **Source Anchor:** An immutable attribute (e.g., `mS-DS-ConsistencyGuid` or `objectGUID`) used to uniquely identify and link an object between the two directories.
*   **Prerequisites:**
    *   **Routable Domain:** The on-premises User Principal Name (UPN) suffix (e.g., `@contoso.com`) should match a verified custom domain in Azure AD. If it uses a non-routable domain (e.g., `.local`), users will sync as `@tenant.onmicrosoft.com`.
*   **Authentication Methods:**
    1.  **Password Hash Synchronization (PHS):**
        *   **Mechanism:** Synchronizes a hash of the user's on-premises password hash to Azure AD.
        *   **Auth Location:** Authentication happens entirely in the **Cloud**.
        *   **Pros:** Simplest to deploy, high availability (not dependent on on-prem uptime for auth), supports leaked credential detection.
        *   **Default:** This is the default method.
    2.  **Pass-Through Authentication (PTA):**
        *   **Mechanism:** Validates passwords against the on-premises Active Directory via a lightweight agent installed on-premises.
        *   **Auth Location:** Authentication happens **On-Premises**.
        *   **Pros:** Enforces on-premises account policies (e.g., logon hours) immediately.
        *   **Cons:** Dependent on on-premises infrastructure availability.
    3.  **Federation (AD FS):**
        *   **Mechanism:** Redirects authentication to a separate federation server (e.g., AD FS) or third-party provider (e.g., PingFederate).
        *   **Auth Location:** Authentication happens on the **Federation Server**.
        *   **Pros:** Supports advanced scenarios like smart card auth, third-party MFA integration, or complex conditional access policies not supported natively.
        *   **Cons:** High complexity and infrastructure maintenance.
*   **Seamless Single Sign-On (Seamless SSO):**
    *   Automatically signs users in when they are on their corporate devices connected to the corporate network.
    *   Can be enabled with PHS or PTA.
*   **Monitoring & Health:**
    *   **Azure AD Connect Health:** A robust monitoring tool to view the health of the sync engine and federation infrastructure (AD FS).
    *   **Alerts:** Notifies admins of sync errors or infrastructure downtime.
    *   **Licensing:** Basic sync is free, but advanced monitoring (Connect Health) often requires **Azure AD Premium P1**.
*   **Troubleshooting Synchronization:**
    *   **Common Errors:**
        *   **Duplicate Attributes:** `UserPrincipalName` or `ProxyAddresses` must be unique across the entire tenant. If a synced user conflicts with an existing cloud user, a sync error occurs.
        *   **Data Validation:** Ensure on-premises data meets Azure AD complexity and formatting requirements.

#### 2.2.2 Setup Azure AD Connect

*   **Objective:** Install and configure the Azure AD Connect agent on a Windows Server to synchronize on-premises identities to the cloud.
*   **Prerequisites:**
    *   **Server:** A domain-joined Windows Server (standard or datacenter).
    *   **Credentials:**
        *   **Azure AD:** Global Administrator or Hybrid Identity Administrator.
        *   **On-Premises:** Enterprise Administrator (for creating the AD account used by the service).
*   **Installation Steps (Express vs. Custom):**

    1.  **Connect to Azure AD:** Launch the installer and sign in with your Azure AD Global Admin credentials.
    2.  **Connect to Directories:** Enter the credentials for the on-premises Active Directory Forest.
    3.  **Domain and OU Filtering:**
        *   **Domains:** Select the verified domains to sync.
        *   **Filtering:** You can choose to sync specific Organizational Units (OUs) or specific groups.
        *   *Example:* Syncing only the "Instructors" group rather than the entire directory.
    4.  **Optional Features:**
        *   **Password Hash Synchronization:** Selected as the sign-on method.
        *   **Password Writeback:** (Optional) Allows cloud password changes to write back to on-prem AD. (Disabled in this demo).
    5.  **Configure:** The wizard configures the sync engine and starts the initial synchronization.
*   **Post-Installation:**
    *   **Synchronization Cycle:** The scheduler runs every 30 minutes by default to sync changes (new users, password updates) from on-prem to cloud.
    *   **Verification:**
        *   Go to **Azure Portal** > **Users**.
        *   Verify that on-premises users appear with the "Directory synced" status set to **Yes**.
    *   **Monitoring:** Use **Azure AD Connect Health** to monitor the sync status and troubleshoot errors.

## Module 3: Authentication & Access Management

This module covers the critical aspects of securing user identities through robust authentication methods and access controls. You will learn about implementing Multi-Factor Authentication (MFA), passwordless strategies, and Self-Service Password Reset (SSPR). Furthermore, it dives into securing access using Conditional Access policies and managing enterprise applications to ensure only authorized users can access organizational resources.

### 3.1 Authentication Methods

This section explores the various authentication methods available in Microsoft Entra ID to secure user access. It covers the configuration and enforcement of Multi-Factor Authentication (MFA), the implementation of passwordless authentication strategies, and the use of password protection policies. Additionally, it details how to enable Self-Service Password Reset (SSPR) to empower users to manage their own credentials.

#### 3.1.1 Introduction to Azure MFA

*   **Overview:**
    *   **Multi-Factor Authentication (MFA):** A security process that requires more than one method of authentication from independent categories of credentials to verify the user's identity.
    *   **Goal:** Protect user identities. If a password is compromised, the attacker still cannot access the account without the second factor (e.g., the user's phone).
    *   **Scope:** This course focuses on **Azure MFA (Cloud-based)**. The on-premises **MFA Server** is deprecated and not covered.
*   **Authentication Factors:**
    *   **Something you know:** Password.
    *   **Something you have:** Phone (SMS, Call, Mobile App), Hardware token.
    *   **Something you are:** Biometrics (Fingerprint, Face ID).
*   **Common Methods:**
    *   **Microsoft Authenticator App:** Generates time-based codes or push notifications.
    *   **SMS/Text Message:** Sends a code to the registered mobile number.
    *   **Phone Call:** Automated call to verify identity.
    *   **Email:** Often used for guest users or specific scenarios.
*   **Enabling MFA:**
    *   **Per-User MFA (Legacy/Basic):**
        *   Enabled individually for specific users via a dedicated portal link.
        *   **Status:** Disabled -> Enabled -> Enforced (after registration).
        *   **User Experience:** Once enabled, the user is forced to register for MFA methods upon their next sign-in.
    *   **Organizational Strategy:**
        *   Instead of enabling per-user, it is better to have a strategy based on risk, roles (admins), or login frequency.
        *   This is typically handled via **Conditional Access** (covered later) or Security Defaults.
*   **Configuration Location:**
    *   In the Azure Portal, search for **"Multifactor Authentication"** to access service-level settings (fraud alerts, session settings, trusted IPs).

#### 3.1.2 MFA Settings

*   **MFA Server:**
    *   **Status:** Deprecated as of July 1, 2019. New deployments are not supported.
    *   **Focus:** This course focuses on Azure MFA (Cloud).
*   **Service Settings (Cloud MFA):**
    *   Accessed via the "Additional cloud-based MFA settings" link in the portal.
    *   **Trusted IPs:**
        *   Allows users to skip MFA when signing in from a known corporate network location (Public IP CIDR ranges).
        *   *Note:* Private IPs (e.g., 10.x.x.x) cannot be used here.
    *   **Verification Options:**
        *   **Call to phone:** Automated voice call.
        *   **Text message to phone:** SMS with a code.
        *   **Notification through mobile app:** Push notification (Approve/Deny) via Microsoft Authenticator.
        *   **Verification code from mobile app:** Time-based One-Time Password (TOTP) via Microsoft Authenticator.
    *   **Remember Multi-Factor Authentication:**
        *   Allows users to mark a device as "Trusted" to skip MFA for a set number of days (1 to 365).
*   **Azure Portal MFA Settings:**
    *   **Account Lockout:**
        *   Configures temporary account lockout after a sequence of failed MFA attempts (e.g., wrong code entered multiple times).
        *   **Settings:** Number of denials before lockout, lockout duration, and reset counter duration.
    *   **Block/Unblock Users:**
        *   Manual administrative action to block a specific user from using MFA (e.g., if a device is lost or stolen).
    *   **Fraud Alert:**
        *   Allows users to report fraudulent MFA requests (e.g., receiving a push notification when they are not trying to sign in).
        *   **Action:** Can be configured to automatically block the user when fraud is reported.
        *   **Notifications:** Configure email recipients (e.g., Security Team) to receive alerts when fraud is reported.
    *   **OATH Tokens:**
        *   Support for hardware tokens (e.g., YubiKey, RSA key fobs) that generate OATH TOTP codes.
        *   Admins can upload seed files to register these devices for users.
    *   **Phone Call Settings:**
        *   **Caller ID:** Configure a specific phone number to display when Azure calls users for MFA.
        *   **Custom Greetings:** Upload audio files for custom greetings during the MFA call.

#### 3.1.3 Passwordless Authentication

*   **Concept:**
    *   **The Goal:** Remove the password from the login experience entirely.
    *   **Security vs. Convenience:**
        *   **Passwords:** Convenient but Low Security.
        *   **MFA:** High Security but Inconvenient.
        *   **Passwordless:** High Security AND Convenient.
*   **Three Main Passwordless Methods:**
    1.  **Windows Hello for Business:**
        *   **Type:** Device-specific biometric/PIN.
        *   **Mechanism:** Replaces passwords with strong two-factor authentication on Windows 10/11 devices.
        *   **Security:** Uses biometrics (Face, Fingerprint) or PIN tied to the device's TPM (Trusted Platform Module).
    2.  **Microsoft Authenticator App (Phone Sign-In):**
        *   **Type:** Software-based.
        *   **Mechanism:** The user enters their username on the computer. A number is displayed on the screen. The user must open the app on their phone, select the matching number, and approve via biometric/PIN.
        *   **Benefit:** Turns the phone into a secure token.
    3.  **FIDO2 Security Keys:**
        *   **Type:** Hardware device (USB, NFC, Bluetooth).
        *   **Mechanism:** "Fast Identity Online". Users plug in a key (e.g., YubiKey) and touch it (often with a fingerprint) to authenticate.
        *   **Use Case:** High security environments, shared workstations (kiosks, hospitals) where mobile phones are restricted or not practical.
*   **Configuration:**
    *   Navigate to **Security** > **Authentication methods**.
    *   **Enable Methods:** Select the specific method (e.g., Microsoft Authenticator) and toggle "Enable" to **Yes**.
    *   **Targeting:** Assign to **All users** or specific **Groups**.
*   **User Experience (Number Matching):**
    *   To prevent "MFA Fatigue" (users blindly approving requests), Microsoft enforces **Number Matching**. The user must physically see the login screen to know which number to select on their phone.

#### 3.1.4 Password Protection

*   **Overview:**
    *   Azure AD Password Protection detects and blocks known weak passwords and their variants.
    *   It consists of a **Global Banned Password List** (maintained by Microsoft) and a **Custom Banned Password List** (maintained by you).
*   **Smart Lockout:**
    *   **Goal:** Protect against brute-force attacks by locking out potential attackers while ensuring valid users can still access their accounts.
    *   **Configuration:**
        *   **Lockout threshold:** The number of failed attempts allowed before the account is locked (Default: 10).
        *   **Lockout duration:** The length of time the account remains locked in seconds (Default: 60).
    *   **Intelligence:** Azure AD uses "smart" logic to distinguish between a likely attacker and the genuine user, potentially locking out the attacker's IP while leaving the user's access from a trusted location intact.
*   **Custom Banned Password List:**
    *   **Purpose:** Prevent users from using organization-specific terms in their passwords, which are easily guessable (e.g., Company Name, "Password123", local sports teams).
    *   **Normalization:** The algorithm checks for common character substitutions (e.g., "P@ssw0rd" matches "Password").
    *   **Configuration:** Enter a list of strings (one per line) to ban.
*   **On-Premises Integration:**
    *   **Password Protection for Windows Server AD:**
        *   You can extend Azure AD password policies to your on-premises Active Directory Domain Controllers.
        *   **Requirement:** Install the Azure AD Password Protection proxy and agent on on-premises servers.
    *   **Modes:**
        *   **Audit:** Logs when a user sets a weak password but does not block it.
        *   **Enforced:** Actively blocks users from setting passwords that violate the policy.

#### 3.1.5 Self-Service Password Reset (SSPR)

*   **Overview:**
    *   **Self-Service Password Reset (SSPR)** allows users to reset their own passwords without contacting the help desk.
    *   **Default State:** Disabled for standard users ("None"). Enabled by default for Administrators.
*   **Enabling SSPR:**
    *   Navigate to **Password reset** > **Properties**.
    *   **Self Service Password Reset Enabled:**
        *   **None:** Disabled.
        *   **Selected:** Enable for specific groups (Recommended for piloting).
        *   **All:** Enable for everyone in the tenant.
*   **Authentication Methods:**
    *   Define how users prove their identity before resetting a password.
    *   **Number of methods required:** Choose 1 or 2.
    *   **Available Methods:**
        *   Mobile app notification / code.
        *   Email.
        *   Mobile phone (SMS).
        *   Office phone (Call).
        *   **Security Questions:** Can be used for SSPR (unlike MFA).
*   **Registration:**
    *   **Require users to register when signing in:** If set to **Yes**, users are prompted to set up their authentication data (phone/email) the next time they log in.
*   **Notifications:**
    *   **Notify users on password resets:** Sends an email to the user confirming the change (Security Alert).
    *   **Notify all admins when other admins reset their password:** Alerts the admin team of privileged account changes.
*   **Hybrid Identity (Password Writeback):**
    *   **Scenario:** If users are synced from on-premises AD.
    *   **Requirement:** **Password Writeback** must be enabled in Azure AD Connect.
    *   **Function:** When a user resets their password in the cloud, Azure AD writes the new password back to the on-premises Active Directory in real-time. Without this, the on-prem password remains unchanged.
*   **Administrator Policy:**
    *   Admins always have SSPR enabled (cannot be turned off).
    *   They are required to use strong authentication methods (Email, Phone, or App).

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
#### 4.1.3 External User Lifecycle Management

### 4.2 Access Reviews
#### 4.2.1 Introduction to Access Reviews
#### 4.2.2 Create Access Reviews
#### 4.2.3 Perform an Access Review
#### 4.2.4 Access Review Licensing

### 4.3 Privileged Identity Management (PIM)
#### 4.3.1 Introduction to Privileged Identity Management
#### 4.3.2 Assigning Roles with PIM
#### 4.3.3 Emergency Break Glass Accounts
