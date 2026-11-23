# Production Code Blueprints

Sanitized, production-grade configuration templates demonstrating **Infrastructure as Code (IaC)**, **GitLab CI/CD governance**, **Day-2 fleet automation**, and **Kubernetes GitOps observability** across hybrid enterprise environments.

> **Security Note:** All tenant IDs, subscription identifiers, internal endpoints, and credentials have been sanitized or replaced with environment variables and Managed Identities.

---

## 1. Hybrid Cloud Provisioning (Terraform + Native AlmaLinux)

Declarative module provisioning an Availability Zone-resilient Linux application node on **Microsoft Azure** using a native **AlmaLinux** marketplace image, NSG micro-segmentation, **System-Assigned Managed Identity** (for passwordless Key Vault access), and `cloud-init` bootstrapping for telemetry agents.

=== "main.tf"

    ```hcl
    terraform {
      required_version = ">= 1.6.0"

      required_providers {
        azurerm = {
          source  = "hashicorp/azurerm"
          version = "~> 3.110"
        }
      }

      backend "azurerm" {
        resource_group_name  = "rg-platform-tfstate-prod"
        storage_account_name = "stplatformtfstateprod"
        container_name       = "tfstate"
        key                  = "omnichannel/app-tier.tfstate"
      }
    }

    provider "azurerm" {
      features {}
    }

    resource "azurerm_network_security_group" "app_nsg" {
      name                = "nsg-${var.workload_name}-${var.environment}"
      location            = var.location
      resource_group_name = var.resource_group_name
      tags                = var.common_tags

      security_rule {
        name                       = "Allow-Kong-Gateway-Ingress"
        priority                   = 100
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_ranges    = ["8080", "8443"]
        source_address_prefix      = var.gateway_subnet_cidr
        destination_address_prefix = "*"
      }

      security_rule {
        name                       = "Allow-Prometheus-Scrape"
        priority                   = 110
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_range     = "9100"
        source_address_prefix      = var.monitoring_subnet_cidr
        destination_address_prefix = "*"
      }
    }

    resource "azurerm_network_interface" "app_nic" {
      name                = "nic-${var.workload_name}-${var.environment}-01"
      location            = var.location
      resource_group_name = var.resource_group_name
      tags                = var.common_tags

      ip_configuration {
        name                          = "internal"
        subnet_id                     = var.app_subnet_id
        private_ip_address_allocation = "Dynamic"
      }
    }

    resource "azurerm_network_interface_security_group_association" "app_nsg_assoc" {
      network_interface_id      = azurerm_network_interface.app_nic.id
      network_security_group_id = azurerm_network_security_group.app_nsg.id
    }

    resource "azurerm_linux_virtual_machine" "app_node" {
      name                            = "vm-${var.workload_name}-${var.environment}-01"
      resource_group_name             = var.resource_group_name
      location                        = var.location
      zone                            = "1"
      size                            = var.vm_size
      admin_username                  = "azureops"
      disable_password_authentication = true
      network_interface_ids           = [azurerm_network_interface.app_nic.id]
      tags                            = var.common_tags

      admin_ssh_key {
        username   = "azureops"
        public_key = var.ops_ssh_public_key
      }

      source_image_reference {
        publisher = "almalinux"
        offer     = "almalinux-x86_64"
        sku       = "9-gen2"
        version   = "latest"
      }

      os_disk {
        caching              = "ReadWrite"
        storage_account_type = "Premium_ZRS"
        disk_size_gb         = 64
      }

      identity {
        type = "SystemAssigned"
      }

      custom_data = base64encode(templatefile("${path.module}/templates/cloud-init.yaml.tftpl", {
        environment = var.environment
        workload    = var.workload_name
      }))
    }
    ```

=== "variables.tf"

    ```hcl
    variable "environment" {
      description = "Deployment environment (dev, qa, prod)"
      type        = string
      validation {
        condition     = contains(["dev", "qa", "prod"], var.environment)
        error_message = "Environment must be dev, qa, or prod."
      }
    }

    variable "workload_name" {
      description = "Service or middleware tier identifier"
      type        = string
      default     = "omnichannel-api"
    }

    variable "location" {
      type    = string
      default = "eastus2"
    }

    variable "resource_group_name" {
      type = string
    }

    variable "app_subnet_id" {
      type = string
    }

    variable "gateway_subnet_cidr" {
      type    = string
      default = "10.240.10.0/24"
    }

    variable "monitoring_subnet_cidr" {
      type    = string
      default = "10.240.99.0/24"
    }

    variable "vm_size" {
      type    = string
      default = "Standard_D4s_v5"
    }

    variable "ops_ssh_public_key" {
      type      = string
      sensitive = true
    }

    variable "common_tags" {
      type = map(string)
      default = {
        managed_by  = "terraform"
        cost_center = "platform-engineering"
        compliance  = "pci-dss"
      }
    }
    ```

=== "cloud-init.yaml.tftpl"

    ```yaml
    #cloud-config
    package_update: true
    packages:
      - jq
      - curl
      - chrony
      - policycoreutils-python-utils

    write_files:
      - path: /etc/profile.d/platform-env.sh
        permissions: "0644"
        content: |
          export PLATFORM_ENV="${environment}"
          export WORKLOAD_TIER="${workload}"

    runcmd:
      - systemctl enable --now chronyd
      - timedatectl set-timezone America/Santiago
      - echo "Bootstrap complete for ${workload} (${environment})" > /var/log/platform-bootstrap.log
    ```

### Infrastructure Architecture & Security Controls

* **Remote State Governance (`terraform` / `backend "azurerm"`):** Pins Terraform (`>= 1.6.0`) and the `hashicorp/azurerm` provider (`~> 3.110`) while persisting state remotely in an isolated Azure Storage container (`rg-platform-tfstate-prod`) with native blob lease locking.
* **Network Micro-Segmentation (`azurerm_network_security_group`):** Enforces least-privilege ingress at the NIC level—restricting application traffic (`8080/8443`) exclusively to the **Kong API Gateway** subnet CIDR and telemetry scraping (`9100`) to the **Prometheus** monitoring subnet CIDR.
* **Hardened Compute & Availability Zone Resilience (`azurerm_linux_virtual_machine`):** Provisions a Gen2 **AlmaLinux 9** marketplace image pinned to Availability Zone `1`, backed by zone-redundant managed storage (`Premium_ZRS`) and key-based SSH authentication (`disable_password_authentication = true`).
* **Passwordless Identity & Immutable Bootstrapping (`identity` / `custom_data`):** Attaches a `SystemAssigned` Managed Identity for zero-secret **Azure Key Vault** integration and renders a base64-encoded `cloud-init` template (`cloud-init.yaml.tftpl`) to configure time sync (`chronyd`), timezone standards, and runtime environment variables at first boot.

### Immutable First-Boot Provisioning (`cloud-init.yaml.tftpl`)

* **Baseline Package & SELinux Tooling (`package_update` / `packages`):** Refreshes DNF repositories on first boot and installs core operational and security utilities (`jq`, `curl`, `chrony`, and `policycoreutils-python-utils` for SELinux policy management).
* **Environment Metadata Injection (`write_files`):** Renders Terraform variables (`${environment}`, `${workload}`) into `/etc/profile.d/platform-env.sh` (`0644`), exposing standardized runtime context to shell sessions, deployment scripts, and observability agents.
* **Time Synchronization & Audit Trail (`runcmd`):** Enables `chronyd` and enforces deterministic timezone configuration (`America/Santiago`) to prevent TLS handshake failures and log timestamp drift across distributed tiers, logging bootstrap completion to `/var/log/platform-bootstrap.log`.

---

## 2. Ticket-Traceable Infrastructure Pipeline (GitLab CI/CD)

Multi-stage **GitLab CI/CD** pipeline enforcing Terraform formatting, static security scanning (`tfsec`), Jira ticket traceability in merge requests, remote state locking, and a gated manual `apply` step for **Technical Lead / CAB** approval.

```yaml
stages:
  - validate
  - security
  - plan
  - apply

variables:
  TF_ROOT: "${CI_PROJECT_DIR}/environments/prod"
  TF_IN_AUTOMATION: "true"

default:
  image:
    name: hashicorp/terraform:1.8.5
    entrypoint: [""]
  before_script:
    - cd "${TF_ROOT}"
    - terraform --version
    - terraform init -reconfigure

terraform:validate:
  stage: validate
  script:
    - terraform fmt -check -recursive
    - terraform validate
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
    - if: $CI_COMMIT_BRANCH ==$CI_DEFAULT_BRANCH

terraform:security-scan:
  stage: security
  image:
    name: aquasec/tfsec:latest
    entrypoint: [""]
  before_script: []
  script:
    - tfsec "${TF_ROOT}" --minimum-severity HIGH
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
    - if: $CI_COMMIT_BRANCH ==$CI_DEFAULT_BRANCH

terraform:plan:
  stage: plan
  script:
    - |
      if ! echo "${CI_COMMIT_TITLE}" | grep -Eq '^[A-Z]+-[0-9]+'; then
        echo "ERROR: Commit message must reference a valid Jira ticket (e.g., PLAT-1420)."
        exit 1
      fi
    - terraform plan -out=tfplan.binary
    - terraform show -no-color tfplan.binary > tfplan.txt
  artifacts:
    name: "tfplan-${CI_COMMIT_SHORT_SHA}"
    paths:
      - "${TF_ROOT}/tfplan.binary"
      - "${TF_ROOT}/tfplan.txt"
    expire_in: 7 days
  rules:
    - if: $CI_COMMIT_BRANCH ==$CI_DEFAULT_BRANCH

terraform:apply:
  stage: apply
  script:
    - terraform apply -input=false -auto-approve tfplan.binary
  dependencies:
    - terraform:plan
  environment:
    name: production
  when: manual
  rules:
    - if: $CI_COMMIT_BRANCH ==$CI_DEFAULT_BRANCH
```
### Pipeline Architecture & Execution Controls

* **Execution Context & State Locking (`default` / `variables`):** Targets the isolated `environments/prod` root module using `hashicorp/terraform:1.8.5` with `TF_IN_AUTOMATION=true` and `terraform init -reconfigure` against a remote state backend to prevent concurrent state corruption.
* **Static Validation (`terraform:validate`):** Enforces canonical HCL formatting (`terraform fmt -check -recursive`) and syntax/reference integrity (`terraform validate`) on every Merge Request and default branch commit.
* **IaC Security Gate (`terraform:security-scan`):** Runs static security analysis via `aquasec/tfsec`, blocking the pipeline on `HIGH` or `CRITICAL` misconfigurations (e.g., overly permissive CIDR rules, unencrypted storage) before plan generation.
* **Ticket Traceability & Immutable Plan Artifacts (`terraform:plan`):** Enforces commit-level **Jira** ticket validation (`^[A-Z]+-[0-9]+`) on the default branch and exports both the binary execution plan (`tfplan.binary`) and human-readable diff (`tfplan.txt`) as 7-day auditable artifacts.
* **Gated Production Rollout (`terraform:apply`):** Requires manual authorization (`when: manual`) under the protected `production` environment (Technical Lead / CAB sign-off) and executes strictly against the exact `tfplan.binary` artifact produced in the previous stage to eliminate plan-to-apply drift.

## 3. Hardening & Kubernetes GitOps Telemetry

=== "Ansible: OS Hardening & FS Hygiene"

    Idempotent **Ansible** playbook used across RHEL/AlmaLinux fleets (and triggered via **Rundeck** jobs) to enforce SSH security baselines, verify timezone/DST compliance, and purge stale application logs before filesystem saturation occurs.

    ```yaml
    ---
    - name: Enterprise Linux Hardening & Preventive Log Maintenance
      hosts: linux_app_tiers
      become: true
      vars:
        log_retention_days: 14
        app_log_dirs:
          - /opt/middleware/domains/prod_domain/servers/logs
          - /var/log/kong

      tasks:
        - name: Enforce hardened SSH daemon configuration
          ansible.builtin.lineinfile:
            path: /etc/ssh/sshd_config
            regexp: "{{ item.regexp }}"
            line: "{{ item.line }}"
            validate: "/usr/sbin/sshd -t -f %s"
          loop:
            - { regexp: '^#?PermitRootLogin', line: 'PermitRootLogin no' }
            - { regexp: '^#?PasswordAuthentication', line: 'PasswordAuthentication no' }
            - { regexp: '^#?MaxAuthTries', line: 'MaxAuthTries 3' }
          notify: Reload sshd

        - name: Verify NTP / Chrony synchronization for DST accuracy
          ansible.builtin.command: chronyc tracking
          register: chrony_status
          changed_when: false
          failed_when: "'Normal' not in chrony_status.stdout"

        - name: Find archived application logs older than retention threshold
          ansible.builtin.find:
            paths: "{{ app_log_dirs }}"
            patterns: "*.log.*,*.gz"
            age: "{{ log_retention_days }}d"
            recurse: true
          register: stale_logs

        - name: Purge rotated logs to prevent filesystem saturation
          ansible.builtin.file:
            path: "{{ item.path }}"
            state: absent
          loop: "{{ stale_logs.files }}"
          loop_control:
            label: "{{ item.path }}"

      handlers:
        - name: Reload sshd
          ansible.builtin.service:
            name: sshd
            state: reloaded
    ```
### Fleet Hygiene & Hardening Controls (`Ansible` + `Rundeck`)

* **Syntax-Validated SSH Hardening (`ansible.builtin.lineinfile`):** Enforces security compliance baselines across `linux_app_tiers` by disabling root login, enforcing key-based authentication, and capping `MaxAuthTries` to `3`—validating configuration syntax via `sshd -t` prior to reloading the daemon via idempotent handlers.
* **Deterministic Time & DST Verification (`chronyc tracking`):** Audits NTP synchronization health across distributed application nodes without mutating state (`changed_when: false`), failing fast on clock drift to protect TLS handshake integrity and cross-tier log correlation.
* **Automated Filesystem & Log Lifecycle Governance (`find` + `file`):** Continuously identifies and purges rotated/compressed middleware and **Kong** gateway logs (`*.log.*`, `*.gz`) exceeding the 14-day retention policy—eliminating disk-saturation incidents and manual operational toil when orchestrated via **Rundeck** jobs.

=== "AKS + Prometheus ServiceMonitor"

    Declarative Kubernetes deployment synchronized by **ArgoCD** on **Azure Kubernetes Service (AKS)**—pulling images from **ACR** via Workload/Managed Identity (`AcrPull` RBAC) and exposing HTTP latency (`p95/p99`) metrics to **Prometheus**.

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: omnichannel-gateway-adapter
      namespace: banking-core
      labels:
        app: omnichannel-gateway-adapter
        tier: middleware
    spec:
      replicas: 3
      strategy:
        type: RollingUpdate
        rollingUpdate:
          maxSurge: 1
          maxUnavailable: 0
      selector:
        matchLabels:
          app: omnichannel-gateway-adapter
      template:
        metadata:
          labels:
            app: omnichannel-gateway-adapter
        spec:
          containers:
            - name: adapter
              image: acrbankingprod.azurecr.io/omnichannel-adapter:v2.14.0
              ports:
                - name: http
                  containerPort: 8080
                - name: metrics
                  containerPort: 9090
              resources:
                requests:
                  cpu: "250m"
                  memory: "512Mi"
                limits:
                  cpu: "1000m"
                  memory: "1Gi"
              readinessProbe:
                httpGet:
                  path: /actuator/health/readiness
                  port: http
                initialDelaySeconds: 10
                periodSeconds: 5
              livenessProbe:
                httpGet:
                  path: /actuator/health/liveness
                  port: http
                initialDelaySeconds: 20
                periodSeconds: 10
    ---
    ```

---

## 7. SLO-Driven SRE Alerting & Predictive Capacity Rules (PrometheusRule)

```yaml
apiVersion: monitoring.coreos.com/v1
    kind: ServiceMonitor
    metadata:
      name: omnichannel-adapter-monitor
      namespace: monitoring
      labels:
        release: prometheus-stack
    spec:
      namespaceSelector:
        matchNames:
          - banking-core
      selector:
        matchLabels:
          app: omnichannel-gateway-adapter
      endpoints:
        - port: metrics
          path: /actuator/prometheus
          interval: 15s
    ```
### Cloud-Native GitOps & Telemetry Architecture (`AKS` + `Prometheus`)

* **Zero-Downtime Rolling Deployments (`Deployment` / `RollingUpdate`):** Runs 3 replicas across AKS nodes with `maxUnavailable: 0` and `maxSurge: 1`, ensuring continuous API availability during **ArgoCD**-synchronized image rollouts pulled passwordlessly from **Azure Container Registry (ACR)** via `AcrPull` Managed Identity RBAC.
* **Resource Governance & Health Probes (`resources` / `readinessProbe` / `livenessProbe`):** Enforces deterministic CPU/memory boundaries (`250m–1000m` CPU, `512Mi–1Gi` RAM) to prevent noisy-neighbor contention while decoupling traffic routing (`/actuator/health/readiness`) from container self-healing (`/actuator/health/liveness`).
* **Cross-Namespace Prometheus Auto-Discovery (`ServiceMonitor`):** Isolates transactional traffic (`8080`) from internal telemetry (`9090`) and configures **Prometheus Operator** (`release: prometheus-stack`) to scrape `/actuator/prometheus` every `15s` across the `banking-core` namespace for real-time `p95/p99` latency tracking and SRE alerting.

---

## 4. Edge Traffic Governance & L7 Load Balancing (Kong + HAProxy)

Declarative ingress and reverse-proxy blueprints governing east-west and north-south API traffic—enforcing JWT authentication, distributed rate limiting, TLS 1.3 termination, and active L7 health checks before requests reach middleware or containerized tiers.

=== "Kong Declarative (decK)"

    ```yaml
    _format_version: "3.0"
    _transform: true

    services:
      - name: omnichannel-core-service
        # Internal Kubernetes DNS resolution for the upstream AKS middleware service
        url: http://omnichannel-gateway-adapter.banking-core.svc.cluster.local:8080
        # Strict upstream timeouts to fail fast and prevent thread pool exhaustion
        connect_timeout: 5000
        read_timeout: 15000
        write_timeout: 15000
        retries: 2
        routes:
          - name: omnichannel-v2-route
            paths:
              - /api/v2/omnichannel
            methods:
              - GET
              - POST
            strip_path: false
            protocols:
              - https
        plugins:
          # Enforce zero-trust edge authentication via signed JWT validation
          - name: jwt
            config:
              claims_to_verify:
                - exp
              key_claim_name: iss
          # Distributed rate limiting backed by Redis; fails open if cache is degraded
          - name: rate-limiting
            config:
              minute: 1200
              policy: redis
              redis_host: redis-ratelimit.platform-cache.svc.cluster.local
              redis_port: 6379
              fault_tolerant: true
          # Export granular per-consumer latency and status metrics to Prometheus
          - name: prometheus
            config:
              per_consumer: true
              status_code_metrics: true
              latency_metrics: true
    ```

=== "HAProxy L7 Load Balancer"

    ```haproxy
    global
        log /dev/log local0 info
        maxconn 50000
        user haproxy
        group haproxy
        daemon
        # Enforce modern TLS 1.3 ciphers only and disable session tickets for forward secrecy
        ssl-default-bind-ciphersuites TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
        ssl-default-bind-options ssl-min-ver TLSv1.3 no-tls-tickets

    defaults
        log     global
        mode    http
        option  httplog
        option  dontlognull
        # Preserve client source IP across proxy hops for audit logging
        option  forwardfor
        timeout connect 5s
        timeout client  30s
        timeout server  30s
        retries 2

    frontend https_edge_in
        # Terminate TLS with HTTP/2 ALPN negotiation enabled
        bind *:443 ssl crt /etc/haproxy/certs/api-gateway.pem alpn h2,http/1.1
        http-request set-header X-Forwarded-Proto https
        # Strip server technology headers to prevent version fingerprinting
        http-request del-header X-Powered-By
        acl is_middleware path_beg /api/v2/omnichannel
        use_backend middleware_app_cluster if is_middleware

    backend middleware_app_cluster
        # Route traffic to the node with fewest active connections
        balance leastconn
        # Active L7 health probe verifying Spring Boot readiness endpoint
        option httpchk GET /actuator/health/readiness HTTP/1.1\r\nHost:\ internal-health
        http-check expect status 200
        # 30s slowstart prevents cold JVM nodes from being overwhelmed post-deployment (PAP)
        default-server inter 5s fall 3 rise 2 slowstart 30s
        server app-node-01 10.240.20.11:8080 check
        server app-node-02 10.240.20.12:8080 check
        server app-node-03 10.240.20.13:8080 check
    ```

### Edge Traffic Architecture & Security Controls

* **Declarative API Gateway Policy (`Kong` / `decK`):** Enforces zero-trust edge authentication (`jwt` claim verification) and Redis-backed distributed rate limiting (`1,200 req/min` with `fault_tolerant: true` failover) while exporting per-consumer latency histograms to **Prometheus**.
* **Hardened TLS 1.3 Termination & Active Health Probing (`HAProxy`):** Restricts ingress ciphers exclusively to TLS 1.3 (`alpn h2,http/1.1`), strips server fingerprint headers, and routes traffic via `leastconn` with `slowstart 30s` warm-up windows to protect JVM/middleware nodes during production deployments (PAP).

---

## 5. Multi-Environment GitOps Control Plane (ArgoCD ApplicationSet)

Declarative **ArgoCD** `ApplicationSet` automating continuous reconciliation across `dev`, `qa`, and `prod` AKS clusters—enforcing automated drift self-healing, orphaned resource pruning, and resilient retry policies.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: omnichannel-platform-appset
  namespace: argocd
spec:
  # Matrix generator rendering isolated ArgoCD Applications per target AKS cluster
  generators:
    - list:
        elements:
          - env: dev
            cluster: https://aks-dev-eastus2.hcp.eastus2.azmk8s.io
            namespace: banking-core-dev
          - env: qa
            cluster: https://aks-qa-eastus2.hcp.eastus2.azmk8s.io
            namespace: banking-core-qa
          - env: prod
            cluster: https://aks-prod-eastus2.hcp.eastus2.azmk8s.io
            namespace: banking-core
  template:
    metadata:
      name: "omnichannel-adapter-{{env}}"
      labels:
        environment: "{{env}}"
        managed-by: argocd
    spec:
      project: core-banking
      source:
        repoURL: https://gitlab.enterprise.internal/platform/gitops-manifests.git
        targetRevision: main
        # Environment-specific Kustomize overlay path
        path: "overlays/{{env}}/omnichannel-adapter"
      destination:
        server: "{{cluster}}"
        namespace: "{{namespace}}"
      syncPolicy:
        automated:
          # Garbage-collect orphaned Kubernetes objects removed from Git
          prune: true
          # Automatically revert unauthorized manual kubectl mutations
          selfHeal: true
        syncOptions:
          - CreateNamespace=true
          - PrunePropagationPolicy=foreground
          - ApplyOutOfSyncOnly=true
        # Exponential backoff retry policy for transient API server or webhook failures
        retry:
          limit: 3
          backoff:
            duration: 10s
            factor: 2
            maxDuration: 3m

```

---

## 6. Self-Service Disaster Recovery & Controlled Rolling Restart (Rundeck)

Declarative **Rundeck** job definition empowering NOC/SRE teams to execute auditable, batch-controlled rolling restarts across enterprise middleware fleets with automated post-restart HTTP health verification and fail-fast halting.

```yaml
- defaultTab: nodes
  description: >-
    Controlled rolling restart for enterprise middleware tiers with pre-flight
    drain checks and post-restart HTTP readiness validation.
  executionEnabled: true
  group: Platform-Operations/Disaster-Recovery
  loglevel: INFO
  name: Rolling-Restart-Middleware-Tier
  nodeFilterEditable: false
  nodefilters:
    dispatch:
      excludePrecedence: true
      # Fail-fast circuit breaker: abort workflow immediately if any node fails
      keepgoing: false
      rankOrder: ascending
      # Serial execution (1 node at a time) to preserve cluster availability
      threadcount: 1
    filter: "tags: middleware-prod"
  options:
    # Enforce change-management traceability before allowing execution
    - name: change_ticket
      regex: "^[A-Z]+-[0-9]+$"
      required: true
      description: "Mandatory Jira/GSTI change ticket reference (e.g., CHG-4812)"
    - name: health_endpoint
      value: "http://localhost:8080/actuator/health/readiness"
      required: true
  scheduleEnabled: false
  sequence:
    keepgoing: false
    # Complete full restart + health validation per node before moving to the next
    strategy: node-first
    commands:
      - description: "1. Audit change ticket and pre-restart service state"
        exec: |
          echo "[AUDIT] Ticket ${option.change_ticket} initiated by ${job.username} on${node.name}"
          systemctl is-active middleware-adapter.service || true
      - description: "2. Graceful service restart with systemd timeout protection"
        exec: |
          sudo systemctl restart middleware-adapter.service
      - description: "3. Post-restart HTTP readiness verification loop (max 60s)"
        exec: |
          # Poll local readiness endpoint every 5s (up to 60s) before advancing batch
          for i in $(seq 1 12); do
            STATUS=$(curl -s -o /dev/null -w "\%{http_code}" "${option.health_endpoint}" || echo "000")
            if [ "$STATUS" = "200" ]; then
              echo "[OK] ${node.name} healthy (HTTP 200)"
              exit 0
            fi
            sleep 5
          done
          echo "[CRITICAL] ${node.name} failed readiness check after 60s. Halting rollout."
          exit 1

```

---

## 7. SLO-Driven SRE Alerting & Predictive Capacity Rules (PrometheusRule)

```yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: omnichannel-slo-alerts
  namespace: monitoring
  # Label matched by Prometheus Operator to auto-load alerting rules
  labels:
    release: prometheus-stack
spec:
  groups:
    - name: omnichannel.slo.rules
      interval: 30s
      rules:
        # Latency SLO breach: alert if 95th percentile response time exceeds 500ms for 3m
        - alert: OmnichannelHighP95Latency
          expr: |
            histogram_quantile(0.95,
              sum(rate(http_server_requests_seconds_bucket{namespace="banking-core",app="omnichannel-gateway-adapter"}[5m])) by (le)
            ) > 0.5
          for: 3m
          labels:
            severity: warning
            tier: middleware
          annotations:
            summary: "High p95 latency (> 500ms) on {{ $labels.app }}"
            description: "95th percentile HTTP latency has exceeded 500ms for 3 consecutive minutes (current: {{ $value | humanizeDuration }})."

        # Availability SLO breach: escalate critical alert if HTTP 5xx ratio exceeds 1%
        - alert: OmnichannelElevated5xxErrorRate
          expr: |
            (
              sum(rate(http_server_requests_seconds_count{namespace="banking-core",app="omnichannel-gateway-adapter",status=~"5.."}[5m]))
              /
              sum(rate(http_server_requests_seconds_count{namespace="banking-core",app="omnichannel-gateway-adapter"}[5m]))
            ) > 0.01
          for: 2m
          labels:
            severity: critical
            tier: middleware
          annotations:
            summary: "Critical 5xx error rate (> 1%) on {{ $labels.app }}"
            description: "HTTP 5xx error ratio breached 1% SLO threshold over a 5-minute window."

        # Proactive capacity forecasting: predict disk exhaustion 4h ahead using 1h write rate
        - alert: NodeFilesystemPredictiveExhaustion
          expr: |
            (
              node_filesystem_avail_bytes{mountpoint=~"/|/opt|/var",fstype!="tmpfs"} > 0
              and
              predict_linear(node_filesystem_avail_bytes{mountpoint=~"/|/opt|/var",fstype!="tmpfs"}[1h], 4 * 3600) < 0
            )
          for: 10m
          labels:
            severity: warning
            tier: infrastructure
          annotations:
            summary: "Disk exhaustion predicted within 4h on {{ $labels.instance }} ({{$labels.mountpoint }})"
            description: "Linear trend analysis indicates {{ $labels.mountpoint }} will reach 0 bytes available within 4 hours; trigger Rundeck log-hygiene job."

```
