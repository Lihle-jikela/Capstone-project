# Ubuntu Innovations (Pty) Ltd - Week 2 Systems Administration Documentation

## Deliverable 1: User and Permissions Matrix

To support a secure, production-ready environment, a Role-Based Access Control (RBAC) model has been established. Access is restricted using the principle of least privilege, ensuring employees only have access to resources required to complete their job functions.

| Department | Security Group | Folder Path | Folder Access Permissions | Access Control Type |
| :--- | :--- | :--- | :---: | :--- |
| **Executive Management** | `mgmt_grp` | `/Company/Management` | **RWX** (Full Access) | Group-Based Owner |
| | | `/Company/Public` | **RWX** (Full Access) | Group-Based |
| | | `/Company/Finance` | **R-X** (Read/Execute) | Read-Only Audit |
| | | `/Company/HR` | **R-X** (Read/Execute) | Read-Only Audit |
| | | `/Company/Sales` | **R-X** (Read/Execute) | Read-Only Audit |
| | | `/Company/Development` | **R-X** (Read/Execute) | Read-Only Audit |
| **Finance** | `finance_grp` | `/Company/Finance` | **RWX** (Full Access) | Group-Based Owner |
| | | `/Company/Public` | **RW-** (Read/Write) | Standard Access |
| | | `/Company/Management` | **---** (No Access) | Explicit Deny |
| | | `/Company/HR` | **---** (No Access) | Explicit Deny |
| **Human Resources** | `hr_grp` | `/Company/HR` | **RWX** (Full Access) | Group-Based Owner |
| | | `/Company/Public` | **RW-** (Read/Write) | Standard Access |
| | | `/Company/Finance` | **---** (No Access) | Explicit Deny |
| | | `/Company/Development` | **---** (No Access) | Explicit Deny |
| **Sales and Marketing** | `sales_grp` | `/Company/Sales` | **RWX** (Full Access) | Group-Based Owner |
| | | `/Company/Public` | **RW-** (Read/Write) | Standard Access |
| | | `/Company/Management` | **---** (No Access) | Explicit Deny |
| **Software Development** | `dev_grp` | `/Company/Development` | **RWX** (Full Access) | Group-Based Owner |
| | | `/Company/Public` | **RW-** (Read/Write) | Standard Access |
| | | `/Company/Finance` | **---** (No Access) | Explicit Deny |

---

## Deliverable 2: Shared Folder Structure

### 1. Directory Tree Representation
Below is the directory hierarchy implemented on the central Network-Attached Storage (NAS) device:

```
/Company
├── Public/                  (Shared among all staff)
├── Management/              (Confidential Executive files)
├── Finance/                 (Audits, Payroll, Budgets)
│   ├── Audits/
│   ├── Payroll/             (Inheritance disabled, restricted to Senior Accountant)
│   └── Budgets/
├── HR/                      (Personnel records, Contracts)
│   ├── EmployeeFiles/
│   └── Recruitment/
├── Sales/                   (Marketing collateral, Client leads)
└── Development/             (Source code, Software pipelines)
    ├── SourceCode/
    └── Builds/
```

### 2. Inheritance and Group Policies
* **Permissions Inheritance:** In standard configurations, child directories (e.g., `/Company/Finance/Audits`) automatically inherit permissions from parent directories (`/Company/Finance`). However, for highly sensitive directories such as `/Company/Finance/Payroll`, inheritance is explicitly disabled. Custom access control lists (ACLs) are applied to restrict payroll access specifically to the Finance Director and HR Lead, preventing general Finance department staff from viewing employee compensation.
* **Group Policy Objects (GPOs):** GPOs in Active Directory will drive automation and security. Drive mapping GPOs will dynamically map paths as local drives (e.g., `P:` for `/Company/Public` and `S:` for departmental shares) depending on security group memberships. Security Filtering ensures that users only see the specific drives they have access to, reducing security discovery vectors.

---

## Deliverable 3: Command-Line Administration Guide

This quick-reference guide illustrates how to administer files, permissions, processes, and packages on our Linux-based NAS or server environment.

### 1. `pwd` (Print Working Directory)
* **Description:** Displays the absolute path of the directory you are currently in.
* **Example:**
  ```bash
  pwd
  # Output: /Company/Development
  ```

### 2. `ls` (List)
* **Description:** Lists files and directories within a target path.
* **Example (Long list showing hidden files and detailed permissions):**
  ```bash
  ls -la /Company/Finance
  # Output: drwxrwx--- 3 root finance_grp 4096 May 28 12:00 .
  #         -rwxrwx--- 1 root finance_grp 8024 May 28 12:05 Q1_Report.xlsx
  ```

### 3. `cd` (Change Directory)
* **Description:** Navigates between folders.
* **Example:**
  ```bash
  cd /Company/Public
  ```

### 4. `mkdir` (Make Directory)
* **Description:** Creates one or more new directories.
* **Example (Creating nested folders recursively):**
  ```bash
  mkdir -p /Company/Development/SourceCode/api-service
  ```

### 5. `touch` (Create File / Update Timestamp)
* **Description:** Creates an empty file immediately, or updates the modified timestamp of an existing file.
* **Example:**
  ```bash
  touch /Company/Public/office_relocation_notice.txt
  ```

### 6. `chmod` (Change Mode / Permissions)
* **Description:** Alters read (r), write (w), and execute (x) permissions for owner, group, and others.
* **Example (Owner/Group get full permissions, Others blocked entirely):**
  ```bash
  chmod 770 /Company/Finance
  ```

### 7. `chown` (Change Owner)
* **Description:** Changes the file owner and/or group association.
* **Example (Change owner to root and group to dev_grp recursively):**
  ```bash
  chown -R root:dev_grp /Company/Development
  ```

### 8. `ps` (Process Status)
* **Description:** Snapshot of currently running processes.
* **Example (Show all active system processes with detailed CPU/RAM owners):**
  ```bash
  ps aux | grep smbd
  ```

### 9. `top` (Table of Processes)
* **Description:** Real-time dynamic monitoring of system processes, CPU, and Memory utilization.
* **Example:**
  ```bash
  top
  ```

### 10. `apt install` (Advanced Package Tool)
* **Description:** Installs software packages from standard repositories on Debian/Ubuntu systems.
* **Example (Installing the Samba file sharing service for the NAS):**
  ```bash
  sudo apt update && sudo apt install -y samba
  ```

---

## Deliverable 4: Software Deployment Plan

To construct an efficient Minimum Viable Product (MVP) that remains highly scalable, software deployment and updates will be standardized and automated.

### 1. Standard OS & Software Image (All Users)
All company workstations (Desktops and Laptops) will be provisioned using a base enterprise system image containing:
* **Operating System:** Windows 11 Pro.
* **Productivity:** Microsoft 365 Business Standard (Outlook, Word, Excel, Teams, OneDrive).
* **Security:** CrowdStrike Falcon EDR and Microsoft Defender.
* **Browsers:** Google Chrome and Microsoft Edge (managed with corporate ADMX templates).
* **Utilities:** Adobe Acrobat Reader, Zoom desktop client.

### 2. Department-Specific Software Mapping
Specific packages are deployed automatically to endpoints based on department group memberships via **Microsoft Intune**:
* **Management:** Microsoft Power BI Desktop.
* **Finance:** Xero Desktop Integration client, local banking security certs.
* **HR:** BambooHR desktop portal app, Adobe Acrobat Pro.
* **Sales:** Salesforce CRM Integration client, Adobe Creative Cloud Suite.
* **Development:** Git, Docker Desktop, VS Code, WSL 2 (Ubuntu LTS), Node.js, Python 3.

### 3. Automated Patch Management & Update Procedures
Patch management will utilize Microsoft Intune ring-based deployments:
* **Pilot Ring (IT & Dev volunteers - 3 users):** Patches deployed instantly. Evaluated for 3 days.
* **Production Ring (Remaining users - 22 users):** Patches deployed automatically after the 3-day pilot evaluation.
* **Schedule:** Weekly patch applications scheduled for Tuesday nights at 22:00 (SAST) to minimize productivity disruptions. Reboots enforced at 03:00 if pending.

### 4. Centralized Licensing Requirements
* **M365 Licensing:** Microsoft 365 Admin Center utilizing Azure Active Directory (Entra ID) group-based auto-licensing.
* **Creative Cloud Licensing:** Managed via Adobe Admin Console using Federated IDs synced with Entra ID.
* **Developer Licensing:** Standardized JetBrains/GitHub licenses managed globally through billing groups.

---

## Deliverable 5: IP Addressing Plan

The network architecture is structured around a scalable `/24` CIDR block subnetting scheme. This isolates network traffic, improves security, and controls broadcast domains across the Cape Town office.

| VLAN ID | Subnet Name / Device Type | Subnet Range | Subnet Mask | Gateway | DHCP Range / Allocation | Business Justification |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **VLAN 10** | Core Infrastructure & Servers | 10.10.10.0/24 | 255.255.255.0 | 10.10.10.1 | N/A (Static Assignment) | Hosts Router, Core Managed Switch, WAP Admin Interfaces, and the NAS. |
| **VLAN 20** | Corporate User Devices (Wired) | 10.10.20.0/24 | 255.255.255.0 | 10.10.20.1 | 10.10.20.10 - 10.10.20.200 | For stationary desktops belonging to Management, Finance, HR, and Sales. |
| **VLAN 30** | Software Development (Wired) | 10.10.30.0/24 | 255.255.255.0 | 10.10.30.1 | 10.10.30.10 - 10.10.30.200 | High-performance developer systems. Separated to apply custom firewall rules and protect code. |
| **VLAN 40** | Network Printers | 10.10.40.0/24 | 255.255.255.0 | 10.10.40.1 | N/A (Static IPs Only) | Segregates printers to secure them from print-spooler exploits and minimize multi-cast chatter. |
| **VLAN 50** | Corporate Wi-Fi (Staff Laptops) | 10.10.50.0/24 | 255.255.255.0 | 10.10.50.1 | 10.10.50.10 - 10.10.50.250 | Connects laptops and authorized corporate smartphones using WPA3 Enterprise authentication. |
| **VLAN 60** | Guest Wi-Fi Network | 10.10.60.0/24 | 255.255.255.0 | 10.10.60.1 | 10.10.60.10 - 10.10.60.250 | Provides isolated internet access to clients. Blocked from routing to internal VLANs. |
