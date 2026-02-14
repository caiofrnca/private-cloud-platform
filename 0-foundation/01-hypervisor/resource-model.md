## Resource Domains

### 1. Host Domain

Reserved resources for:

- Hypervisor processes
- Storage subsystem
- Networking stack
- Logging and monitoring agents

Host resources are never fully allocated to guest workloads.

---

### 2. Core Services Domain

Workloads classified as infrastructure-critical:

- Firewall
- Identity services
- DNS / DHCP
- Observability stack

These services receive guaranteed allocations.

Ballooning and aggressive overcommit are not permitted.

---

### 3. Application Domain

General-purpose workloads:

- Internal services
- Automation agents
- Development systems

May use controlled overcommit within defined limits.

---

### 4. High-Compute Domain

Performance-sensitive workloads:

- AI inference
- Data processing
- Benchmarking tasks

Run in full virtual machines.
Containers are not used for heavy compute workloads.

---

## Allocation Policy

- CPU overcommit kept conservative
- Memory ballooning disabled for critical workloads
- Storage I/O isolation maintained where possible
- No uncontrolled dynamic scaling

All allocations must be intentional and documented.

---

## Performance Controls

- VirtIO drivers required for optimal performance
- Snapshot before major resource changes
- Benchmarking performed before expanding workload density

---

## Growth Strategy

- Single-node capacity in Phase 1
- Horizontal expansion preferred over aggressive vertical overcommit
- Future multi-node design anticipated

---

## Operational Boundaries

This model does not publish:

- Total memory capacity
- CPU core counts
- Storage size
- Real-time utilization metrics

These values are tracked internally.

---

## Scope

This document defines policy only.

Actual provisioning values are managed through:
- Infrastructure-as-Code
- Internal configuration records
- Secure operational documentation