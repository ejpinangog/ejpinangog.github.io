# Extended Resume

**Erick Pinango**  
Burnaby, BC V5H 0J9 • +17787983650 • [ejpinangog@gmail.com](mailto:ejpinangog@gmail.com) • [linkedin.com/in/erickpinango9bb49a12a](https://www.linkedin.com/in/erickpinango9bb49a12a)

---

## Summary

Technical Lead, SRE, DevOps, Platform Engineer, Systems and Infrastructure Administrator with over 10 years of hands-on experience building, automating, and scaling enterprise infrastructures across hybrid cloud environments. Highly organized and collaborative, I enjoy bringing structure to complex systems and partnering with teams to drive efficiency. Proven track record of optimizing continuous delivery workflows, managing complex architectures, and leading critical incident responses to ensure high availability in fast-paced, high-demand production environments.

---

## Skills

| Domain | Focus Area |
| :--- | :--- |
| **Operations & Reliability:** | High Availability (HA), Disaster Recovery (DR), SLI/SLO/SLA Governance, Root Cause Analysis (RCA), Blameless Postmortems, Capacity Planning, ITIL v3 Change Management |
| **Automation & Infrastructure as Code:** | Terraform, Packer, Ansible, Chef, Rundeck, SCCM/MECM, Bash, Python, PowerShell |
| **Containers & Orchestration:** | Kubernetes (AKS, K8s), Docker, Docker Swarm, Helm, ArgoCD, Microservices Architecture |
| **Cloud & Hybrid Virtualization:** | Microsoft Azure (IaaS/PaaS, VNets, Load Balancer, Application Gateway, Arc), Oracle Cloud Infrastructure (OCI), VMware vSphere (5.5/6.5), Nutanix HCI |
| **CI/CD & GitOps Traceability:** | GitLab CI/CD, Azure DevOps (YAML & Classic Pipelines), Jenkins, Jira Webhook Integrations |
| **Observability & Monitoring:** |Datadog, Prometheus, Grafana, Splunk, ELK Stack (Elasticsearch, Logstash, Kibana), Instana, Azure Monitor, Nagios, Icinga |
| **Middleware, Networking & Security:** | Kong API Gateway, HAProxy, Oracle Service Bus (OSB), Oracle WebLogic (WL), SSL/TLS Lifecycle, OS Hardening, Active Directory & GPO |

---

## Work Experience

### Technical Lead, 02/2025 to Current
**FALABELLA BANK – Santiago De Chile**

* **Systems Architecture & Hybrid Cloud:**
    * **Objective:** Design and maintain the technological infrastructure, combining on-premise enterprise servers (VMware-Nutanix) with cloud services (Microsoft Azure and Oracle Cloud).
    * **Implementation (How I did it):** Configured hypervisors, established hybrid network tunnels, mapped storage domains, and synchronized cloud control planes with local data center resources.
    * **Goal:** Ensure systems remain secure, comply with strict regulatory frameworks, and enable the company to recover data and resume operations within minutes in the event of a natural disaster or massive technical failure.

* **Automation & Configuration Management:**
    * **Objective:** Eliminate repetitive manual server provisioning by deploying intelligent automation frameworks (Ansible, Chef, Rundeck, and ArgoCD).
    * **Implementation (How I did it):** Authored infrastructure recipes and automation playbooks, scheduled recurring jobs in Rundeck, and configured continuous state synchronization across host fleets.
    * **Goal:** Mitigate human error. Instead of technicians manually installing software across 2,000 servers, leverage scripts or automated programs to deploy changes identically and concurrently.

* **CI/CD Pipelines & Containerization:**
    * **Objective:** Build automated digital deployment pipelines (using GitLab and Jenkins) that continuously test and ship software releases, encapsulating workloads into standardized containerization formats (Docker and Kubernetes).
    * **Implementation (How I did it):** Built multi-stage pipeline configuration files, set up container registries, defined Kubernetes deployment manifests and Helm charts, and configured automated code-testing hooks.
    * **Goal:** Empower developers to deliver patches and feature enhancements rapidly and securely to production environments with zero downtime.

* **Infrastructure as Code (IaC):**
    * **Objective:** Define server states, parameters, and infrastructure configurations declaratively through code files (using Terraform and Packer) rather than manual configurations.
    * **Implementation (How I did it):** Wrote modular Terraform state scripts to provision infrastructure resources and utilized Packer to automate the generation of immutable golden machine images.
    * **Goal:** Guarantee that in the event of a server failure, the entire infrastructure can be rapidly reconstructed to its exact previous state within minutes, functioning like an identical 3D blueprint.

* **System Monitoring & Troubleshooting (Observability & RCA):**
    * **Objective:** Act as the ultimate L3/L4 technical escalation lead and forensic investigator during critical production incidents using monitoring toolsets like Datadog, Splunk, and Nagios.
    * **Implementation (How I did it):** Configured metric collection agents, set up custom telemetry dashboards, parsed centralized log queries, and executed deep log-trace analyses to isolate system bottlenecks.
    * **Goal:** Detect anomalies proactively before end-users are impacted, execute precise Root Cause Analysis (RCA), and implement automated safeguards to prevent recurring failures.

* **Scripting & Workflow Automation:**
    * **Objective:** Develop robust automation scripts and utility tools using Bash, Python, and PowerShell.
    * **Implementation (How I did it):** Authored executable scripts, scheduled cron jobs and task schedulers, and integrated custom parsing tools to automate system administration workflows.
    * **Goal:** Offload heavy routine operational tasks to automation, such as automated nightly security audits, mass identity and access management (IAM), and disk cleanup routines.

* **Large-Scale Cloud Migrations:**
    * **Objective:** Migrate legacy on-premise data center workloads and traditional applications successfully to scalable cloud-based virtual environments.
    * **Implementation (How I did it):** Executed lift-and-shift strategies, re-architected networking topologies, synchronized storage volumes, and validated system integrity post-migration.
    * **Goal:** Optimize physical hardware capital expenditures, enhance system performance and responsiveness, and modernize corporate technology stacks with zero data loss.

* **Technical Leadership & Collaborative Culture:**
    * **Objective:** Mentor engineering peers, author comprehensive technical documentation and runbooks, and champion Agile and DevOps working methodologies.
    * **Implementation (How I did it):** Facilitated technical workshops, established internal wikis and standard operating procedures (SOPs), and guided cross-functional planning sessions.
    * **Goal:** Foster cross-functional team collaboration, accelerate onboarding ramp-up times for new hires, and eliminate single points of failure by preventing knowledge silos.

---

### Site Reliability Engineer, 01/2024 to 01/2025
**FALABELLA BANK – Santiago De Chile**

#### Core Infrastructure and Operations Management

* **Operated and supported large-scale distributed systems across hybrid-cloud (Azure, OCI) and on-premise environments, ensuring high availability (HA), disaster recovery (DR) readiness, and end-to-end service resilience:**
    * **Objective:** Guarantee operational continuity and fault tolerance across complex, heterogeneous architectures.
    * **Implementation (How I did it):** Configured redundant enterprise nodes, established geo-replicated data backups, and monitored infrastructure health loops across hybrid cloud tenants.
    * **Goal:** Maintain uptime for mission-critical services and ensure seamless execution of disaster recovery (DR) plans.

* **Designed and deployed Infrastructure as Code (IaC) and configuration management frameworks for hybrid environments using Terraform, Packer, Chef, and Ansible, minimizing manual interventions and configuration drift:**
    * **Objective:** Automate infrastructure provisioning and standardize server configuration management.
    * **Implementation (How I did it):** Authored modular Terraform templates, built immutable machine images using Packer, and deployed Chef/Ansible playbooks to synchronize host configurations.
    * **Goal:** Eliminate environment discrepancies (configuration drift) and compress infrastructure deployment lead times from days down to minutes.

* **Engineered and scaled automated container platforms (Docker, Kubernetes) while planning and executing migrations to enhance workload scalability and fault tolerance:**
    * **Objective:** Modernize application architecture through the enterprise adoption of containerization technologies.
    * **Implementation (How I did it):** Packaged legacy runtimes into Docker containers, defined Kubernetes orchestration manifests, and deployed cluster management policies.
    * **Goal:** Enable seamless horizontal service auto-scaling and ensure robust workload resilience in production environments.

#### Automation, CI/CD, and Reliability

* **Designed and implemented automated CI/CD pipelines and deployment workflows (GitLab CI, Jenkins) integrated with Jira to enforce complete traceability from tickets to production:**
    * **Objective:** Streamline the software development lifecycle (SDLC) while maintaining rigorous audit compliance and tracking.
    * **Implementation (How I did it):** Built multi-stage pipeline configuration files, hooked repository events, and mapped automated webhook triggers between Jira issue keys and deployment scripts.
    * **Goal:** Achieve secure, continuous, and automated deployments bridging Jira task planning directly to production releases.

* **Defined and implemented SLIs, SLOs, and SLAs, leveraging data-driven metrics to steer continuous reliability, performance optimization, and capacity planning initiatives:**
    * **Objective:** Quantitatively measure service health and align engineering metrics with overarching business objectives.
    * **Implementation (How I did it):** Configured performance telemetry gauges, established logging thresholds, and evaluated historical system capacity trends.
    * **Goal:** Establish strict performance thresholds (SLOs) and drive data-backed capacity planning decisions.

#### Observability, Incident Management, and Leadership

* **Troubleshot complex multi-layer production issues spanning operating systems, network topologies, infrastructure components, and application layers, minimizing mean time to resolution (MTTR):**
    * **Objective:** Execute holistic root-cause debugging across the entire multi-tier technology stack.
    * **Implementation (How I did it):** Analyzed multi-layer error logs, traced packet flows across virtual networks, and inspected runtime configurations to isolate root failures.
    * **Goal:** Minimize Mean Time to Resolution (MTTR) to reduce adverse business impact.

* **Built comprehensive observability and monitoring stacks using Datadog, Prometheus, Grafana, Splunk, Instana, ELK, and Nagios, optimizing alert quality to reduce alert fatigue and enhance proactive detection:**
    * **Objective:** Maximize system state visibility and pivot operational posture from reactive to proactive monitoring.
    * **Implementation (How I did it):** Deployed agent collectors, built customized telemetry dashboards, and configured targeted notification thresholds for performance metrics.
    * **Goal:** Suppress operational noise (alert fatigue) through actionable alerting thresholds and early anomaly detection.

* **Led the full incident lifecycle—covering triage, deep-dive debugging, Root Cause Analysis (RCA), and blame-free postmortems—to formulate robust preventive strategies:**
    * **Objective:** Standardize incident response mechanisms and institutionalize learnings from failures to prevent recurrence.
    * **Implementation (How I did it):** Facilitated triage calls, authored formal technical postmortem documentation, and tracked corrective action items through engineering backlogs.
    * **Goal:** Drive actionable postmortem outcomes focused on long-term preventative hardening and architectural mitigations.

* **Provided technical leadership and mentorship across engineering teams, promoting SRE culture, establishing DevOps best practices, and facilitating technical knowledge transfer:**
    * **Objective:** Drive the organizational adoption of SRE culture and engineering best practices.
    * **Implementation (How I did it):** Led peer review sessions, conducted technical enablement workshops, and published internal documentation and runbooks.
    * **Goal:** Cultivate internal technical talent, unify operational standards, and streamline cross-functional knowledge sharing.

---

### Devops & Platform Engineer, 04/2021 to 01/2024
**FALABELLA BANK – Santiago De Chile**

* **CI/CD Automation and Traceability**
    * **Objective:** Design and maintain continuous integration and continuous deployment (CI/CD) pipelines utilizing GitLab CI/CD and Jenkins to standardize builds and automated testing workflows; Integrate Jira with GitLab to drive automated, ticket-driven deployments, establishing absolute traceability from task planning through to production execution.
    * **Implementation (How I did it):** Configured pipeline YAML scripts, connected repository hooks with Jenkins build triggers, and mapped Jira issue keys directly to deployment metadata.
    * **Goal:** Accelerate the Software Development Life Cycle (SDLC) by shortening time-to-market for production releases while mitigating human error and ensuring transparent, auditable change management.

* **Infrastructure as Code (IaC) and Configuration Management**
    * **Objective:** Orchestrate resource provisioning across both on-premise data centers and Cloud environments leveraging Terraform, Packer, and Chef; Automate OS configurations and platform consistency using Ansible, Chef, and custom Bash/PowerShell scripts to guarantee environment parity from development to production.
    * **Implementation (How I did it):** Authored declarative Terraform files, built custom images using Packer, and deployed configuration management playbooks to align system states across servers.
    * **Goal:** Eliminate manual server configurations (configuration drift) and empower infrastructure to scale elastically, reliably, and audibly under the Infrastructure as Code paradigm.

* **Containerization and Microservices Orchestration**
    * **Objective:** Standardize application packaging via Docker to streamline local testing workflows and deployment predictability; Execute technical migrations from isolated Docker architectures to Kubernetes (K8s) clusters, unlocking high availability, self-healing capabilities, and advanced microservices management.
    * **Implementation (How I did it):** Created multi-stage Dockerfiles, built container images, and established Kubernetes deployment manifests, services, and cluster scaling policies.
    * **Goal:** Decouple applications from underlying host environments to guarantee absolute code portability, optimize resource utilization, and facilitate horizontal scaling.

* **Cloud Migrations and Production Support**
    * **Objective:** Execute secure migrations of on-premise workloads to Cloud platforms following industry-standard architecture and security best practices; Provide advanced-level production support to engineering teams, conducting rigorous Root Cause Analysis (RCA) for critical incidents to implement definitive preventive fixes.
    * **Implementation (How I did it):** Mapped out data transfer workflows, reconfigured network routing for cloud endpoints, and debugged production service logs to pinpoint system failures.
    * **Goal:** Modernize legacy technology stacks by migrating workloads to the Cloud with zero business disruption, ensuring robust operational resilience.

* **Observability, Monitoring, and Operations**
    * **Objective:** Consolidate telemetry, metrics, distributed tracing, and centralized logging solutions utilizing industry-leading tools such as Datadog, Prometheus, Instana, the ELK stack, and Icinga/Nagios; Build performance-centric dashboards and early-warning alerting mechanisms to monitor real-time service health.
    * **Implementation (How I did it):** Deployed telemetry agents across nodes, built customized visualization panels, and tuned alerting triggers to capture resource thresholds.
    * **Goal:** Transition from a reactive support model to a proactive operational posture, drastically reducing Mean Time to Detection (MTTD) and Mean Time to Resolution (MTTR).

* **Technical Leadership and Documentation**
    * **Objective:** Develop internal tooling and automation scripts for recurring operational overhead (e.g., access management and auditing), boosting overall engineering productivity; Provide technical mentorship to team members, guide the adoption of emerging technologies, and author comprehensive architectural documentation to ensure long-term maintainability.
    * **Implementation (How I did it):** Authored automation scripts for task execution, led pairing sessions and enablement workshops for peers, and structured technical wikis and design blueprints.
    * **Goal:** Scale DevOps culture across the organization by standardizing operational workflows, reducing manual toil, and driving efficient technical knowledge transfer.

---

### System Engineer, 07/2019 to 03/2021
**KIBERNUM CHILE – Santiago De Chile**

* **Cloud/Virtualization Migration and Infrastructure Provisioning**
    * **Objective:** Collaborate externally on enterprise-level migration projects (such as migrating workloads to VMware 6.5 for corporate banking clients like Falabella Financiero); Design, build, and configure scalable infrastructure utilizing Terraform, Packer, and Chef to automate server provisioning and enforce environment consistency.
    * **Implementation (How I did it):** Wrote declarative Terraform state scripts, built custom golden machine images with Packer, and executed Chef recipes to automate multi-tier server provisioning and migration sequences.
    * **Goal:** Modernize enterprise infrastructure architectures, replace manual deployment bottlenecks with repeatable automated templates, and ensure seamless cross-platform environment migration.

* **Container Orchestration, Middleware, and Database Operations**
    * **Objective:** Administer containerized microservices and service meshes using Docker Swarm and Kong, alongside enterprise middleware environments such as Oracle Service Bus (OSB) and WebLogic (WL), handling end-to-end incident troubleshooting via Jira; Manage database servers and platform runtimes to guarantee high performance and low-latency data processing.
    * **Implementation (How I did it):** Deployed container clusters on Docker Swarm, configured Kong API routes, managed OSB/WebLogic domain parameters, and resolved service tickets logged in Jira.
    * **Goal:** Maintain resilient application runtime environments, secure API gateways, and robust clustered middleware platforms capable of handling high-transaction financial services.

* **Observability, Metrics, and System Documentation**
    * **Objective:** Configure and administer comprehensive monitoring, logging, and alerting stacks using Kibana, Instana, Icinga, and Nagios for deep telemetry tracking; Produce meticulous technical documentation detailing system configurations, architectural mapping, and operational standard operating procedures (SOPs).
    * **Implementation (How I did it):** Set up dashboards in Kibana and Instana, defined health check rules in Icinga and Nagios, and authored technical design documents mapping system topologies.
    * **Goal:** Shift from reactive troubleshooting to deep system visibility, ensuring real-time performance tracking, historical log analysis, and continuous knowledge transfer.

* **Systems Engineering Architecture and Feasibility**
    * **Objective:** Evaluate system constraints and propose technically feasible engineering solutions for greenfield system designs and infrastructure expansions; Conduct thorough technical reviews and performance assessments to recommend strategic system updates, patches, or architectural refactoring.
    * **Implementation (How I did it):** Performed infrastructure capacity audits, reviewed resource utilization metrics, and drafted technical proposal documents for system upgrades and refactoring initiatives.
    * **Goal:** Align technical architecture with rigorous system stability standards, ensuring that new implementations scale efficiently without introducing technical debt or performance degradation.

* **IT Governance, Stakeholder Collaboration, and Operations Support**
    * **Objective:** Partner directly with executive leadership and cross-functional stakeholders to translate high-level business goals into precise technical requirements; Deliver second/third-line helpdesk technical assistance, execute end-user reviews for newly deployed systems, and safeguard overall platform stability, security integrity, and efficient daily operations.
    * **Implementation (How I did it):** Facilitated alignment meetings with executives, provided tier-2/3 troubleshooting support for end users, and performed compliance assessments on deployed systems.
    * **Goal:** Bridge the gap between business strategy and IT execution, ensuring optimal user adoption, strict SLA adherence, and long-term infrastructure health.

---

### Infrastructure Consultant, 03/2019 to 06/2019
**ACTIVEIT CONSULTORES – Santiago De Chile**

* **Virtualization Management and Data Protection**
    * **Objective:** Specialize in virtual infrastructure administration via VMware and enterprise data protection strategies using Veeam Backup & Replication.
    * **Implementation (How I did it):** Configured ESXi hosts, managed vCenter clusters, provisioned virtual machines, and designed backup/restore policies, synthetic full backups, and replication jobs to secure critical workloads.
    * **Goal:** Ensure high availability, zero data loss, and rapid disaster recovery capabilities across the virtualized environment.

* **Cross-Platform Systems Administration**
    * **Objective:** Manage and optimize mixed-OS enterprise environments running Windows Server and Linux distributions.
    * **Implementation (How I did it):** Handled user management, patch management, service configuration, performance tuning, and resource allocation across physical and virtual nodes.
    * **Goal:** Guarantee optimal functionality, system security integrity, and high performance for business-critical applications.

* **Disaster Recovery Planning and Backup Monitoring**
    * **Objective:** Collaborate with cross-functional teams on Disaster Recovery (DR) planning, network backup strategies, and continuous recovery process auditing.
    * **Implementation (How I did it):** Executed periodic restore drills, tested backup integrity, monitored backup job queues, and documented recovery time objectives (RTO) and recovery point objectives (RPO).
    * **Goal:** Mitigate operational risks and ensure business continuity in the event of unexpected hardware failure, data corruption, or system outages.

* **Hardware/Software Lifecycle and Best Practices Maintenance**
    * **Objective:** Install, support, and maintain corporate hardware and software infrastructure adhering to industry best practices.
    * **Implementation (How I did it):** Managed physical server deployments, component replacements, firmware/BIOS updates, software installations, and license compliance audits.
    * **Goal:** Maximize hardware lifespan, prevent unexpected downtime, and maintain a standardized, secure technology asset inventory.

* **Network Performance Analysis and Troubleshooting**
    * **Objective:** Analyze, troubleshoot, and enhance network performance to drive operational efficiency.
    * **Implementation (How I did it):** Diagnosed latency issues, monitored bandwidth utilization, analyzed packet flows, and optimized network routing or firewall rules to resolve connectivity bottlenecks.
    * **Goal:** Deliver reliable, high-speed network connectivity that supports seamless communication across internal services and users.

* **Technical Leadership and Project Coordination**
    * **Objective:** Lead professional team development and assist in project management coordination for infrastructure initiatives.
    * **Implementation (How I did it):** Mentored junior engineers, structured task distribution, tracked milestones, and collaborated on technical scoping for infrastructure upgrades.
    * **Goal:** Align technical execution with project timelines while fostering skill growth and operational synergy within the IT team.

---

### Infrastructure Administrator, 08/2017 to 03/2019
**ANIDA CONSULTORES – Santiago De Chile**

* **On-Site Physical Infrastructure and Hardware Support**
    * **Objective:** Deliver robust on-site infrastructure support for mission-critical services deployed on HP ProLiant DL Series servers.
    * **Implementation (How I did it):** Handled hardware installations, component replacements, firmware/BIOS upgrades, and physical rack-and-stack deployments in the data center.
    * **Goal:** Maximize physical server availability, minimize hardware-related downtime, and maintain a highly stable physical core.

* **Virtualized Infrastructure and OS Administration**
    * **Objective:** Administer and support enterprise virtualized environments (VMware vSphere 5.5 and 6.5) alongside multi-OS operating systems (Windows Server, Linux, and ESXi).
    * **Implementation (How I did it):** Provisioned virtual machines, executed physical-to-virtual (P2V) and virtual-to-virtual migrations, and managed day-to-day OS patching, tuning, and troubleshooting.
    * **Goal:** Optimize resource utilization, enhance workload portability, and establish a resilient foundation for business applications.

* **Advanced VMware Networking and Clustering**
    * **Objective:** Configure and manage advanced virtualization networking features and enterprise clustering topologies (VDS, VSS, VLAN, vMotion, DRS, HA, LUN, and failover clusters).
    * **Implementation (How I did it):** Implemented distributed switches, configured multi-path storage LUNs, tuned Distributed Resource Scheduler (DRS) rules, and validated High Availability (HA) failover triggers.
    * **Goal:** Eliminate single points of failure, ensure seamless workload migration, and guarantee high availability for clustered services.

* **Data Center Physical Design, Cabling, and Documentation**
    * **Objective:** Architect, install, and map physical data center facilities using high-availability best practices and enterprise documentation tools.
    * **Implementation (How I did it):** Deployed enterprise switches, structured fiber optic cabling, organized Power Distribution Units (PDUs) for redundant power feeds, and created detailed server layouts and physical network mappings using Microsoft Visio.
    * **Goal:** Maintain an immaculate, auditable, and fault-tolerant physical data center layout that prevents accidental outages and simplifies asset tracking.

* **ITIL Service Management and Incident Resolution**
    * **Objective:** Process Request for Changes (RFCs under ITIL v3 framework) and manage customer requirements and incidents using ticketing systems like AutoTask and HPSM.
    * **Implementation (How I did it):** Triage incoming tickets, troubleshoot complex infrastructure errors, execute formal change management workflows, and coordinate technical escalations with service level agreements (SLAs) in mind.
    * **Goal:** Deliver structured, high-quality technical support while ensuring operational changes are fully tracked, reviewed, and risk-mitigated.

---

## Education

* **Rodolfo Loero Arismendi University Technical Industrial Institute**, 03/2010 – Caracas Venezuela — Bachelor's degree, Information Technology

---

## Certifications

* **Microsoft Certified: Azure Fundamentals 2026**
* **AWS Certified Cloud Practitioner 2024**
