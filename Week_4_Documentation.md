# Ubuntu Innovations (Pty) Ltd - Week 4 Final Project Implementation & Service Management

## Task 1: IT Service Management (ITSM) & Helpdesk Workflow

To ensure efficient resolution of technical issues and minimize downtime, a formalized IT Helpdesk workflow based on ITIL best practices is established for the Ubuntu Innovations staff.

**Ticketing System:** Jira Service Management (or Zendesk) will be used to track, prioritize, and resolve all IT requests.

**Standard Helpdesk Workflow:**
1. **Ticket Generation:** Users submit issues via an employee self-service portal, email, or a dedicated Slack/Teams IT channel.
2. **Triage & Categorization:** The system automatically categorizes tickets based on keywords (e.g., Network, Hardware, Access) and assigns a priority.
3. **Prioritization Matrix:**
    *   **P1 (Critical):** Network outage, server down, VIP (Exec) completely blocked. (SLA: 15-minute response, 2-hour resolution).
    *   **P2 (High):** Department-wide issue (e.g., shared printer offline, Finance cannot access NAS). (SLA: 1-hour response, 4-hour resolution).
    *   **P3 (Medium):** Single user impacted but has workarounds (e.g., secondary monitor not working). (SLA: 4-hour response, 24-hour resolution).
    *   **P4 (Low):** Software request, general inquiry. (SLA: 24-hour response, 3-day resolution).
4. **Resolution & Documentation:** The IT technician resolves the issue, documents the fix in the internal Knowledge Base (KB) for future reference, and closes the ticket.

---

## Task 2: Cloud Integration & Migration Strategy

While the MVP relies on local infrastructure (NAS, on-premise routing), Ubuntu Innovations must scale rapidly. A hybrid-cloud approach using **Microsoft Azure** (or AWS) will be implemented.

*   **Phase 1 (Current): On-Premise MVP + SaaS**
    *   Local NAS for large file storage (development builds, video assets).
    *   SaaS adoption: Microsoft 365 for email, Teams, and SharePoint (document collaboration).
*   **Phase 2: Identity & Access Management (IAM) Migration**
    *   Migrate local user management to **Azure Active Directory (Entra ID)**. This enables Single Sign-On (SSO) and enforces MFA across all cloud applications, removing the need for a local domain controller.
*   **Phase 3: Infrastructure as a Service (IaaS)**
    *   As the Software Development team (8 users) scales, CI/CD pipelines and testing environments will be migrated to Azure Virtual Machines and Azure Kubernetes Service (AKS), reducing the hardware strain on the local office network.

---

## Task 4: IT Budget & Cost Estimation (MVP)

A high-level estimated budget to procure the necessary hardware, software, and services for the Cape Town office.

### Capital Expenditure (CapEx) - One-Time Purchases
| Item | Quantity | Estimated Unit Cost (ZAR) | Total Cost (ZAR) |
| :--- | :---: | :--- | :--- |
| Standard Desktops (i5, 16GB RAM) | 20 | R 15,000 | R 300,000 |
| Performance Laptops (i7, 16GB RAM) | 5 | R 28,000 | R 140,000 |
| 24-Port Managed Switch | 1 | R 8,500 | R 8,500 |
| Business Firewall Router | 1 | R 12,000 | R 12,000 |
| Wi-Fi 6 Access Points | 2 | R 4,000 | R 8,000 |
| 8TB NAS (w/ RAID drives) | 1 | R 18,000 | R 18,000 |
| Network Laser Printers | 2 | R 15,000 | R 30,000 |
| 1500VA UPS | 2 | R 4,500 | R 9,000 |
| **Total CapEx** | | | **R 525,500** |

### Operational Expenditure (OpEx) - Monthly Recurring
| Service | Estimated Monthly Cost (ZAR) |
| :--- | :--- |
| Fiber Internet (1Gbps Symmetrical) | R 3,500 |
| M365 Business Standard (25 Users) | R 6,250 |
| Cloud Backup Storage (AWS S3) | R 1,500 |
| CrowdStrike EDR Licensing (25 Users) | R 2,800 |
| Helpdesk Ticketing System (Jira) | R 1,200 |
| **Total Monthly OpEx** | **R 15,250** |

---

## Task 4: Vendor Management & Procurement Policy

To ensure the integrity of the IT supply chain and maintain hardware warranties, the following procurement policies are enforced:
*   **Authorized Resellers Only:** All networking and compute hardware must be purchased through Tier-1 authorized South African distributors (e.g., Mustek, Axiz) to avoid counterfeit goods and ensure valid local warranties.
*   **Hardware Lifecycle:** Desktops will follow a 4-year lifecycle; laptops will follow a 3-year lifecycle. Out-of-warranty hardware will be securely wiped and responsibly e-cycled.
*   **Software Vetting:** All new software requests (especially SaaS tools for the Marketing and Dev teams) must undergo a security review by IT to ensure they comply with data privacy regulations (POPIA).

---

## Task 5: Project Implementation Timeline

A phased rollout schedule for the new office infrastructure, ensuring zero downtime for existing operations during the transition.

*   **Week 1: Procurement & Cabling**
    *   Order all CapEx hardware.
    *   Vendor completes Cat6 Ethernet drops and server rack installation at the new Cape Town office.
    *   ISP installs and activates the primary fiber line.
*   **Week 2: Core Infrastructure Deployment**
    *   Install and configure the Router, Switch, VLANs, and Wi-Fi Access points.
    *   Configure the NAS, storage volumes, and RBAC security groups.
    *   Execute external network penetration tests.
*   **Week 3: Endpoint Provisioning**
    *   Image all 25 desktops and laptops using the automated Intune deployment plan.
    *   Install and test network printers.
*   **Week 4: User Migration & Go-Live**
    *   Staff occupy the new office.
    *   IT Support performs desk-side orientation, ensuring users can log in, access the NAS, and print.
    *   Official hand-off to the Helpdesk for ongoing maintenance and Phase 2 cloud planning.
