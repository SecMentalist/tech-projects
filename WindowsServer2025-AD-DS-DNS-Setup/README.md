# Active Directory Domain Services (AD DS) Deployment

## Overview
This repository documents the complete installation and configuration of **Active Directory Domain Services (AD DS)** on a Windows Server environment. It walks through the **Role-based** and **Feature-based** installation process using the "Add Roles and Features" wizard, including dependency management, server promotion, and post-installation troubleshooting.

## Objective
- Install and configure a new Active Directory Domain Controller.
- Deploy supporting services, including **Active Directory Certificate Services (AD CS)**.
- Install essential administration tools for efficient domain management.
- Domain Name: **RobTech**

---

## Role-Based vs. Feature-Based Installation
This deployment distinguishes between core server functionality and supporting utilities:

| Installation Type | Description | Components Installed |
| :--- | :--- | :--- |
| **Role-Based** | Core services that define the server's primary purpose. | - Active Directory Domain Services (AD DS)<br>- Active Directory Certificate Services (AD CS) |
| **Feature-Based** | Supporting tools and utilities that enhance manageability. | - Group Policy Management<br>- Remote Server Administration Tools (RSAT)<br>- AD DS Snap-Ins & Command-Line Tools<br>- Active Directory Administrative Center |

---

## Step-by-Step Installation Process

### 1. Launch the "Add Roles and Features" Wizard
Open **Server Manager** and initiate the **Add Roles and Features** wizard.

### 2. Add Server Roles
Under the **Server Roles** section, I selected:
- ✅ **Active Directory Certificate Services** (AD CS)

### 3. Add Features
Under the **Features** section, I selected:
- ✅ **Group Policy Management**

### 4. Install Required Dependencies
The wizard automatically detected that Active Directory Domain Services requires additional management tools. The following dependencies were prompted and added:

- ✅ **Remote Server Administration Tools (RSAT)**
- ✅ **Role Administration Tools**
  - ✅ **AD DS and AD LDS Tools**
    - ✅ **Active Directory module for Windows PowerShell**
    - ✅ **AD DS Tools**
      - ✅ **[Tools] Active Directory Administrative Center**
      - ✅ **[Tools] AD DS Snap-Ins and Command-Line Tools**

> 💡 These tools are essential for administering the domain post-installation.

### 5. Confirm and Install
After reviewing the summary, the following components were installed:

- ✅ Active Directory Domain Services
- ✅ Remote Server Administration Tools
- ✅ Role Administration Tools
- ✅ AD DS and AD LDS Tools
- ✅ Active Directory module for Windows PowerShell
- ✅ AD DS Tools
- ✅ Active Directory Administrative Center
- ✅ AD DS Snap-Ins and Command-Line Tools

I also checked the box: ✔ **"Restart the destination server automatically if required"**.

Clicked **Install** and waited for the process to complete.

---

## Promote the Server to a Domain Controller
Once the roles and features were successfully installed:

1. Clicked the notification flag in Server Manager.
2. Selected **"Promote this server to a domain controller"**.
3. Chose **"Add a new forest"** (as this is the first domain controller in the environment).
4. Entered the root domain name: **RobTech**.
5. Configured the Directory Services Restore Mode (DSRM) password and completed the remaining prompts.
6. The server automatically rebooted upon successful promotion.

---

## 🛠️ Troubleshooting & Resolution

### Issue Encountered
During the Domain Controller promotion prerequisite checks, I encountered the following error:

> *"The local Administrator account becomes the domain Administrator account when you create a new domain. The new domain cannot be created because the local Administrator account password does not meet requirements."*

### Root Cause
The built-in local `Administrator` account had a **blank password**. Microsoft blocks the promotion if this password is blank to enforce security best practices.

### Resolution (PowerShell Command)
I opened **Windows PowerShell** as an **Administrator** and executed the following command to set a strong password:

```powershell
net user Administrator YourNewStrongP@ssw0rd!

    ⚠️ Password Requirements: The new password must be at least 7 characters long and include at least three of the following categories: uppercase letters (A-Z), lowercase letters (a-z), numbers (0-9), and special characters (!@#$%^&*).

After setting the password, I returned to the AD DS Configuration Wizard and clicked Re-run prerequisites. The validation passed successfully, allowing the domain promotion to proceed.
Post-Installation Verification

After the final reboot, I verified the installation by:

    Opening Server Manager.

    Clicking the Tools menu in the top-right corner.

    Confirming that all Active Directory administration tools are present and accessible:

        Active Directory Administrative Center

        Active Directory Users and Computers

        Active Directory Sites and Services

        Group Policy Management

        ADSI Edit

All tools were successfully installed and available for use.
Summary of Deployment
Attribute	Details
Operating System	Windows Server (2025 / 2022 / 2019)
Primary Role	Active Directory Domain Services
Additional Role	Active Directory Certificate Services
Management Features	Group Policy Management, RSAT, PowerShell Module
Domain Name	RobTech
Deployment Status	✅ Successfully Installed and Verified
