# Tutorial 9: Cyber Security Management

## Task 1: Select Security Objectives (NIST CSF)

Four security sub-categories selected from the NIST Cybersecurity Framework (CSF) for our manufacturing network architecture:

### 1. Function: Protect (PR) | Category: Identity Management, Authentication and Access Control (PR.AC)
* **Sub-category:** **PR.AC-5** — *Network integrity is protected (e.g., network segmentation, network management).*
* **Why it is important:** Our facility combines traditional office IT with factory floor operational technology (PLCs, HMIs) and surveillance systems. Strict network segmentation prevents lateral movement between untrusted zones (e.g., Guest Wi-Fi) and critical production machinery.
* **Attack / Vulnerability Mitigated:** Mitigates **lateral pivot attacks** and **worm-like malware propagation** (such as ransomware). If an office workstation is infected, VLAN segmentation and firewall ACLs prevent the infection from spreading into the factory subnet (VLAN 30) or local CAD servers (VLAN 50).

### 2. Function: Protect (PR) | Category: Data Security (PR.DS)
* **Sub-category:** **PR.DS-1** — *Data-at-rest is protected.*
* **Why it is important:** Our local on-site servers store proprietary CAD engineering blueprints, financial ledgers, and 3D prototype files representing core intellectual property.
* **Attack / Vulnerability Mitigated:** Mitigates **data theft and extortion from physical disk theft or unauthorized file access**. Implementing full-disk encryption (AES-256 / BitLocker) ensures that stolen storage media cannot be read without cryptographic keys.

### 3. Function: Detect (DE) | Category: Anomalies and Events (DE.AE)
* **Sub-category:** **DE.AE-1** — *A baseline of network operations and expected data flows for users and systems is established and managed.*
* **Why it is important:** Factory floor PLCs and IP cameras exhibit predictable, repetitive network traffic patterns. Establishing normal traffic baselines allows the security system to immediately spot unusual data spikes or abnormal communication paths.
* **Attack / Vulnerability Mitigated:** Mitigates **covert data exfiltration** and **unauthorized remote access**. For instance, if an IP camera begins sending gigabytes of outbound traffic to an external IP, baseline anomaly alerts will flag the compromised device immediately.

### 4. Function: Detect (DE) | Category: Security Continuous Monitoring (DE.CM)
* **Sub-category:** **DE.CM-1** — *The network is monitored to detect potential cybersecurity events.*
* **Why it is important:** Our perimeter Next-Generation Firewall (NGFW) and managed switches handle all inter-VLAN routing and cloud VPN traffic, making centralized traffic inspection essential.
* **Attack / Vulnerability Mitigated:** Mitigates **active network port scanning and brute-force intrusion attempts**. Continuous monitoring using Intrusion Detection/Prevention Systems (IDS/IPS) detects repeated failed SSH/RDP logins or port scans across subnets, triggering automated blocking before a breach occurs.

---

## Task 2: Create Asset Inventory

### 1. Data Assets
| Asset ID | Asset Name | Description | Location | Classification | Key Protections |
| :--- | :--- | :--- | :--- | :--- | :--- |
| DAT-01 | Proprietary CAD Blueprints | 3D prototype files and schematics | On-Site File Server (`54.10.50.10`) | **Restricted / Confidential** | **Confidentiality & Integrity** |
| DAT-02 | Financial & Payroll Records | Employee wages, accounting records | Financial Server (`54.10.50.11`) | **Confidential** | **Confidentiality & Integrity** |
| DAT-03 | PLC Operational Logic | Assembly line control program code | Factory OT Controller (`54.10.30.15`) | **Internal / Sensitive** | **Integrity & Availability** |
| DAT-04 | Surveillance Video Archives | 30-day rolling security footage | NVR Storage (`54.10.40.10`) | **Internal** | **Availability & Integrity** |
| DAT-05 | Public Website Code & Images | Marketing content and product catalog | Cloud Web Server (AWS/Azure) | **Public** | **Availability & Integrity** |

### 2. Hardware Assets
| Asset ID | Device Type | Model / Description | IP / Identifier | Serial Number | Physical Location |
| :--- | :--- | :--- | :--- | :--- | :--- |
| HWD-01 | Next-Gen Firewall (NGFW) | Fortinet FortiGate 100F | `54.10.0.1` / `00:09:0F:54:01:01` | FG100F-849201 | Server Room (Rack 1) |
| HWD-02 | Core Layer 3 Switch | Cisco Catalyst 9300 48P | `54.10.10.2` / `00:1E:BD:54:10:02` | FCW2410L01A | Server Room (Rack 1) |
| HWD-03 | Local Storage Server | Dell PowerEdge R650 | `54.10.50.10` / `00:14:22:54:50:10` | DPKG-9823-SRV | Server Room (Rack 2) |
| HWD-04 | Industrial 3D Prototype Printer| Stratasys F370 3D Printer | `54.10.20.45` / `00:26:55:54:20:45` | STRAT-3D-4412 | Engineering Lab |
| HWD-05 | Factory Assembly PLC | Siemens S7-1500 Controller | `54.10.30.15` / `00:1B:1B:54:30:15` | S7-1500-9923B | Factory Floor Line 1 |
| HWD-06 | Network Video Recorder (NVR) | Hikvision 32-Channel NVR | `54.10.40.10` / `00:18:AE:54:40:10` | NVR-HK-77102 | Security Office |

### 3. Software Assets
| Asset ID | Software Name | Version / Publisher | Installed On | License / Type |
| :--- | :--- | :--- | :--- | :--- |
| SFT-01 | FortiOS Firmware | v7.4.2 (Fortinet) | FortiGate 100F Gateway | Commercial Subscription |
| SFT-02 | Windows Server 2025 | Datacenter Edition (Microsoft) | Primary On-site Server | Enterprise Volume License |
| SFT-03 | SolidWorks CAD Suite | Professional 2026 (Dassault) | Engineering Desktop PCs | Floating Network License |
| SFT-04 | TIA Portal PLC Engineering | v19 (Siemens) | Factory Maintenance Laptops | Dedicated Workstation License |
| SFT-05 | NGINX Web Server | v1.26 LTS (Open Source) | Cloud Linux VM | Free / Open Source |

### 4. Personnel Assets
| Asset ID | Role / Title | Responsibilities | Access Level | Security Clearance |
| :--- | :--- | :--- | :--- | :--- |
| PER-01 | Network / Security Admin | Firewall rules, switch configs, patch management | Full Domain & Gateway Admin | High |
| PER-02 | Lead CAD Engineer | Develops 3D models and manufacturing specs | Read/Write to CAD Share | Medium (Confidential IP) |
| PER-03 | Plant Operations Manager | Oversees factory floor assembly line schedules | Read/Write to PLC Monitoring | Medium (OT Systems) |
| PER-04 | Office Administrative Staff | General invoicing, customer correspondence | Standard Office PC User | Low (Internal Only) |
| PER-05 | Facility Security Guard | Monitors live camera feeds and physical badges | Read-Only to Camera Feeds | Low |

### 5. Facility Assets
| Asset ID | Facility / Room | Function | Physical Controls | Location |
| :--- | :--- | :--- | :--- | :--- |
| FAC-01 | Main Server Room | Houses core network switches, firewall, storage servers | Biometric keypad, CCTV, temperature sensor | Office 1st Floor |
| FAC-02 | Engineering Design Lab | Contains 3D printers, large plotters, CAD workstations | RFID badge reader, CCTV | Office 2nd Floor |
| FAC-03 | Factory Production Floor | Houses assembly lines, conveyor belts, and PLCs | Keycard turnstile, hard-hat area | Building B |
| FAC-04 | Security Gatehouse | Guards visitor entrance and monitors cameras | Keyed entry, panic button | Main Gate |

### 6. Network Assets
| Asset ID | Network Segment / VLAN | Subnet / Gateway | Purpose | Connected Devices |
| :--- | :--- | :--- | :--- | :--- |
| NET-01 | VLAN 10 (Management) | `54.10.10.0/24` (`.1`) | Administration of switches, firewall, and APs | Switches, Firewall, AP controllers |
| NET-02 | VLAN 20 (Office Wired) | `54.10.20.0/24` (`.1`) | Staff computers, office printers, 3D prototype printers | 30–40 Staff PCs, 3D Printers, Plotters |
| NET-03 | VLAN 30 (Factory OT) | `54.10.30.0/24` (`.1`) | Industrial automation and assembly machinery | PLCs, HMIs, assembly conveyor belts |
| NET-04 | VLAN 40 (Surveillance) | `54.10.40.0/24` (`.1`) | Video surveillance traffic isolated from other networks | ~30 IP Cameras, Local NVR Server |
| NET-05 | VLAN 50 (Local Servers) | `54.10.50.0/24` (`.1`) | Internal core services and local IP file storage | AD/DNS/DHCP, CAD Storage, Finance Share |
| NET-06 | VLAN 60 (Staff Wi-Fi) | `54.10.60.0/24` (`.1`) | Encrypted WPA3 corporate wireless access | Staff laptops, factory monitoring tablets |
| NET-07 | VLAN 70 (Guest Wi-Fi) | `54.10.70.0/24` (`.1`) | Internet-only isolated guest access | Visitors, personal smartphones |
| NET-08 | Cloud VPN Tunnel | IPsec VTI (`Virtual`) | Secure link between on-site firewall and cloud server | Cloud Linux Web Server & Off-site Backups |
