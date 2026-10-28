# Tutorial 8: Attacks and Vulnerabilities

## Task 1: CIA Protections

Based on our manufacturing company network project scenario (incorporating on-site CAD servers, 3D prototype printers, factory PLCs, and physical surveillance):

* **Asset 1: Confidential CAD Engineering Designs & IP (Local Server)**
  * **Protection:** Confidentiality
  * **Reason:** Proprietary 3D prototype models, engineering schematics, and trade secrets represent the company's primary commercial value. If leaked to a competitor or the public, the business loses its competitive advantage and intellectual property.
* **Asset 2: Factory Floor PLCs and Assembly Conveyor Machinery**
  * **Protection:** Integrity
  * **Reason:** Programmable Logic Controllers (PLCs) govern the physical assembly line operations. If an attacker modifies sensor logic, motor RPMs, or temperature calibration parameters, it could cause physical hardware destruction, ruined batches, or worker injury.
* **Asset 3: IP Security Cameras & Network Video Recorder (NVR)**
  * **Protection:** Availability
  * **Reason:** Constant video surveillance is required for physical plant security, inventory monitoring, and occupational safety compliance. If cameras or the NVR are rendered unreachable or knocked offline, physical intrusions and workplace accidents go unrecorded.
* **Asset 4: Next-Generation Perimeter Firewall (NGFW Gateway)**
  * **Protection:** Availability & Integrity
  * **Reason:** The firewall controls inter-VLAN routing, perimeter packet filtering, and the site-to-site IPsec VPN tunnel to the cloud. Altering its filtering rules compromises network isolation, while knocking it offline disconnects all plant operations from cloud services.

---

## Task 2: Threat Sources and Motivation

* **Threat Source 1: Industrial Competitors / Corporate Espionage**
  * **Motivation:** Gain an illicit market advantage by stealing confidential product blueprints, 3D prototype designs, and bill-of-materials pricing to duplicate designs without research and development expenses.
* **Threat Source 2: Financially Motivated Cybercriminal Syndicates (Ransomware Operators)**
  * **Motivation:** Extort money by deploying ransomware across file servers and factory control computers. Attackers target manufacturing firms knowing that prolonged operational downtime costs thousands of dollars per hour, pressuring management to pay ransoms quickly.
* **Threat Source 3: Disgruntled Former or Current Employees (Malicious Insiders)**
  * **Motivation:** Sabotage or financial gain driven by grievances over termination, compensation, or workplace disputes. Insiders exploit authorized access to delete backups, tamper with machinery logic, or leak customer databases.
* **Threat Source 4: Opportunistic Walk-by Attackers / Visitors on Guest Wi-Fi**
  * **Motivation:** Obtain free unauthorized bandwidth or launch network reconnaissance and lateral pivoting attacks from the guest wireless segment into corporate subnets.

---

## Task 3: Explore Vulnerabilities (NIST NVD)

### 1. Critical Severity CVE: CVE-2024-3400
* **CVE ID:** CVE-2024-3400
* **Description:** An arbitrary file creation flaw in the GlobalProtect feature of Palo Alto Networks PAN-OS software allows an unauthenticated remote attacker to execute arbitrary OS commands with root privileges on the target firewall.
* **Date Published:** April 12, 2024
* **CVSS Version 3.1 Score:** 10.0 (Critical) — Vector: `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H`
* **Impact on CIA:**
  * **Confidentiality:** High (Full administrative access allows complete file and configuration exfiltration)
  * **Integrity:** High (Full access allows modifying firewall rules, binaries, and system configs)
  * **Availability:** High (Attacker can disable firewall services or crash the appliance)
* **CWE:** CWE-77 (Improper Neutralization of Special Elements used in a Command / Command Injection)
* **Company & Product:** Palo Alto Networks; PAN-OS (operating system for Next-Generation Firewalls).
* **Vulnerability Explanation:** The GlobalProtect portal improperly sanitizes user-supplied session IDs, allowing path traversal that creates arbitrary files. When internal cron telemetry scripts process these filenames, the crafted filenames are evaluated as shell commands, running arbitrary commands as the root user.
* **Detection & Mitigation:** Upgrade PAN-OS to patched versions (10.2.9-h1, 11.0.4-h1, 11.1.2-h3, or later). Organizations can enable Threat Prevention IDs 95187 and 95189 on GlobalProtect interfaces to block inbound exploit payloads.

### 2. High Severity CVE: CVE-2024-21626
* **CVE ID:** CVE-2024-21626
* **Description:** An internal file descriptor leak in `runc` allows newly spawned container processes to retain references to host directories, enabling attackers to escape the container environment into the host filesystem.
* **Date Published:** January 31, 2024
* **CVSS Version 3.1 Score:** 8.6 (High) — Vector: `CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:C/C:H/I:H/A:H`
* **Impact on CIA:**
  * **Confidentiality:** High (Host filesystem content accessible to containerized processes)
  * **Integrity:** High (Attacker can overwrite host system binaries and host configuration files)
  * **Availability:** High (Host services can be modified or terminated)
* **CWE:** CWE-403 (Exposure of File Descriptor to Unintended Control Sphere)
* **Company & Product:** Open Containers Initiative (OCI) / Linux Foundation; `runc` (container CLI runtime used in Docker and Kubernetes).
* **Vulnerability Explanation:** While configuring a new container process, `runc` left open file descriptors pointing to host directories (such as `/sys/fs/cgroup`). An attacker configuring a malicious container working directory can leverage these descriptors to break out of the container's isolated namespace and write to the underlying host.
* **Detection & Mitigation:** Update container runtimes to `runc` version 1.1.12 or newer. Enforce SELinux in Enforcing mode on container hosts to block processes from accessing host-level files even if a file descriptor is retained.

### 3. Medium Severity CVE: CVE-2024-43381
* **CVE ID:** CVE-2024-43381
* **Description:** Stored Cross-Site Scripting (XSS) in reNgine versions 2.1.2 and earlier allows remote attackers to execute arbitrary web scripts or HTML by injecting payloads into DNS records evaluated during target reconnaissance.
* **Date Published:** August 16, 2024
* **CVSS Version 3.1 Score:** 5.4 (Medium) — Vector: `CVSS:3.1/AV:N/AC:L/PR:L/UI:R/S:C/C:L/I:L/A:N`
* **Impact on CIA:**
  * **Confidentiality:** Low (Session tokens or dashboard data of administrators can be intercepted)
  * **Integrity:** Low (Web application dashboard output can be cosmetically or functionally altered)
  * **Availability:** None (Does not impair application uptime or backend availability)
* **CWE:** CWE-79 (Improper Neutralization of Input During Web Page Generation / Cross-Site Scripting)
* **Company & Product:** reNgine; open-source automated reconnaissance web application framework used by security teams.
* **Vulnerability Explanation:** During external domain reconnaissance scans, reNgine parses DNS TXT/CNAME records returned from target domains and renders them directly in the administrative dashboard without contextual HTML output encoding, causing malicious script tags to execute in the analyst's browser.
* **Detection & Mitigation:** Upgrade reNgine to release 2.1.3 or later. Implement strict output encoding functions before rendering external scan data, and enforce a strict Content Security Policy (CSP) header.

---

## Task 4: Vulnerability Disclosures (Viewpoint)

Coordinated Vulnerability Disclosure (CVD) provides a structured middle ground between secrecy and reckless zero-day disclosure. Vendors require time to triage reports, reproduce flaws, write patches, conduct regression testing, and distribute updates across their customer base. A disclosure window of **60 to 90 days** (industry standard under Google Project Zero and CERT/CC) represents a reasonable compromise between giving vendors adequate remediation time and protecting consumers from lingering vulnerabilities.

If a vendor fails to acknowledge a reported flaw or stalls indefinitely, the researcher is justified in making the vulnerability public. Without a firm deadline, vendors often deprioritize security remediation in favor of product delivery. Responsible disclosure protects users by giving developers time to build fixes, while public disclosure deadlines preserve accountability and ensure critical security weaknesses are addressed.

---

## Task 5: OWASP Top 10 Purpose and Importance

The **OWASP Top 10** is an authoritative awareness document and benchmark identifying the ten most critical security risks facing web applications. 

* **For Web Application Developers:** It provides an architectural blueprint for secure software engineering. Rather than treating security as an afterthought, developers use the OWASP Top 10 to establish defensive development patterns early in the software development lifecycle (SDLC), such as using parameterized SQL queries to prevent injection attacks and implementing centralized access control checks.
* **For Security Professionals:** It serves as a standardized baseline for vulnerability assessments, source code reviews, penetration testing, and regulatory compliance. It ensures that security auditors and software engineers share a common terminology to prioritize high-risk threats, mitigate attack surfaces, and reduce organizational breach risk.
