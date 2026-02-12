# ROADMAP

This roadmap tracks the phased build of `private-cloud-platform/`.
Milestones are versioned to support clear progress and future LinkedIn write-ups.

---

## v0.1 — Foundation + Secure Edge (Current)
**Objective:** Establish a hardened hypervisor baseline and deploy pfSense as the edge firewall.

**Deliverables**
- [x] Proxmox VE installed on UM790
- [x] pfSense 2.7.2 installed as VM
- [ ] Document Proxmox network bridges and interface mapping
- [ ] Export/scrub pfSense baseline configuration (store under `1-networking/`)
- [ ] Define segmentation plan (VLANs, subnets, zones)

**Exit criteria**
- Proxmox management access is stable and documented
- pfSense WAN/LAN operational
- Baseline backups and “how to restore” runbook exists

---

## v0.2 — VLAN Segmentation + Policy Baseline (Next)
**Objective:** Move from flat LAN to segmented zones with least-privilege rules.

**Deliverables**
- [ ] Create VLANs (MGMT / INTERNAL / DMZ / optional GUEST)
- [ ] Assign interfaces + gateways in pfSense
- [ ] Enable DHCP/DNS per VLAN where needed
- [ ] Implement firewall rule matrix (default deny between zones)
- [ ] Validation tests (reachability + isolation proof)

**Exit criteria**
- VLAN routing works where intended
- Isolation is enforced (e.g., LAB cannot reach MGMT)
- Rule intent is documented and reproducible

---

## v0.3 — Core Services Baseline
**Objective:** Add foundational services that make the platform usable and safe.

Candidate deliverables (scope may adjust)
- [ ] Proxmox Backup Server (or equivalent) + retention policy
- [ ] Internal DNS strategy documented
- [ ] Secure remote access (WireGuard or Tailscale)
- [ ] Reverse proxy baseline (Traefik/Caddy/Nginx) for internal services

---

## v0.4+ — Planned phases (High level)
These phases exist as direction and will be refined with ADRs.

- Storage (2-storage/)
- Identity & SSO (3-identity/)
- Monitoring/logging foundations (4-core-services/)
- Applications portfolio (5-applications/)
- AI/ML layer (6-ai-ml/)
- Observability maturity (7-observability/)
- Security hardening + scanning (8-security/)
- Disaster recovery + chaos engineering (9-disaster-recovery/)

