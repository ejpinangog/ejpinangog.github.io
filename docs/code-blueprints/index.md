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
    apiVersion: [monitoring.coreos.com/v1](https://monitoring.coreos.com/v1)
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