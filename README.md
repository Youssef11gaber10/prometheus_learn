# Prometheus Learning on Kubernetes
This repository documents a hands-on learning journey for Prometheus using multiple deployment styles:
- Manual installation (without Kubernetes)
- Raw Kubernetes YAML manifests
- Prometheus Operator (CRDs + Custom Resources)
- Helm chart customization (`kube-prometheus-stack`)

The goal is to understand the same monitoring stack from different angles, from low-level manual setup to production-style operator and Helm workflows.

## What I have done in this project
- Installed and configured Prometheus manually.
- Wrote manual alert rules and Alertmanager routing rules.
- Deployed Prometheus, Alertmanager, and Node Exporter using plain Kubernetes YAML.
- Installed operators, created secrets, and wrote Custom Resources (CRs) for operator-managed deployments.
- Customized a Helm chart values file for `kube-prometheus-stack`.

## Folder structure and purpose
- `opt/manual-prometheus/`
  - Manual Prometheus + Alertmanager configuration files.
  - Includes local Prometheus scrape config and alert rules.

- `prometheus/`
  - Plain Kubernetes resources for Prometheus core deployment:
    - ConfigMap (Prometheus config + rules)
    - StatefulSet
    - Headless Service

- `alert-manger/`
  - Plain Kubernetes resources for Alertmanager:
    - ConfigMap
    - Deployment
    - Service
    - Secret for email app password

- `node-exporter/`
  - Node Exporter DaemonSet and Service manifests.
  - Covers host metrics collection from cluster nodes.

- `prometheus-operator/`
  - Prometheus Operator Custom Resources and RBAC:
    - Prometheus CR
    - Alertmanager CR
    - PrometheusRule CR
    - ServiceMonitor CR
    - ServiceAccount/ClusterRole/ClusterRoleBinding

- `kube-prometheus-stack-chart/`
  - Helm values override file used to customize chart behavior.

- `maria-db-operator/`
  - MariaDB operator practice files (secret + custom resource).

- `cloudnative-pg-operator/`
  - CloudNativePG operator practice files (secret + custom resource).

## File mapping by approach

### 1) Manual way (without Kubernetes)
These files represent the manual installation and configuration path:
- `opt/manual-prometheus/prometheus.yml`
- `opt/manual-prometheus/first_rule_to_fire_alert.yml`
- `opt/manual-prometheus/second_rule_to_fire_alert.yml`
- `opt/manual-prometheus/alertmanager-0.28.1.linux-amd64/alertmanager.yml`

What this section demonstrates:
- Static scrape targets
- Rule-based alerting
- Alertmanager routing to different receivers (email/slack)

### 2) Kubernetes YAML way (without operator)
These files represent direct Kubernetes resources that you wrote by hand:

Prometheus:
- `prometheus/prometheus-configmap.yml`
- `prometheus/prometheus-statefulset.yml`
- `prometheus/prometheus-headless-service.yml`

Alertmanager:
- `alert-manger/alertmanger-configmap.yml`
- `alert-manger/alertmanger-deployment.yml`
- `alert-manger/alertmanger-svc.yml`
- `alert-manger/gmail-secret.yml`

Node Exporter:
- `node-exporter/node-exporter-daemonset.yml`
- `node-exporter/node-exporter-svc.yml`

What this section demonstrates:
- Full control over workload specs (resources, mounts, args, services)
- Service discovery setup for node metrics
- End-to-end alert pipeline using native Kubernetes objects

### 3) Operator way (CRDs + Custom Resources)
These files represent operator-managed setup using Custom Resources:
- `prometheus-operator/prometheus-CR.yml`
- `prometheus-operator/alertmanger-CR.yml`
- `prometheus-operator/alertmanger-secret.yml`
- `prometheus-operator/prometheus-rules-CR.yml`
- `prometheus-operator/service-monitor-CR.yml`
- `prometheus-operator/rbac-sa.yml`

What this section demonstrates:
- Moving from low-level manifests to higher-level declarative monitoring resources
- Prometheus and Alertmanager lifecycle managed by operator controllers
- Dynamic target discovery through `ServiceMonitor`
- Rule management through `PrometheusRule`

### 4) Helm chart way
This file represents the Helm-based approach:
- `kube-prometheus-stack-chart/values.yml`

What this section demonstrates:
- Chart-level customization with a values override file
- Alert rules and Alertmanager route/receiver configuration through Helm values
- Reproducible installation with fewer raw manifests

### 5) Additional operator practice (database operators)
These files show extra operator learning outside Prometheus stack:
- `maria-db-operator/maria-db-secret.yml`
- `maria-db-operator/maria-db-crd.yml`
- `cloudnative-pg-operator/posgres-db-secret.yml`
- `cloudnative-pg-operator/postgres-db-crd.yml`

Note: filenames include `crd`, but these files are Custom Resource instances consumed by already-installed operators.

## Suggested study order in this repository
1. Start with `opt/manual-prometheus/` to understand core Prometheus and Alertmanager concepts.
2. Move to `prometheus/`, `alert-manger/`, and `node-exporter/` for raw Kubernetes deployment practice.
3. Continue to `prometheus-operator/` to learn CRDs/CRs and operator-managed workflows.
4. Finish with `kube-prometheus-stack-chart/values.yml` to understand Helm-based customization.
5. Optionally review database operator folders for broader operator experience.

## Important notes
- Replace placeholder secrets (`app_password`, webhook URLs, test passwords) before real deployment.
- Most Kubernetes manifests target namespace `prometheus-ns`; create it first if missing.
- Some filenames use the spelling `alertmanger` (without second "a"), keep that in mind when applying files.
