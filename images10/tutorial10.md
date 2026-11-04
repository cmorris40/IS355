# Tutorial 10: Cyber Security Controls

## Task 1: Essential Eight Mitigation Strategies

Four mitigation strategies from the Australian Cyber Security Centre (ACSC) Essential Eight framework selected across two distinct objective groups for our manufacturing network:

### Strategy 1: Application Control
* **Group:** Prevent Malware Delivery and Execution
* **How Applied to Project:** Configured on all office workstations (VLAN 20) and factory shop-floor PCs (VLAN 30) using Microsoft AppLocker / Windows Defender Application Control (WDAC). Execution is restricted to an approved whitelist of verified binaries, including SolidWorks CAD software, Siemens TIA Portal, and signed business productivity applications.
* **Why Selected:** Attackers often target manufacturing firms with malicious attachments or weaponized software installers. Application control prevents unauthorized scripts, trojans, and ransomware from executing, even if an employee accidentally downloads them.

### Strategy 2: Patch Applications
* **Group:** Prevent Malware Delivery and Execution
* **How Applied to Project:** Automated patch management policies deployed via centralized endpoint management (e.g., Microsoft Intune/WSUS) targeting client-facing applications such as web browsers, PDF viewers, Java runtime environments, and engineering CAD suites. High-priority and critical security patches are tested and rolled out within 48 hours of release.
* **Why Selected:** Third-party client applications are prime attack vectors for initial access via drive-by downloads or malicious web content. Prompt application patching closes known CVE vulnerabilities before exploit kits can leverage them.

### Strategy 3: Restrict Administrative Privileges
* **Group:** Limit the Extent of Cyber Security Incidents
* **How Applied to Project:** Enforce the principle of least privilege across Active Directory. Daily office staff and factory operators use standard user accounts without local administrator rights. IT staff are issued separate privileged administrator accounts (`admin_user`) strictly reserved for system maintenance, which cannot be used for routine web browsing or checking external emails.
* **Why Selected:** If a workstation on VLAN 20 is compromised, lack of local administrator rights prevents malware from dumping local credential caches, installing kernel-level persistence mechanisms, or easily pivoting into factory control networks (VLAN 30) or local file servers (VLAN 50).

### Strategy 4: Regular Backups
* **Group:** Recover Data and System Availability
* **How Applied to Project:** Implement a 3-2-1 backup topology for our critical CAD engineering files, financial databases, and factory PLC configurations. Daily snapshots are created locally on the on-site backup repository, while deduplicated, AES-256 encrypted backups are transferred over an IPsec VPN tunnel to our USA-based cloud backup storage server.
* **Why Selected:** In the event of a severe operational disruption, hardware failure, or ransomware incident, verified off-site, immutable backups ensure critical business data and manufacturing schematics can be restored without paying extortion ransoms or suffering permanent operational downtime.

### Reasons Why Other Strategies Were Not Prioritized:
* **User Application Hardening & Macro Restrictions:** While important, office productivity macros are not widely used on factory OT consoles or engineering design workstations, making application control and least privilege significantly higher-impact defenses for this specific architecture.
* **Multi-Factor Authentication (MFA):** Essential for external VPN/cloud access, but foundational host protections (patching, application control, and backups) provide immediate, comprehensive mitigation against primary on-site malware delivery and data loss risks.

---

## Task 2: Explore and Select NIST SP 800-53 Controls

Six base controls selected from four distinct NIST SP 800-53 control families tailored to our manufacturing facility:

### 1. Control Family: Access Control (AC)
* **Base Control:** **AC-2 (Account Management)**
* **Relevance & Importance:** Regulates the lifecycle of corporate identity accounts, preventing orphaned credentials from lingering after employee departures or role changes.
* **Application to Project:** Implemented via Active Directory Domain Services on the local server (`54.10.50.10`). All account creations, privilege escalations, and revocations require managerial approval. Automated policies immediately disable accounts after 30 days of inactivity or upon notification of employee termination, mitigating unauthorized insider access.

* **Base Control:** **AC-4 (Information Flow Enforcement)**
* **Relevance & Importance:** Controls data transfers between interconnected network segments based on defined security policies and boundary rules.
* **Application to Project:** Enforced by our perimeter Next-Generation Firewall (NGFW) (`54.10.0.1`) and Core Layer 3 Switch (`54.10.10.2`). Strict Access Control Lists (ACLs) enforce one-way inspection and prevent isolated subnets—such as Guest Wi-Fi (VLAN 70) and Surveillance Cameras (VLAN 40)—from initiating inbound sessions to the local server storage subnet (VLAN 50) or industrial factory floor (VLAN 30).

### 2. Control Family: Identification and Authentication (IA)
* **Base Control:** **IA-2 (Identification and Authentication for Organizational Users)**
* **Relevance & Importance:** Verifies the unique digital identity of any user attempting to access internal network assets and administrative services.
* **Application to Project:** Configured across all network infrastructure management interfaces (VLAN 10) and the remote access IPsec VPN tunnel. Administrators connecting to the NGFW, core switch, or cloud Linux management consoles must supply distinct named user accounts backed by time-based one-time password (TOTP) multi-factor authentication, eliminating shared root or generic admin accounts.

### 3. Control Family: System and Communications Protection (SC)
* **Base Control:** **SC-7 (Boundary Protection)**
* **Relevance & Importance:** Establishes defensible physical and logical perimeters separating internal private subnets from public and untrusted networks.
* **Application to Project:** Implemented at our network boundary using the Fortinet FortiGate NGFW (`54.10.0.1`). The appliance filters all traffic traversing the external fiber ISP link, blocks unapproved inbound ports by default, terminates the encrypted IPsec VPN tunnel to the cloud environment, and inspects outbound traffic for command-and-control indicators.

* **Base Control:** **SC-28 (Protection of Information at Rest)**
* **Relevance & Importance:** Safeguards proprietary data stored on local disks against theft, hardware decommissioning leaks, or physical server theft.
* **Application to Project:** Deployed across our Dell PowerEdge on-site storage server (`54.10.50.10`) housing confidential CAD blueprints and company financial data. Volumes are encrypted using AES-256 BitLocker Drive Encryption with cryptographic keys stored in a hardware Trusted Platform Module (TPM 2.0).

### 4. Control Family: Contingency Planning 
* **Base Control:** **CP-9 (Information System Backup)**
* **Relevance & Importance:** Guarantees that copies of user-level information, system-level software, and configuration states are preserved and testable for disaster recovery.
* **Application to Project:** Automated nightly backup routines archive all PLC control configurations, Active Directory database states, and engineering file revisions. Backups are validated weekly with test integrity restorations to confirm recovery reliability in the event of an outage.
