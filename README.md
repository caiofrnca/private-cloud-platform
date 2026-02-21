## Private Cloud Platform

> Security-first, AI-ready private cloud infrastructure built on a Minisforum UM790.

### Overview

This repository documents the design, implementation, and evolution of a layered private cloud platform running on a mini-pc (Minisforum UM790).

I wanted a space to get my hands dirty with AI and serious infrastructure, so I'm building a private cloud that's as professional as it is personal.

The objective is to build an enterprise-style, security-first infrastructure using:

- **Proxmox VE** (virtualization layer)
- **pfSense** (secure edge & segmentation)
- Network-as-Code principles
- Documentation-as-Code
- Infrastructure-as-Code (IaC)
- Observability and automation-first mindset

-> This is not a home lab experiment.  
-> This is a reproducible platform engineering project.

-> And YES, an AI does help with the 'aesthetic emoji' formatting here. My human brain is busy making sure the engineering actually works.

Ps: This README will evolve with each phase!

---

### 🎯 Current Status (v0.1) - February 2026

### Phase 1 — Foundation
- Proxmox installed on bare metal (UM790)
- Single VLAN-aware trunk bridge design adopted (vmbr0)
- Baseline host access validated

### Phase 2 — Networking
- pfSense deployed as VM
- Supernet locked: **10.200.0.0/16**
- VLAN gateways implemented (pfSense as default gateway per VLAN)
- Phase 1 access matrix baseline established (default deny east-west; explicit allows)

### Phase 4/5 — Core Services + Applications (early validation win)
- Traefik terminating HTTPS using **Step-CA issued certs**
- OpenClaw published behind Traefik with working **WebSocket upgrade**
  - Browser → HTTPS (Traefik + Step-CA) → OpenClaw UI → WebSocket → Gateway → Device Connected

---

### 🧱 Architecture (Current State)
```text
Internet
 ↓
 ISP Router
 ↓
 Proxmox Host
 ↓
 pfSense (WAN)
 ↓
 pfSense (LAN trunk: VLAN)
 ↓
Proxmox (vmbr0 trunk)
 ↓
VMs / Services (Traefik, Step-CA, OpenClaw, ...)
```
See diagrams in `docs/architecture/diagrams/`.
---

### 📂 Repository Structure overview
```text
private-cloud-platform/
├── 0-foundation/                   # Hardware & Hypervisor
├── 1-networking/                   # Network as Code
├── 2-storage/                      # Software-defined Storage
├── 3-identity/                     # SSO & Directory Services
├── 4-core-services/                # Monitoring, Logging, DNS
├── 5-applications/                 # Media, Home, Productivity
├── 6-ai-ml/                        # MLOps Pipeline
├── 7-observability/                # Telemetry & Service Mesh
├── 8-security/                     # Compliance & Hardening
├── 9-disaster-recovery/            # Business Continuity
|
├── JUSTIFICATION.md        # Why this project exists, design philosophy 
├── README.md               # You are here
└── ROADMAP.md              # Moved up, versioned milestones

```
#### 0-foundation/
- Hardware inventory
- BIOS configuration notes
- Proxmox configuration
- Hypervisor network design

#### 1-networking/
- Subnet definitions
- Firewall zone definitions
- VLAN architecture
- pfSense configuration backups
- Rules-as-code planning

---

### 🔐 Design Principles

- Least privilege by default
- Segmentation before services
- Everything version controlled
- No secrets committed
- Rollback mindset (snapshots/backups before major changes)
- Reproducibility over convenience

---

### 🛣 Roadmap (next)

- [ ] Formalize firewall matrix as code and generate pfSense rules
- [ ] Standardize DNS naming + internal zones
- [ ] WireGuard / VPN VLAN100 with role-based access
- [ ] GitOps bootstrap (Flux) for Traefik + cert issuance + apps
- [ ] Observability baseline (Prometheus/Grafana/Loki) in VLAN80


---

### 🧪 Validation Checklist

#### Foundation
- [x] Proxmox reachable via management IP
- [x] Proxmox trunk networking validated

#### Networking
- [x] pfSense WAN working (ISP DHCP)
- [x] VLAN gateways active (10.200.x.1)
- [x] Baseline isolation policy applied
- [ ] Inter-VLAN matrix fully validated + documented

#### Services (Ingress/PKI/App)
- [x] Step-CA issues certs for internal services
- [x] Traefik serves HTTPS with Step-CA cert
- [x] OpenClaw accessible via HTTPS + WebSockets

---

### 🚀 Evolution

This platform will expand into:

- Storage layer
- Identity & SSO
- Observability stack
- AI/ML workloads
- Disaster recovery automation
- Chaos engineering

---