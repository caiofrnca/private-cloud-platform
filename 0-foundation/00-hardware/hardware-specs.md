# 00 - Hardware Specifications

## 0.1 Overview

**Platform:** MinisForum UM790 Pro  
**Role:** Private Cloud Core Node  
**Environment:** Home Data Center (Single-Node Phase 1)  
**Operational Mode:** 24/7 Always-On

This document defines the physical compute foundation for the private-cloud-platform.

---

## 0.2 CPU

**Model:** AMD Ryzen 9 7940HS  
**Architecture:** Zen 4 (4nm TSMC)  
**Cores / Threads:** 8C / 16T  
**Max Boost Clock:** 5.2 GHz  
**L3 Cache:** 16 MB  

### Capabilities

- Hardware virtualization (SVM / AMD-V)
- IOMMU support (passthrough ready)
- AES-NI support
- SMT enabled

### Integrated Components

**GPU:** Radeon 780M (RDNA3)  
**AI Engine:** Ryzen AI (XDNA NPU)

---

## 0.3 Memory

**Type:** DDR5 SODIMM  
**Speed:** 5600 MT/s  
**Installed Capacity:** 32 GB  
**Max Supported:** 64 GB  

**Configuration:** Dual Channel

### Allocation Strategy (Phase 1)

| Component | Reserved RAM |
|------------|--------------|
| Hypervisor | 6 GB |
| Core Services | 16 GB |
| Observability / Security | 4–6 GB |
| Free Headroom | 4–6 GB |

---

## 0.4 Storage

**Slots:** 2 × M.2 2280  
**Interface:** PCIe 4.0 x4  

### Disk Layout Strategy

**NVMe #1**
- Hypervisor OS
- Core VM images
- Boot pool

**NVMe #2**
- VM datastore
- Backup storage
- Log aggregation
- Model storage (Phase 2)

### Filesystem Strategy

- ZFS (primary pool)
- Compression: `zstd`
- atime: disabled
- Snapshot-based backups enabled

---

## 0.5 Networking

**Physical NICs:**  
- 1 × 2.5GbE Ethernet

### Design Constraints

- Single uplink topology
- VLAN-aware trunk bridge (vmbr0)
- Router-on-a-stick model
- Segmentation enforced at firewall layer

---

## 0.6 Power & Reliability

- 24/7 runtime target
- UPS integration recommended
- Graceful shutdown automation
- Scheduled backups (nightly)

---

## 0.7 Intended Workload Classes

### Phase 1
- Core infrastructure services
- Firewall VM
- Observability stack
- Internal DNS/DHCP
- Reverse proxy

### Phase 2
- Identity management
- Secrets management
- Central logging
- AI inference services

### Phase 3
- Automation pipelines
- Service mesh experimentation
- Chaos testing
- High-availability simulation

---

## 0.8 Operational Boundaries

This node is designed for:

- Enterprise-style infrastructure simulation
- Production-grade home segmentation
- Security architecture experimentation
- Infrastructure-as-code development

This node is not designed for:

- GPU model training workloads
- Multi-node Kubernetes clusters (Phase 1)
- High-density virtualization farms

---

## 0.9 Status

Deployment Phase: Foundation  
Risk Level: Low  
Scaling Path: Add secondary node for HA testing