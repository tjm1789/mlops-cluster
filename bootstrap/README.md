# Bootstrap

The Bootstrap layer handles the **"Day 0"** transition from bare metal to a reachable node. This stage establishes the minimum viable environment required for remote automation tools to take control.

Configuration is kept intentionally minimal. Any setting that can be managed via Ansible after the node is reachable should be excluded from this layer.

---

## Objectives

Establishment of a baseline state with the following characteristics:
1. **Connectivity:** Network accessibility via SSH (Public key authentication enabled; password authentication disabled).
2. **ML-Ops Readiness:** `cgroups` enabled to support containerized workloads.
3. **Execution Environment:** Python 3 installed to facilitate remote orchestration.
4. **Consistency:** Standardized user accounts and hostname conventions across the cluster.

---

## Hardware & OS Requirements

ML-Ops workloads require specific host-level configurations that must be applied during the initial imaging or first-boot process (via `cloud-init`, `rpi-imager` customization, or manual configuration):

* **Architecture:** 64-bit OS (required for modern ML libraries like TensorFlow and PyTorch).
* **Kernel Parameters:** The following must be appended to the boot command line (typically `cmdline.txt` or `user-data`):
    ```text
    cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory
    ```
* **Storage/Partitioning:** Custom partitioning for drives should be finalized at this stage.

---

## Inventory Convention

* `inventory.sample.ini`: A template defining the cluster architecture. This file is tracked in version control.
* `inventory.ini`: The local source of truth containing specific node IPs, MAC addresses, and hostnames. This file is git-ignored to protect environment-specific data.

**Group Classifications:**
* `[masters]`: Control plane nodes.
* `[workers]`: Compute nodes for ML inference and training workloads.

---

## Workflow

1. **Imaging/Installation:** Installation of the OS (e.g., Ubuntu 24.04/22.04 LTS) using a headless `cloud-init` configuration or a manual setup with peripherals.
2. **Initial Configuration:** Application of host-level settings, including hostname assignment and drive partitioning.
3. **Network Identification:** Identification of the node IP address via router tables, static assignment, or network scanning.
4. **Inventory Update:** Addition of the node details to `inventory.ini`.
5. **Connectivity Verification:** ```bash
    ssh <user>@<node-ip> "python3 --version"
    ```
6. **Handoff:** Transition to the root directory for cluster-wide provisioning and ML-Ops stack deployment.
