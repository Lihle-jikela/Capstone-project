# Ubuntu Innovations (Pty) Ltd - Week 3 System Administration and Cybersecurity

## Task 1: Backup and Disaster Recovery Plan

To support business continuity and protect the company's critical data, the following automated and scalable backup strategy has been implemented for the core infrastructure and cloud services.

* **Recovery Time Objective (RTO):** 4 Hours (The maximum acceptable downtime before business operations are critically impacted).
* **Recovery Point Objective (RPO):** 24 Hours (The maximum acceptable amount of data loss).

**Backup Strategy & Workflows:**
* **Daily Incremental Backups:** Automated jobs run daily at 01:00 SAST, capturing only the changes made since the last backup. These are pushed directly to a secure offsite cloud storage bucket (e.g., AWS S3 or Azure Blob) to protect against local disasters.
* **Weekly Full Backups:** A complete system and data backup is performed every Sunday at 02:00 SAST to a secondary, localized storage appliance. This ensures rapid restoration capabilities without relying solely on internet bandwidth.
* **Monthly Archive Backups:** Stored in immutable, air-gapped cloud storage (e.g., AWS S3 Glacier). This guarantees a clean, unalterable baseline in the event of a severe ransomware attack.

---

## Task 2: Cybersecurity Policy

This policy establishes the baseline security requirements necessary to protect the Minimum Viable Product (MVP) infrastructure and corporate data at Ubuntu Innovations.

* **Password Complexity Requirements:** All user accounts must use passphrases with a minimum of 14 characters, combining uppercase, lowercase, numbers, and special symbols. Passwords will not expire on a set schedule unless a compromise is actively suspected.
* **Multi-Factor Authentication (MFA):** Mandatory across all company platforms (Microsoft 365, VPNs, internal applications). Users must utilize an authenticator app (time-based OTP or push notification); SMS-based MFA is deprecated.
* **Device Encryption:** Full Disk Encryption (BitLocker for Windows, FileVault for macOS) is strictly enforced via Mobile Device Management (MDM) on all company-issued laptops and desktops.
* **Antivirus & EDR Standards:** Next-generation Endpoint Detection and Response (EDR) software (e.g., CrowdStrike Falcon or Microsoft Defender for Endpoint) must be active, updated, and monitored on all network endpoints.
* **Patch Management:** Automated weekly patching schedules managed centrally via Microsoft Intune to ensure OS and third-party software vulnerabilities are mitigated promptly.
* **Acceptable Use Policy (AUP):** Company-issued hardware and networks are provisioned strictly for business operations. Bypassing security controls, installing unauthorized software, or accessing malicious/illegal content is grounds for disciplinary action.

---

## Task 3: Risk Assessment Matrix

The following matrix outlines the primary IT risks facing the Cape Town office and the implemented mitigation strategies to reduce their likelihood and impact.

| Risk Event | Likelihood | Impact | Mitigation Strategy |
| :--- | :--- | :--- | :--- |
| **Phishing / Social Engineering** | High | High | Regular Security Awareness Training; implementation of robust email filtering and anti-phishing gateways. |
| **Ransomware Infection** | Medium | High | Implementation of immutable offsite backups; active EDR monitoring; adherence to the principle of least privilege. |
| **Hardware Failure (Server/NAS)** | Medium | Medium | RAID 10 storage redundancy on local NAS; localized weekly full backups; proactive hardware lifecycle management. |
| **Power Outage (Load Shedding)** | Medium | High | Deployment of 1500VA UPS systems for core networking equipment (Router, Switch, NAS) to allow graceful shutdowns and prevent data corruption. |

---

## Task 4: Incident Response Plan

In the event of a security breach or critical system failure, IT Support will execute the following six-phase incident response plan to ensure rapid containment and recovery.

1. **Detection:** Anomalies are identified through automated EDR alerts, firewall intrusion logs, or direct reports from employees regarding suspicious system behavior.
2. **Reporting:** The incident is formally logged in the IT ticketing system. Critical breaches must be escalated immediately to Executive Management.
3. **Containment:** IT Support isolates the affected systems to prevent lateral movement. This involves disconnecting the compromised devices from the corporate network (disabling switch ports or Wi-Fi).
4. **Eradication:** The root cause is identified (e.g., malware payload, compromised account credentials). Malicious files are securely wiped, and compromised passwords are unconditionally reset.
5. **Recovery:** Affected systems are restored using the most recent clean backup. Systems are cautiously reconnected to the production network and heavily monitored for recurring anomalies.
6. **Lessons Learned:** Within 48 hours of resolution, IT Support conducts a post-incident review to document the attack vector, evaluate the response efficiency, and update security policies to prevent future occurrences.

---

## Task 5: Security Awareness Training Outline

To cultivate a security-first culture, all employees (including new hires during onboarding) must complete this practical, 4-module training program.

* **Module 1: Identifying Phishing & Suspicious Communications**
    * Understanding the anatomy of a phishing email (urgent tone, spoofed sender addresses).
    * Best practices for verifying links before clicking.
    * How to properly report suspicious emails to the IT Helpdesk.
* **Module 2: Password Hygiene & Authentication**
    * The danger of password reuse and the benefits of passphrases.
    * Introduction to the company-approved Password Manager.
    * Why Multi-Factor Authentication (MFA) is our strongest defense.
* **Module 3: Physical Security in the New Office**
    * Preventing "tailgating" through secure office entry points.
    * Enforcing the "Clean Desk" policy to protect sensitive physical documents.
    * Mandatory screen locking (`Win + L`) when stepping away from workstations.
* **Module 4: Incident Reporting Workflows**
    * Defining what constitutes a security incident.
    * The exact emergency contact procedures to reach IT Support.
    * What information to provide to accelerate the containment phase.
