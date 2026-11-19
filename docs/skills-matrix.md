# Technical Competency & Engineering Matrix

Deep-dive technical alignment across **Platform Engineering, Site Reliability Engineering (SRE), DevOps, and Cloud Infrastructure Administration**—backed by **10+ years of production implementations** in high-availability hybrid banking and enterprise environments.

---

## 1. Cloud Infrastructure & Hybrid Architecture (Azure, OCI, VMware & Nutanix)

**Technical Scope:** Hybrid topologies bridging VMware vSphere and Nutanix HCI with Microsoft Azure and Oracle Cloud Infrastructure (OCI), Availability Zone resiliency, Azure Arc governance, and FinOps automation.

??? info "View Verified Production Evidence"
    * **Hybrid Cloud & Omnichannel Topologies (Falabella Bank & Kibernum):** Designed, deployed, and maintained hybrid architectures bridging on-premises **VMware vSphere** and **Nutanix HCI** clusters with **Microsoft Azure** and **Oracle Cloud Infrastructure (OCI)** via encrypted site-to-site network tunnels and phased replication strategies to guarantee business continuity during platform migrations.
    * **Azure IaaS, PaaS & Resiliency Engineering:** Architected single-region high-availability networks using **Azure VNets**, subnets, NSGs, **Availability Zone-redundant** compute/storage (**ZRS** Managed Disks & Blob Storage), **Azure Backup**, and **Azure Site Recovery** to meet strict RPO/RTO targets against datacenter-level failures.
    * **Hybrid Governance via Azure Arc:** Onboarded on-premises Linux and Windows Server fleets into **Azure Arc** to enforce centralized configuration compliance, inventory visibility, and governance via **Azure Policy**.
    * **Layer 4/7 Traffic Modernization:** Replaced legacy on-premises **HAProxy** topologies with **Azure Load Balancers (L4)** and **Azure Application Gateways (L7/WAF)** for scalable ingress routing and SSL termination.
    * **FinOps & Compute Optimization:** Reduced Azure infrastructure spend by **>30%** using budget alerts, telemetry-driven resource right-sizing, automated cleanup policies, and scheduled start/stop automation tied to containerized Dev/QA workloads.
    * **Certifications:** *Microsoft Certified: Azure Fundamentals* (2026) | *AWS Certified Cloud Practitioner* (2024).

---

## 2. Infrastructure as Code (IaC) & Immutable Provisioning

**Technical Scope:** Modular Terraform with remote state locking, GitLab CI orchestration across 300–500+ nodes, one-time Packer golden images for on-prem, and native AlmaLinux cloud catalogs.

??? info "View Verified Production Evidence"
    * **Modular Terraform at Scale (Falabella Bank & Kibernum):** Architected reusable **Terraform** modules with remote state locking to provision and manage lifecycle state across **400–600+ multi-tier nodes** in Omnichannel project (Web, App, Database, and Monitoring) across Development, QA, and Production environments to four country (Chile, Colombia, Peru y Mexico).
    * **Automated IaC Pipelines:** Integrated Terraform (`fmt`, `validate`, `plan`, and gated `apply`) into **GitLab CI/CD** pipelines to enforce declarative infrastructure changes and eliminate manual console provisioning across VMware, Nutanix, Azure, and OCI.
    * **Immutable Golden Images vs. Native Cloud Catalogs:** Built one-time hardened golden image templates using **HashiCorp Packer** for on-premises VMware/Nutanix hypervisors, while leveraging native **AlmaLinux**, **OracleLinux** marketplace/platform images in Azure and OCI—cutting provisioning lead times by **99%**.
    * **Zero-Drift Post-Provisioning:** Integrated Terraform outputs directly with **Chef** and **Ansible** inventories to enforce deterministic node roles immediately after compute creation.

---

## 3. Linux/Windows Systems Engineering & Fleet Automation

**Technical Scope:** RHEL/AlmaLinux/Unix & Windows Server tuning, central Chef cookbooks, Ansible playbooks across 2,000+ legacy nodes, Bash/PowerShell automation, SCCM patching, and pre-decommissioning audits.

??? info "View Verified Production Evidence"
    * **Enterprise OS Administration & Performance Tuning (Falabella Bank, Kibernum, ActiveIT, Anida):** Administered mission-critical **RHEL, AlmaLinux, CentOS, Oracle Linux, Unix**, and **Windows Server** fleets—handling kernel tuning, filesystem/LVM storage expansion, CPU/memory bottleneck remediation, and strict SLA compliance.
    * **Declarative Configuration Management (Chef & Ansible):** Co-owned central **Chef** repositories (custom cookbooks/recipes) and authored modular **Ansible** playbooks/roles across **2,000+ legacy servers** to enforce OS security baselines, manage SSH keys and `sudoers` elevation, automate Linux kernel patching, and execute zero-downtime `.ear`/`.war` deployments on WebLogic/OSB.
    * **Advanced Bash & PowerShell Automation:** Engineered production scripts for **SSL/TLS certificate lifecycle rotation**, **Active Directory/GPO** enforcement, automated **Daylight Saving Time (DST)** offset verification, and log parsing (`/var/log`, `journalctl`, Windows Event Viewer)—cutting repetitive operational toil by **80%**.
    * **Centralized Windows Patch Orchestration:** Managed monthly security patch rollouts and vulnerability remediation across Windows Server fleets using **Microsoft Endpoint Configuration Manager (MECM/SCCM)**.
    * **Runbook Automation & Self-Healing via Rundeck:** Built RBAC-controlled **Rundeck** jobs to automate log purging, filesystem (FS) cleanup thresholds, and orchestrated service restarts across Linux/Windows nodes—preventing disk-saturation outages with full execution audit logs.
    * **Pre-Decommissioning Audits & Capacity Governance:** Executed pre-decommissioning dependency audits (inspecting active sockets, systemd services, mounts, and process bindings) to prevent collateral production outages, paired with proactive capacity forecasting.

---

## 4. Kubernetes, Containers & CI/CD GitOps Pipelines

**Technical Scope:** Production AKS + ACR (`AcrPull` RBAC), ArgoCD declarative sync, Helm charts, Docker multi-stage builds, YAML pipelines (GitLab), and Jira audit traceability.

??? info "View Verified Production Evidence"
    * **Production AKS & GitOps Architecture (Falabella Bank):** Provisioned and operated **Azure Kubernetes Service (AKS)** clusters integrated with **Azure Container Registry (ACR)** via **Managed Identities (`AcrPull` RBAC)**; built **GitLab CI/CD** pipelines to compile/push multi-stage **Docker** images and update declarative K8s manifests synchronized via **ArgoCD**—leveraging ArgoCD for real-time deployment monitoring, drift detection, cluster health checks, and automated self-healing.
    * **Container Orchestration & Workload Migration:** Packaged legacy middleware and monolithic services into containerized runtimes across **Kubernetes (K8s/AKS)** and **Docker Swarm**, configuring Horizontal Pod Autoscalers (HPA), resource limits/requests, Ingress controllers, and **Helm** charts.
    * **Multi-Platform CI/CD & Artifact Governance (GitLab, Jenkins, Rundeck):** Authored declarative **YAML** and Classic pipelines with automated build/test validation gates, internal artifact repository management, Technical Lead approvals in lower environments, and CAB release gates in production.
    * **End-to-End Ticket Traceability:** Enforced strict audit traceability linking **Jira** requirement and change tickets to GitLab repositories and release pipelines under regulated financial compliance frameworks.

---

## 5. Edge Traffic, API Gateways, Middleware & Database Operations

**Technical Scope:** Kong API Gateway (edge routing & rate limiting), HAProxy, clustered Oracle WebLogic/OSB domains, OS/CLI-level Oracle & SQL database diagnostics, and Veeam backup validation.

??? info "View Verified Production Evidence"
    * **API Gateway & Load Balancing (Falabella Bank):** Deployed, tuned, and supported **Kong API Gateway** (configuring upstream targets, plugins, rate limiting, and secure edge routing) alongside **HAProxy** and **Azure Load Balancers** for high-throughput omnichannel banking traffic.
    * **Enterprise Middleware Administration (Falabella Bank & Kibernum):** Administered clustered **Oracle WebLogic (WL)** and **Oracle Service Bus (OSB)** domains—troubleshooting JVM heap/thread dumps, connection pools, JMS queues, tunning and clustered node health.
    * **OS-Level Database Operations & Backup Verification (Falabella Bank):** Provided OS-level administration and CLI-level operational diagnostics for **Oracle** and **SQL** database servers (I/O latency, tablespace/disk saturation, listener health) alongside automated backup/restore verification using **Veeam Backup & Replication**.

---

## 6. Observability, Full-Stack Telemetry & SRE Incident Response

**Technical Scope:** Prometheus (`ServiceMonitor`) & Grafana (`p95/p99` latency), Datadog & Instana APM, ELK/Splunk/Azure Monitor log forensics, Nagios/Icinga, SLI/SLO governance, automated backend DR (Rundeck), AI-assisted ticket triage, and 24/7 on-call RCA.

??? info "View Verified Production Evidence"
    * **Kubernetes & Cloud-Native Telemetry:** Deployed **Prometheus (`ServiceMonitor`)** and **Grafana** dashboards on AKS and Linux fleets to track node/pod resource saturation, HTTP request latency (**p95/p99**), and error-rate budgets.
    * **Multi-Tiered Enterprise Monitoring & Log Forensics:** Engineered and maintained specialized observability layers—**Datadog** and **Instana** for distributed APM and business transaction metrics; **Nagios** and **Icinga** for legacy/on-prem host and service checks; and **ELK Stack, Splunk, Azure Monitor, and Log Analytics** for centralized log indexing and root-cause forensics.
    * **L3 Incident Leadership, On-Call & Blameless RCA:** Participated in **24/7 technical on-call rotations**, led L2/L3 critical incident triage across distributed tiers, defined **SLIs/SLOs** to reduce **MTTD/MTTR**, and authored blameless **Root Cause Analysis (RCA)** postmortems—cutting recurring critical incidents by **>50%**.
    * **SRE Toil Reduction & Automated Backend DR (Rundeck):** Engineered automated **Rundeck** recovery workflows for backend service tiers to execute controlled failover and service restoration during outages—eliminating manual operational toil and significantly lowering **MTTR**.
    * **AI-Assisted Incident Resolution & Ticket Intelligence:** Spearheaded early-stage **AI** adoption over historical resolved ticket datasets and RCA knowledge bases to surface recurring failure patterns, recommend validated remediation steps, and accelerate triage velocity across support tiers.

---

## 7. Engineering Execution, ITIL Change Control & Technical Leadership

**Technical Scope:** Operational runbooks, disaster recovery SOPs, architecture diagrams, engineering mentorship as a DevOps culture (10+ engineers), Agile/Scrum execution, and ITIL change governance.

??? info "View Verified Production Evidence"
    **Runbooks, SOPs & Reference Architecture:** Authored standardized operational runbooks, disaster recovery procedures, and system architecture diagrams—cutting onboarding ramp-up times by **50%** across teams of **10+ engineers**.
    * **Regulated Change Governance & CAB Leadership:** Represented platform engineering in **Change Advisory Board (CAB)** reviews and executed production releases within **Agile/Scrum** sprints and **ITIL** workflows (**Jira, AutoTask, HPSM**), enforcing validated rollback plans and zero unapproved drift across compliance audits.
    * **DevOps Culture & Cross-Silo Enablement:** Bridged operational silos across AppDev, InfoSec, Networking, and DBA teams by standardizing self-service CI/CD templates, shared observability dashboards, and automated operational workflows.
    * **Continuous Improvement & Postmortem Execution:** Facilitated technical retrospectives and blameless post-incident reviews, converting root-cause findings into prioritized **Jira** automation backlogs to permanently eliminate recurring manual toil.
