# MLOps Homelab

This repository defines a personal MLOps homelab built on a Raspberry Pi Kubernetes cluster.
Its purpose is to demonstrate end-to-end MLOps and platform engineering practices: reproducible infrastructure, GitOps-driven deployment, observability, and the full machine-learning lifecycle from training to serving.

This is a learning and reference platform, not a production service with external SLAs.

---

## Architecture Overview

The system consists of a small ARM-based Kubernetes cluster managed as code.
Infrastructure components and workloads are defined declaratively and reconciled via GitOps.

**Key characteristics**
- K3s-based Kubernetes cluster on Raspberry Pi hardware
- Infrastructure provisioned with Ansible
- Intentional workload placement across heterogeneous nodes (control-plane vs edge workers)
- GitOps-driven deployment (single source of truth: Git)
- Persistent storage for stateful ML workloads
- Observability across cluster, services, and ML components
- Reference MLOps workload: training → artifact storage → model serving → monitoring

**Full architecture and design rationale:** see [`docs/architecture.md`](docs/architecture.md)

---

## Repository Structure

```text
.
├── README.md          # High-level overview
├── docs/              # Architecture notes, ADRs, runbooks
├── bootstrap/         # Ansible: OS baseline, K3s install, initial GitOps bootstrap
├── infra/             # Reusable platform components (ingress, storage, monitoring, etc.)
├── apps/              # Workloads (ML services, training jobs, supporting services)
└── clustering/        # Cluster-specific composition and configuration

