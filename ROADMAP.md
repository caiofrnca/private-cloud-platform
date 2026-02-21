# ROADMAP

This roadmap tracks the phased evolution of `private-cloud-platform/`.

Each milestone represents a maturity boundary and is suitable for public write-ups, architectural retrospectives, or LinkedIn technical posts.

---

## v0.1 — Foundation + Edge Firewall ✅

**Objective:** Establish virtualization baseline and secure edge routing.

### Delivered

- [x] Proxmox VE installed on UM790  
- [x] VLAN-aware trunk bridge (`vmbr0`) implemented  
- [x] pfSense 2.7.2 deployed as edge firewall  
- [x] WAN (DHCP) + LAN operational  
- [x] Static supernet defined (`10.X.X.X/16`)  

### Exit Criteria Met

- Hypervisor stable  
- Firewall routing functional  
- Network topology documented  

---

## v0.2 — VLAN Segmentation + Trust Model ✅

**Objective:** Move from flat LAN to structured zero-trust segmentation.

### Delivered

- [x] VLAN architecture implemented:
- [x] Inter-VLAN access matrix (default deny east-west)  
- [x] Role-based allow rules  
- [x] Isolation validation tests  

### Exit Criteria Met

- East-west traffic controlled  
- Management plane isolated  
- DMZ restricted  
- IoT internet-only  

---

## v0.3 — Internal PKI + Automated TLS ✅

**Objective:** Implement production-style certificate lifecycle.

### Delivered

- [x] Offline Root CA deployed  
- [x] Step-CA issuing authority  
- [x] ACME integration configured  
- [x] Automated certificate issuance  
- [x] Certificate trust chain validated  

### Exit Criteria Met

- No self-signed service certificates  
- Automated renewal working  
- Trust hierarchy documented  

---

## v0.4 — Reverse Proxy + Application Exposure ✅

**Objective:** Standardize service ingress and TLS termination.

### Delivered

- [x] Traefik deployed  
- [x] ACME integrated with Step-CA  
- [x] Secure routing to services  
- [x] OpenClaw application deployed  

### Exit Criteria Met

- Centralized ingress model  
- TLS termination standardized  
- First production-style workload live  

---

# 🔄 CURRENT PHASE — v0.5 (In Progress)

## Observability + Platform Maturity

**Objective:** Make the platform measurable, monitorable, and operationally mature.

### Planned Deliverables

- [ ] Prometheus deployment  
- [ ] Grafana dashboards  
- [ ] Centralized logging pipeline (Loki / Vector / Elastic)  
- [ ] Alert definitions  
- [ ] Healthcheck automation  
- [ ] Service dependency validation tests  

### Exit Criteria

- Platform metrics visible  
- Logs centralized  
- Alerts actionable  
- Failure detection automated  

---

# 🔜 Upcoming Milestones

---

## v0.6 — Identity & Secrets

- [ ] Authentik SSO  
- [ ] Directory integration  
- [ ] Vault policies  
- [ ] Secret lifecycle governance  

---

## v0.7 — Storage Hardening & Backup Validation

- [ ] Restic backup automation  
- [ ] Restore testing  
- [ ] Retention validation  
- [ ] DR runbooks validated  

---

## v0.8 — Security Engineering Automation

- [ ] CIS baseline hardening  
- [ ] Container vulnerability scanning  
- [ ] Runtime audit (Falco / auditd)  
- [ ] Incident response playbooks  

---

## v0.9 — AI/ML Infrastructure

- [ ] GPU passthrough  
- [ ] Model serving  
- [ ] Vector database  
- [ ] MLOps pipeline integration  

---

## v1.0 — Resilience Validation

- [ ] Backup restore drills  
- [ ] Failover simulation  
- [ ] Chaos engineering experiments  
- [ ] Full documentation freeze  

---

# Maturity Overview

| Layer | Status |
|--------|---------|
| Foundation | ✅ Complete |
| Segmentation | ✅ Complete |
| PKI | ✅ Complete |
| Reverse Proxy | ✅ Complete |
| Application Layer | 🟡 Initial |
| Observability | 🔄 In Progress |
| Identity | 🔜 Planned |
| Security Automation | 🔜 Planned |
| DR Validation | 🔜 Planned |

---

## Current Position

- Networking complete  
- Firewall architecture complete  
- PKI automated  
- Ingress standardized  
- First application deployed  

**Transitioned from lab setup to platform engineering phase.**


<!-- =========================================================
 private-cloud-platform — Milestone Badges (README snippet)
 Paste this near the top of README.md
========================================================== -->

## Milestones

![v0.1 Foundation + Edge](https://img.shields.io/badge/v0.1-Foundation%20%2B%20Edge-2ea44f?style=for-the-badge)
![v0.2 VLAN Segmentation](https://img.shields.io/badge/v0.2-VLAN%20Segmentation%20%2B%20Policy-2ea44f?style=for-the-badge)
![v0.3 Internal PKI](https://img.shields.io/badge/v0.3-Root%20CA%20%2B%20Step--CA%20%2B%20ACME-2ea44f?style=for-the-badge)
![v0.4 Reverse Proxy + App](https://img.shields.io/badge/v0.4-Traefik%20%2B%20ACME%20%2B%20OpenClaw-2ea44f?style=for-the-badge)

![v0.5 Observability](https://img.shields.io/badge/v0.5-Observability%20(Stack)%20IN%20PROGRESS-f1c232?style=for-the-badge)
![v0.6 Identity + Secrets](https://img.shields.io/badge/v0.6-Identity%20%2B%20Secrets-9e9e9e?style=for-the-badge)
![v0.7 Storage + Backup Validation](https://img.shields.io/badge/v0.7-Storage%20%2B%20Backup%20Validation-9e9e9e?style=for-the-badge)
![v0.8 Security Automation](https://img.shields.io/badge/v0.8-Security%20Engineering-9e9e9e?style=for-the-badge)
![v0.9 AI%2FML Layer](https://img.shields.io/badge/v0.9-AI%2FML%20Layer-9e9e9e?style=for-the-badge)
![v1.0 Resilience](https://img.shields.io/badge/v1.0-DR%20%2B%20Chaos%20%2B%20Resilience-9e9e9e?style=for-the-badge)

---

### Legend

- ✅ **Green** = complete
- 🟡 **Yellow** = in progress
- ⚪ **Grey** = planned

---

### Optional: Progress Table (compact)

| Version | Milestone | Status |
|---|---|---|
| v0.1 | Foundation + Edge (Proxmox + pfSense + trunk bridge) | ✅ Complete |
| v0.2 | VLAN Segmentation + Policy Baseline | ✅ Complete |
| v0.3 | Internal PKI (Root-CA + Step-CA + ACME) | ✅ Complete |
| v0.4 | Reverse Proxy + First App (Traefik + OpenClaw) | ✅ Complete |
| v0.5 | Observability (metrics/logs/alerts) | 🟡 In progress |
| v0.6 | Identity + Secrets (SSO + Vault) | ⚪ Planned |
| v0.7 | Storage + Backup Validation (restic + restore tests) | ⚪ Planned |
| v0.8 | Security Engineering Automation (scan/audit/IR) | ⚪ Planned |
| v0.9 | AI/ML Layer (model serving + vector DB) | ⚪ Planned |
| v1.0 | Resilience (DR + chaos engineering) | ⚪ Planned |