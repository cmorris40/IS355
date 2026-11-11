# Tutorial 11: Managing IT Services

## Task 1: IT Service Management (ITSM), DevOps, and Infrastructure as Code (IaC)

### 1. IT Service Management (ITIL 4) in Our Manufacturing Project
* **Concept:** IT Service Management (ITSM) focuses on delivering value to customers (both internal employees and external clients) through structured practices rather than managing isolated hardware devices. Under ITIL 4, the Service Value System (SVS) guides how opportunities and business demand are converted into delivered services.
* **Application to Project Scenario:** For our small manufacturing facility, IT operations are divided into distinct ITIL practices:
  * **Information Security Management & Incident Management:** Establishing procedures to triage perimeter firewall alerts, unauthorized access attempts on the industrial OT network (VLAN 30), and network camera outages.
  * **Service Continuity & Availability Management:** Defining recovery time objectives (RTO) and automating nightly off-site backups over the IPsec VPN to ensure manufacturing CAD designs and accounting records remain available in a disaster.
  * **Continual Improvement:** Regularly reviewing network bandwidth utilization on the business fiber link and evaluating firewall rule efficiency as production scales.

---

### 2. DevOps & DevSecOps Lifecycle Integration
* **Concept:** DevOps bridges the traditional gap between software developers (who prioritize rapid feature release) and IT operations teams (who prioritize stability and uptime). DevSecOps integrates automated security checks directly into the continuous integration and continuous delivery (CI/CD) pipeline.
* **Relevance & Benefits:**
  * **Automated CI/CD Testing:** Integrating Git repositories (GitHub/GitLab) with automated test suites ensures code and configuration updates are verified before pushing to production environments.
  * **Security Automation (DevSecOps):** Static Application Security Testing (SAST), software composition analysis, and vulnerability scans are executed automatically during pre-commit and build phases, catching misconfigurations before code reaches production servers.
  * **Zero Downtime Deployment (ZDD):** Updates to the public website or internal inventory systems can be deployed incrementally without interrupting customer access or factory production.

---

### 3. Infrastructure as Code (IaC) vs. Graphical Management
* **Concept:** Infrastructure as Code (IaC) defines and provisions IT resources (virtual machines, virtual networks, subnets, and firewall rules) using declarative code files rather than manual point-and-click GUI administration.
* **GUI vs. CLI / Code Trade-offs:**
  * **GUI (Azure Portal):** Intuitive for initial exploration, single-resource visual inspections, and one-off administrative checks. However, GUI workflows are slow, prone to human configuration error, and difficult to document or replicate across environments.
  * **CLI & IaC (Azure CLI `az` / Terraform):** Enables rapid, fully automated deployments that can be duplicated across testing, staging, and production environments in seconds. IaC templates are stored in version control (Git), providing change tracking, team collaboration, and automated peer review before changes are applied.
* **Project Example:** Instead of manually provisioning our USA-based Linux Web Server and Backup Server through the Azure portal, an IaC template (or Azure CLI script) automatically creates the Resource Group, provisions the `B2ls v2` virtual machines, attaches managed disks, configures Network Security Group (NSG) rules, and establishes network routing with guaranteed consistency.

---

## Task 2: Feedback on Project Report

* **Status:** Pending in-class consultation.
* Have not yet met with Dr. Perera for verbal feedback on the project draft report.
* Notes and action items will be updated here once we complete our group review during class.
