# devops-k3s-assessment
# Infrastructure DevOps Intern Assessment – Hetzner

This repository contains the implementation steps, commands, and explanations
for the Infrastructure Owner assessment.

## Environment
- Provider: Hetzner Cloud
- OS: Ubuntu 24.04
- Kubernetes: k3s (single-node)
- Container Runtime: Docker
- Deployment Tool: Helm

## High-Level Overview
- Installed Docker and k3s on a single VM
- Deployed Open WebUI using Helm
- Configured OIDC authentication
- Diagnosed and explained intentional OIDC failure
- Answered production ownership questions

## Validation Outputs

### Nodes
# Infrastructure DevOps Intern Assessment – Hetzner

This repository documents the end-to-end setup, deployment, debugging,
and operational reasoning for the Infrastructure Owner assessment.

The focus is on **execution, debugging ability, security thinking, cost awareness,
and ownership of production infrastructure**.

---

## 🧱 Environment

- Cloud Provider: Hetzner
- OS: Ubuntu 24.04
- Kubernetes: k3s (single-node)
- Container Runtime: Docker
- Package Manager: Helm
- Application: Open WebUI

---

## 📌 High-Level Architecture

- Single VM running Docker + k3s
- Open WebUI deployed via Helm
- Redis, Pipelines, Ollama as supporting components
- OIDC enabled intentionally to demonstrate failure diagnosis

---

## 🚀 Implementation Walkthrough (with Evidence)

### 1️⃣ Secure VM Access
- SSH access using provided private key
- Verified OS, disk, and memory availability

📸 Screenshot:
- `screenshots/01-ssh-login.png`

---

### 2️⃣ Docker Installation & Validation
- Installed Docker Engine
- Verified daemon status and client/server versions

📸 Screenshot:
- `screenshots/02-docker-version.png`

---

### 3️⃣ Kubernetes Setup (k3s)
- Installed single-node k3s
- Configured kubectl access
- Verified node and system pods

📸 Screenshots:
- `screenshots/03-k3s-nodes.png`
- `screenshots/04-k3s-pods.png`

---

### 4️⃣ Helm & Application Deployment
- Installed Helm v3
- Deployed Open WebUI using official Helm chart
- Service type configured as ClusterIP

📸 Screenshot:
- `screenshots/06-openwebui-running.png`

---

### 5️⃣ OIDC Configuration (Intentional Failure)
- Enabled OIDC using `values-oidc.yaml`
- Placeholder issuer used as part of assessment

📸 Screenshot:
- `screenshots/07-oidc-config.png`

---

## 🧪 Debugging the Intentional Failure

### Observed Behavior
The application starts successfully, but OIDC authentication fails at runtime.

### Root Cause
The OIDC provider uses a self-signed TLS certificate that is not trusted
by the container’s default CA bundle.

### Diagnosis
- Verified pod health
- Inspected Open WebUI logs
- Identified runtime OIDC validation behavior

📸 Screenshot:
- `screenshots/08-openwebui-logs.png`

### Fix (Production-Grade)
- Trust custom CA via Kubernetes ConfigMap
- Mount certificate into pod
- Update container trust store

This preserves TLS security and avoids insecure bypasses.

---

## 📊 Validation Outputs

```bash
kubectl get nodes
kubectl get all -n openwebui
