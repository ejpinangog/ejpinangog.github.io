---

### 2. Matriz de Competencias Interactiva (`docs/skills-matrix.md`)

Este archivo transforma tu matriz de habilidades en bloques desplegables (`??? info`) para que reclutadores y líderes técnicos puedan escanear rápido los 9 dominios y expandir la evidencia concreta cuando quieran profundizar[cite: 2].

```markdown
# Competency & Skills Matrix

This matrix maps core enterprise platform and cloud engineering requirements directly to verified production implementations across banking, financial services, and consulting environments.

---

## 1. Cloud Architecture & Hybrid Migrations (Azure & On-Premises)

**Core Capability:** 10+ years of infrastructure architecture and support, including 5+ years designing and migrating mission-critical workloads to Microsoft Azure and hybrid ecosystems[cite: 2].

??? info "View Verified Production Evidence"
    * **Hybrid Cloud Architecture (Falabella Bank):** Designed and supported hybrid cloud topologies integrating on-premises VMware/Nutanix data centers with Microsoft Azure and Oracle Cloud (OCI) via secure network tunnels and Azure Arc[cite: 1, 2].
    * **IaaS & PaaS Implementation:** Deployed high-availability compute, persistent storage, virtual networks (VNets), Application Gateways, and zone/region-redundant Azure Load Balancers (modernizing legacy on-premises HAProxy setups)[cite: 2].
    * **FinOps & Cost Optimization:** Reduced Azure operational expenses by **>30%** through budget alerts, resource right-sizing, and automated shutdown/startup schedules tied to Docker container deployments in Dev/QA[cite: 2].
    * **Cloud Certifications:** Microsoft Certified: Azure Fundamentals (2026) and AWS Certified Cloud Practitioner (2024)[cite: 1, 2].

---

## 2. Infrastructure as Code (IaC) & Immutable Infrastructure

**Core Capability:** Declarative provisioning and golden image pipelines to eliminate configuration drift and accelerate environment delivery[cite: 1, 2].

??? info "View Verified Production Evidence"
    * **Multi-Environment Terraform Orchestration (Falabella Bank & Kibernum):** Built modular Terraform architectures integrated with GitLab CI to automate provisioning of **300+ servers** across Development, QA, and Production for omnichannel platforms[cite: 2].
    * **Immutable Golden Images (Packer):** Automated OS image baking for Windows Server and Linux, reducing overall infrastructure provisioning lead times by **99%**[cite: 2].
    * **State & Drift Enforcement:** Combined Terraform with Chef and Ansible to enforce deterministic host roles and eliminate manual server setup[cite: 1, 2].

---

## 3. Configuration Management & Fleet Automation

**Core Capability:** Large-scale server lifecycle automation across heterogeneous Linux and Windows fleets[cite: 1, 2].

??? info "View Verified Production Evidence"
    * **Ansible & Chef at Scale (Falabella Bank, Kibernum, Anida):** Authored playbooks, roles, and Chef cookbooks to standardize base OS hardening, deploy security policies, manage SSH keys/sudoers, and orchestrate scheduled middleware deployments (`.ear`/`.war` on WebLogic/OSB)[cite: 2].
    * **Advanced Scripting (PowerShell & Bash):** Engineered automation scripts for Active Directory/GPO governance, SSL/TLS certificate rotation, centralized log diagnostics (`/var/log`, `journalctl`, Event Viewer), and automated Daylight Saving Time (DST) patch verification—cutting manual operational tasks by **80%**[cite: 2].
    * **Centralized Windows Patching:** Orchestrated monthly security compliance across Windows Server fleets using Microsoft Endpoint Configuration Manager (SCCM/MECM)[cite: 2].

---

## 4. CI/CD Pipelines, Containers & GitOps Traceability

**Core Capability:** End-to-end automated software delivery from ticket creation to production container orchestration[cite: 1, 2].

??? info "View Verified Production Evidence"
    * **Ticket-to-Production Traceability (Falabella Bank & Kibernum):** Integrated Jira webhooks with GitLab CI/CD, Azure DevOps (YAML & Classic), and Jenkins to enforce automated build/test gates, approval workflows, and full audit compliance[cite: 1, 2].
    * **Containerization & Kubernetes:** Packaged legacy runtimes into multi-stage Docker images and migrated workloads to Kubernetes (K8s/AKS) and Docker Swarm with horizontal auto-scaling, Helm charts, and ArgoCD state synchronization[cite: 1, 2].

---

## 5. Enterprise Middleware, API Gateways & Complex Environments

**Core Capability:** Administration and troubleshooting of high-transaction, multi-tier distributed ecosystems[cite: 1, 2].

??? info "View Verified Production Evidence"
    * **API Gateways & Load Balancing:** Configured and supported Kong API Gateway, HAProxy, and Azure Load Balancer to ensure low-latency, secure traffic distribution across omnichannel banking services[cite: 2].
    * **Middleware & Databases:** Administered clustered Oracle Service Bus (OSB) and Oracle WebLogic (WL) domains alongside OS-level database operations and automated backup verification (Veeam Backup & Replication)[cite: 1, 2].

---

## 6. Observability, Incident Management & SRE Practices

**Core Capability:** Proactive monitoring, SLI/SLO definition, and L3/L4 incident leadership[cite: 1].

??? info "View Verified Production Evidence"
    * **Full-Stack Telemetry:** Deployed and tuned monitoring and log analytics stacks using Datadog, Prometheus, Grafana, ELK Stack, Splunk, Instana, Azure Monitor, and Nagios/Icinga[cite: 1, 2].
    * **Incident Lifecycle & RCA:** Led critical production triage and authored blameless Root Cause Analysis (RCA) postmortems, reducing recurring critical incidents by **>50%**[cite: 2].
    * **Reliability Engineering:** Defined SLIs, SLOs, and SLAs to guide capacity planning and minimize Mean Time to Detection (MTTD) and Mean Time to Resolution (MTTR)[cite: 1].

---

## 7. Technical Leadership, Agile & ITIL Governance

**Core Capability:** Cross-functional collaboration, engineering mentorship, and audited change management[cite: 1, 2].

??? info "View Verified Production Evidence"
    * **Mentorship & Onboarding:** Provided technical leadership and knowledge transfer to teams of **10+ engineers**, establishing runbooks, SOPs, and architecture diagrams that cut onboarding ramp-up times by **50%**[cite: 2].