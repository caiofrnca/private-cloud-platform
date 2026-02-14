# 01 - Hypervisor Design

## Purpose

Define the minimal hypervisor architecture for the private-cloud-platform.

This document describes structure only.
No operational endpoints or sensitive data are included.

---

## Platform

- Hypervisor: Proxmox VE
- Virtualization: KVM (VM) + LXC (containers)
- Deployment: Single-node (Phase 1)
- Availability Model: Standalone

---

## Networking Model

- Single VLAN-aware trunk bridge
- Segmentation enforced by dedicated firewall VM
- No management services exposed to public internet
- Management access restricted to internal network only

---

## Storage Model

- Primary storage: Local NVMe
- Filesystem: ZFS (snapshot-capable)
- Separation between:
  - Hypervisor system
  - Virtual machine storage
  - Backup datasets

---

## Compute Policy

- Host resources reserved to ensure stability
- No uncontrolled CPU or memory overcommit
- Heavy workloads run in full VMs
- Lightweight services may use containers

---

## Security Principles

- Management plane isolated
- Default-deny inter-network policy
- No direct internet exposure of hypervisor
- Configuration changes documented
- Backups scheduled before major changes

---

## Automation Direction

- Infrastructure-as-Code planned
- VM profiles standardized
- Image builds reproducible
- Manual configuration minimized over time

---

## Scope

This document defines architectural intent only.
Implementation details are maintained in separate runbooks or IaC definitions.