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
flowchart TB
    subgraph Governance ["1. Traceability & Governance"]
        Jira[Jira Ticket / ITIL Change] -->|Webhook Trigger| CI[GitLab CI / Azure DevOps]
    end

    subgraph Build ["2. Immutable Build & IaC"]
        CI -->|Validate & Lint| TF[Terraform Modules]
        CI -->|Bake Golden Image| PK[Packer: RHEL / Windows]
    end

    subgraph Deploy ["3. Hybrid Cloud & Orchestration"]
        TF & PK -->|PR Approval Gate| Azure[Microsoft Azure: AKS, VNets, LB]
        TF & PK -->|GitOps Sync / ArgoCD| OnPrem[On-Premises: VMware / Nutanix]
        Ansible[Ansible & Chef] -.->|Config & Hardening| Azure & OnPrem
    end

    subgraph Observe ["4. Observability & Resilience"]
        Azure & OnPrem -->|Metrics & Traces| Obs[Datadog / Prometheus / Grafana]
        Azure & OnPrem -->|Centralized Logs| Logs[ELK Stack / Splunk]
        Obs & Logs -->|Automated Alerting| RCA[SRE Triage & Blameless RCA]
    end
```

### 1. Infrastructure Provisioning (App, Web, Database, Monitoring)
```mermaid
graph TD
    A[Terraform & Packer Code Definition] --> B[Cloud / On-Prem Provisioning]
    B --> C[Application Servers]
    B --> D[Web Servers]
    B --> E[Database Clusters]
    B --> F[Monitoring Agents Setup]
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

### End-to-End Platform Engineering & Delivery Lifecycle
### End-to-End Platform Engineering & Delivery Lifecycle

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

    subgraph P3 ["3. Dev Support & CI/CD (GitLab)"]
        C1[Dev Commit / Merge Request] --> C2[GitLab CI Pipeline & Tests]
        C2 --> C3[Platform Engineer Support & Troubleshooting]
        C3 --> C4{Environment Target?}
    end

    subgraph P4 ["4. Security & Patching (Ansible)"]
        D1[Vulnerability & Patch Compliance] --> D2[Ansible Playbooks Execution]
        D2 --> D3[Hardened & Audited Fleet]
    end

    subgraph P5 ["5. Governance & Release Approvals"]
        C4 -->|Non-Production| E1[TL Review & Manual Approval]
        C4 -->|Production| E2[RFC / Jira Ticket & CAB Review]
        E1 --> E3[Deploy to Staging / Lower Env]
        E2 -->|Approved| E4[Zero-Downtime Production Deployment]
        E2 -->|Rejected| C1
    end

    %% Flujo entre fases
    P1 --> P2
    P2 --> P3
    P3 --> P4
    P4 --> P5
```