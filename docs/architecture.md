# Architecture

## Purpose

This document describes the architecture of the MLOps cluster defined in this repository.
The goal of the system is to demonstrate reproducible MLOps and platform-engineering practices on modest, heterogeneous hardware, using modern Kubernetes and GitOps workflows.

The emphasis is on clarity, rebuildability, and realistic operational trade-offs in order to enable the widest range of experimentation options.

---

## High-Level Overview

The platform is a small, self-hosted Kubernetes cluster composed of one x86_64 desktop node and two ARM-based Raspberry Pi worker nodes.

- The **desktop node** acts as the primary control-plane and hosts core platform services.
- The **Raspberry Pi nodes** act as worker nodes and are used to run constrained, edge-style workloads.

All infrastructure and workloads are defined declaratively and reconciled via GitOps.
The physical cluster is considered disposable; this Git repository is the source of truth.

---

## Node Roles and Responsibilities

### Desktop PC (x86_64)

Primary responsibilities:
- Kubernetes control-plane (K3s server, etcd)
- GitOps controller
- Observability stack (metrics, logs, dashboards)
- Stateful platform services (e.g. artifact storage, tracking services)
- Cluster coordination and reconciliation

Rationale:
- Significantly more CPU, memory, and storage capacity
- Better suited to stateful and control workloads
- Reduces pressure on constrained ARM nodes

---

### Raspberry Pi Nodes (ARM64)

Primary responsibilities:
- Stateless services
- Model-serving APIs
- Demonstration training jobs (resource-limited)
- Edge-style workloads under constrained conditions

Rationale:
- Explicitly models heterogeneous, resource-constrained environments
- Forces attention to scheduling, resource limits, and multi-architecture images
- Reflects realistic edge / hybrid deployment patterns

---

## Workload Placement Strategy

Workloads are intentionally placed using:
- node labels
- node selectors and/or affinities
- architecture-specific container images

Principles:
- Control-plane and stateful services prefer the desktop node
- Stateless and demonstrative workloads may run on ARM nodes
- Placement decisions are explicit, not accidental

This logic is expressed at the cluster-composition level, not embedded inside reusable workload definitions.

---

## Infrastructure Layers

### Bootstrap Layer

Responsibilities:
- Operating system baseline
- SSH access and security hardening
- K3s installation and configuration
- Initial GitOps bootstrap

Tooling:
- Ansible

This layer is only required to make Kubernetes exist and is not responsible for application lifecycle management.

---

### Platform Infrastructure Layer

Responsibilities:
- Ingress and traffic routing
- Certificate management
- Persistent storage
- Observability (metrics, logs, dashboards)
- Supporting services required by workloads

Characteristics:
- Reusable across environments
- Declarative
- Managed entirely via GitOps once bootstrapped

---

### Application / Workload Layer

Responsibilities:
- ML training jobs
- Model artifact storage and retrieval
- Model-serving APIs
- Demonstration services used to exercise the platform

Characteristics:
- Containerised
- Stateless where possible
- Observable and reproducible
- Explicitly constrained to reflect real-world limits

---

## Configuration and Composition

Cluster-specific configuration lives in the `clustering/` directory.

This layer defines:
- which infrastructure components are enabled
- which workloads are deployed
- environment-specific values such as:
  - hostnames
  - storage classes
  - resource limits
  - node placement rules

Reusable definitions remain environment-agnostic, only configuration varies.

---

## GitOps Model

- Git is the single source of truth
- Desired state is defined declaratively
- Changes are applied by reconciliation, not imperative commands
- Manual intervention is reserved for debugging only

Success is defined as:
> A destroyed cluster can be rebuilt to the same state using code alone.

---

## Security Posture (High Level)

- No plaintext secrets committed to Git
- Secrets are injected via Kubernetes-native mechanisms
- Access is SSH-key based
- Cluster access is restricted to trusted devices on the local network

This is a personal platform; initial security measures are pragmatic rather than compliance-driven.

---

## Non-Goals

- Multi-region or cloud-scale high availability
- Large-scale distributed training
- GPU scheduling
- Enterprise identity and compliance tooling
- Cost optimisation beyond basic resource constraints
