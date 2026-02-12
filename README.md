## Private Cloud Platform

> Security-first, AI-ready private cloud infrastructure built on a Minisforum UM790.

### Overview

This repository documents the design, implementation, and evolution of a layered private cloud platform running on a mini-pc (Minisforum UM790).

I wanted a space to get my hands dirty with AI and serious infrastructure, so I'm building a private cloud that's as professional as it is personal.

The objective is to build an enterprise-style, security-first infrastructure using:

- **Proxmox VE** (virtualization layer)
- **pfSense 2.7.2** (secure edge & segmentation)
- Network-as-Code principles
- Documentation-as-Code
- Infrastructure-as-Code (IaC)
- Observability and automation-first mindset

-> This is not a home lab experiment.  
-> This is a reproducible platform engineering project.

-> And YES, an AI does help with the 'aesthetic emoji' formatting here. My human brain is busy making sure the engineering actually works.

---

### 🎯 Current Status (v0.1) - February 2026

#### ✅ Phase 1 – Foundation Layer
- Proxmox VE 9.1 installed on bare metal
- Hypervisor networking configured
- Initial resource profiles defined and secured
- Baseline connectivity validated

#### ✅ Phase 2 – Networking Layer (In Progress)
- pfSense 2.7.2 deployed as VM
- WAN + LAN interfaces configured
- Initial segmentation introduced
- VLAN structure defined
- Basic firewall rule strategy applied

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
 LAN / VLAN Segmentation
 ↓
 Virtual Machines / Services
```
---

### 📂 Repository Structure
```text
private-cloud-platform/
 ├── 0-foundation/ 
 ├── 1-networking/ 
```
#### 0-foundation/
- Hardware inventory
- BIOS configuration notes
- Proxmox configuration
- Hypervisor network design
- Performance baselines

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
- Recovery procedures documented
- Reproducibility over convenience

---

### 🛣 Roadmap

Next milestones:

- [ ] Complete VLAN isolation model
- [ ] Formalize firewall rule matrix
- [ ] Move Proxmox management into dedicated VLAN
- [ ] Introduce WireGuard secure remote access
- [ ] Document full segmentation validation

---

### 🧪 Validation Checklist

- [x] Proxmox reachable via management IP
- [x] pfSense WAN receives DHCP from ISP
- [x] LAN interface operational
- [ ] Inter-VLAN isolation validated
- [ ] Backup strategy implemented
- [ ] Configuration export stored

---

### 📈 Why This Exists

This project serves as:

- Platform engineering practice
- Network security laboratory
- AI-capable infrastructure base
- Documentation portfolio
- Continuous learning system

---

### 🚀 Evolution

This platform will expand into:

- Storage layer
- Identity & SSO
- Observability stack
- AI/ML workloads
- Disaster recovery automation
- Chaos engineering

This README will evolve with each phase!

---