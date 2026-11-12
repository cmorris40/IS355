# Tutorial 12: Careers in Networking & Cybersecurity

## Task 1: NIST NICE Workforce Framework Self-Assessment

Based on the NIST National Initiative for Cybersecurity Education (NICE) Cybersecurity Workforce Framework and the career pathways covered in IS 355, I evaluated my current technical skills against three target entry-level roles:

### 1. Target Role: Cyber Defense Analyst (PR-CDA-001)
* **Role Focus:** Uses data collected from a variety of cyber defense tools (IDS/IPS, firewalls, packet captures) to analyze events that occur within their environments for the purposes of mitigating threats.
* **Current Strengths:**
  * Analyzing Layer 2–7 network packets (ARP, ICMP, TCP, HTTP) using Wireshark and `tcpdump`.
  * Identifying common web and infrastructure vulnerabilities (OWASP Top 10, CVEs, CWEs) and mapping mitigations using NIST SP 800-53 and the Essential Eight.
* **Areas for Growth:**
  * Gaining hands-on experience with enterprise SIEM platforms (e.g., Splunk, Microsoft Sentinel) for automated log correlation and incident alerting.

### 2. Target Role: Network Operations Specialist (OM-NET-001)
* **Role Focus:** Plans, implements, and operates network services and systems, to include hardware and virtual environments.
* **Current Strengths:**
  * Designing segmented enterprise VLAN architectures (`54.10.x.x/24` subnets) and creating logical topology diagrams using diagrams.net.
  * Configuring virtualized Linux environments, static/dynamic IP routing, and access control rules.
* **Areas for Growth:**
  * Hands-on CLI configuration of physical enterprise switches and Next-Generation Firewalls (e.g., Cisco IOS, FortiOS) beyond virtualized environments.

### 3. Target Role: Cloud Security Associate / Systems Developer (DD-WDEV-001 / SP-ARC-002)
* **Role Focus:** Deploys, secures, and manages cloud infrastructure, virtual machines, and network security boundaries.
* **Current Strengths:**
  * Provisioning Linux VMs, Virtual Networks (VNets), and Network Security Groups (NSGs) in Microsoft Azure.
  * Estimating and comparing cloud infrastructure costs across AWS and Azure, and applying Infrastructure as Code (IaC) and Git version control practices.
* **Areas for Growth:**
  * Writing multi-cloud Terraform templates and automating CI/CD security pipelines (DevSecOps).

---

## Task 2: Cyber Security & Networking Tool Kit

Summary of the core technical tools and platforms utilized throughout IS 355:

| Tool / Platform | Category | Purpose & Practical Application in IS 355 |
| :--- | :--- | :--- |
| **Wireshark & `tcpdump`** | Packet Analysis | Captured and inspected live network traffic across Ethernet, IPv4, ARP, ICMP, TCP, and HTTP layers to troubleshoot connectivity and analyze packet encapsulation. |
| **Git & GitHub** | Version Control & Collaboration | Maintained an individual learning journal (`cmorris40/IS355`) and collaborated on our team network design repository (`IS355_GroupProject`) using commits, branches, and Markdown documentation. |
| **Microsoft Azure** | Cloud Infrastructure | Deployed cloud Linux virtual machines, configured Virtual Networks (VNets) and Network Security Groups (NSGs), and modeled enterprise hosting costs using the Azure Pricing Calculator. |
| **VirtualBox & OpenWRT Linux** | Virtualization & OS | Hosted local virtual machines to practice Linux command-line administration, examine `/etc/shadow` password storage, and configure virtual router/firewall interfaces. |
| **diagrams.net (draw.io)** | Network Architecture | Designed standardized packet encapsulation diagrams and multi-VLAN enterprise network topology maps (`.drawio` source files and `.png` exports). |
| **NIST NVD, CSF & SP 800-53** | Governance & Risk Frameworks | Researched real-world CVEs/CWEs, classified organizational assets, and mapped preventive/detective security controls to manufacturing network risks. |
| **PuTTY / OpenSSH** | Remote Administration | Established secure command-line SSH sessions to manage remote Linux hosts and practice key-based authentication. |

---

## Task 3: Finalize GitHub Journal Checklist

* **Repository Structure:** Verified that all weekly tutorial write-ups (`tutorial7.md` through `tutorial12.md`) and supporting image/diagram folders are properly organized in `cmorris40/IS355`.
* **Diagram & Evidence Verification:** Confirmed all embedded `.png` screenshots, `.drawio` source files, and Markdown tables render cleanly without broken links on GitHub.
* **Project Contribution Log:** Documented individual group project contributions, including the `54.x.x.x/24` VLAN addressing scheme, logical network architecture diagram (`network-architecture.drawio` / `.png`), and Microsoft Azure cost estimates (`Azure_Estimate.xlsx`).
* **Submission Check:** Verified repository organization and links for the **GitHub Journal Part 2** submission on Moodle.
