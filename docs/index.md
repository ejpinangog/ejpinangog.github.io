# Erick Pinango
**Technical Lead | Site Reliability Engineer (SRE) | Platform & DevOps Engineer**  
📍 Burnaby, BC, Canada • [LinkedIn](https://www.linkedin.com/in/erick-johan-pinango-garcia-9bb49a12a) • [GitHub](https://github.com/ejpinangog) • [ejpinangog@gmail.com](mailto:ejpinangog@gmail.com)

---

## Executive Summary

Technical Lead, SRE, DevOps and Platform Engineer with **over 10 years of experience** engineering fault-tolerant, scalable hybrid-cloud infrastructures within regulated enterprise environments. Expert in immutable infrastructure, automated Kubernetes platforms, and full-stack telemetry. Bridges deep system-level root cause analysis with proactive technical mentorship to eradicate manual toil, streamline CI/CD pipelines, and elevate engineering velocity.

### Key Production Impact

| Metric | Impact | Domain & Tooling |
| :--- | :--- | :--- |
| **99% Faster Provisioning** | Reduced server deployment lead times from days to minutes across 300+ multi-tier nodes | Terraform, Packer, GitLab CI |
| **80% Toil Reduction** | Automated user/SSH lifecycle, SSL/TLS rotation, OS patching, and DST audits across 2,000+ servers | Ansible, Chef, PowerShell, Bash |
| **>30% Cloud Cost Savings** | Implemented FinOps governance, automated non-prod shutdown schedules, and resource right-sizing | Microsoft Azure, Docker |
| **>50% Fewer Repeat Incidents** | Established proactive telemetry, tuned actionable alerting, and led blameless RCA postmortems | Datadog, Prometheus, ELK, Splunk |
| **Sub-Minute Recovery (RTO)** | Architected multi-zone high availability architectures and automated failover pipelines | VMware, Azure (Availability Zones), Nutanix |
| **Zero-Downtime Releases** | Migrated legacy monolithic workloads to containerized architectures with continuous rolling updates | Docker, Kubernetes, ArgoCD |

---

## Hybrid Cloud & GitOps Reference Architecture

```mermaid
flowchart TD
    subgraph BaseImg ["0. Base Image Catalog"]
        PK[Packer Custom Build - One-Time] --> TPL[(VMware / Nutanix Template)]
        MKT[(Azure Marketplace: AlmaLinux)]
    end

    subgraph Governance ["1. Requirements & CI/CD Orchestration"]
        Jira[Jira Ticket: Requirement & Traceability] --> CI[GitLab CI/CD Repositories]
    end

    subgraph Provisioning ["2. Hybrid Infrastructure Provisioning (Terraform)"]
        TPL -.->|Clone Template| TF1[Terraform vSphere / Nutanix]
        MKT -.->|Base OS Image| TF2[Terraform azurerm]
        CI -->|Plan & Apply| TF1
        CI -->|Plan & Apply| TF2
        TF1 --> OnPremNodes[On-Prem: Web, App, DB & Monitoring Servers]
        TF2 --> Net[Azure: VNets, NSGs & Load Balancers]
        TF2 --> AzNodes[Azure: VMs & AKS Clusters]
        TF2 --> Sec[Azure: Key Vault & Managed Identities]
    end

    subgraph Day2 ["3. Configuration & Observability"]
        OnPremNodes & AzNodes --> CM[Ansible / Chef: Hardening & Agents]
        CM --> Obs[Datadog, Prometheus, Grafana & ELK/Splunk]
        Obs -->|Automated Alerting| RCA[SRE Triage & Blameless RCA]
    end
```
---

### 1. Infrastructure Provisioning

```mermaid
flowchart TD
    PK[Packer Golden Image - One-Time Build] -.->|VMware / Nutanix Template| TF[Terraform Infrastructure Definition]
    CLD[Native Cloud Images: AlmaLinux on Azure & OCI] -.->|Marketplace / Platform Image| TF

    TF --> B[Hybrid Cloud & On-Prem Provisioning]

    B --> P1[Compute & Containers]
    B --> P2[Networking & Edge]
    B --> P3[Data, Storage & DR]
    B --> P4[Security, IAM & Observability]

    P1 --> C1[APP, WEB, DB &<br/>Monitoring Servers]
    C1 --> APP[APP: OSB, WebLogic, Docker,<br/>Swarm, K8s & Microservices]
    APP --> WEB[WEB: Kong API Gateway]
    WEB --> DB[DB: SQL & Oracle]
    DB --> MON[Monitoring: Datadog, Prometheus,<br/>Grafana, Nagios,<br/>Instana, Icinga]

    P2 --> N1[VNets / VCNs, Subnets &<br/>Peering]
    P2 --> N2[Load Balancers, WAF &<br/>API Gateways]
    P2 --> N3[Private DNS &<br/>Hybrid VPN / ExpressRoute]

    P3 --> D1[Database Clusters &<br/>Read Replicas]
    P3 --> D2[Managed Disks, Block Volumes,<br/>LUNs & Backups]

    P4 --> S1[NSGs / Security Lists, Key Vault /<br/>Vault & IAM]
    P4 --> S2[Monitoring Servers &<br/>Telemetry Agents]
```

### 2. Server Configuration Management with Chef
```mermaid
graph LR
    A[Provisioned Nodes] --> B[Chef Server / Cookbooks]
    B --> C[Configuration Recipes Execution]
    C --> D[Guaranteed Environment Parity]
```

### 3. Load Balancer (LB) Creation & Routing
```mermaid
graph LR
    A[Network & Traffic Rules Definition] --> B[Load Balancer Provisioning]
    B --> C[Backend Pools Association]
    C --> D[Traffic Routing Enabled]
```

### 4. Dev Team Support & GitLab CI Deployments
```mermaid
graph TD
    A[Dev Commit / Merge Request] --> B[GitLab CI Pipeline Trigger]
    B --> C[Automated Testing Validation]
    C --> D{Environment Target?}
    D -->|Non-Production / Staging| E[TL Review & Manual Approval]
    D -->|Production| F[RFC / Jira Change Ticket & CAB Review]
    E --> G[Deploy to Lower Environment]
    F -->|Approved| H[Controlled Production Deployment]
    F -->|Rejected| I[Re-evaluation & Adjustments]
```

### 5. Patch & Vulnerability Management with Ansible
```mermaid
graph LR
    A[Vulnerability Report / Patch Schedule] --> B[Ansible Playbooks]
    B --> C[Concurrent Fleet Execution]
    C --> D[Compliance Validation & Audit]
```

### 6. Production Deployment with CAB Approval
```mermaid
graph TD
    A[Release Candidate in Staging] --> B[RFC / Jira Change Ticket Creation]
    B --> C[CAB Review & Approval]
    C -->|Approved| D[Controlled Production Deployment]
    C -->|Rejected| E[Adjustments & Re-evaluation]
```

<!-- ### End-to-End Platform Engineering & Delivery Lifecycle

```mermaid
flowchart TD
    subgraph P1 ["1. Infrastructure Provisioning (IaC)"]
        A1[Terraform & Packer Definitions] --> A2[Hybrid Cloud / On-Prem Provisioning]
        A2 --> A3[App & Web Tiers]
        A2 --> A4[Database Clusters]
        A2 --> A5[Monitoring Agents]
    end

    subgraph P2 ["2. Configuration & Networking"]
        B1[Chef Server / Cookbooks] --> B2[Node State & Parity Enforcement]
        B2 --> B3[Load Balancer & Backend Pools Setup]
    end

    subgraph P3 ["3. Jira-Driven CI/CD & Dev Support (GitLab)"]
        C1[Jira Ticket Creation & Dev Commit] --> C2[GitLab CI Pipeline & Automated Tests]
        C2 --> C3[GitLab Approval Workflow & Review]
        C3 --> C4{Environment Target?}
    end

    subgraph P4 ["4. Security & Patching (Ansible)"]
        D1[Vulnerability & Patch Compliance] --> D2[Ansible Playbooks Execution]
        D2 --> D3[Hardened & Audited Fleet]
    end

    subgraph P5 ["5. Governance & Release Execution"]
        C4 -->|Non-Production / Staging| E1[TL Ticket Approval & Manual Deploy Trigger]
        C4 -->|Production| E2[CAB Review & Change Approval]
        E1 --> E3[Deployment to Lower Environment]
        E2 -->|Approved| E4[Zero-Downtime Production Deployment]
        E2 -->|Rejected| C1
    end

    %% Flujo entre fases
    P1 --> P2
    P2 --> P3
    P3 --> P4
    P4 --> P5
``` -->