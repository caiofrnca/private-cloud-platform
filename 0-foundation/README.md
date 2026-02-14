# Foundation Layer

## Purpose

The Foundation layer defines the physical and virtualization
baseline of the private-cloud-platform.

It establishes the hardware, hypervisor, storage, and
core infrastructure contracts upon which all higher layers depend.

This layer contains no application workloads.

---

## Scope

The Foundation layer includes:

- Hardware inventory and specifications
- Hypervisor design and configuration contracts
- Storage layout model
- Resource allocation policy
- Baseline operational runbooks

---

## Design Principles

- Stability over feature density
- Segmentation enforced at the network edge
- Default-deny security posture
- No public exposure of management services
- Infrastructure changes documented and controlled

---

## Architecture Model

- Single-node deployment (Phase 1)
- Virtualization-based isolation
- Centralized firewall enforcement
- Snapshot-capable storage

Future phases may introduce multi-node expansion.

---

## Operational Boundary

This layer does not include:

- Application services
- Kubernetes workloads
- AI services
- Public endpoints
- Sensitive operational data

All sensitive configuration artifacts are excluded from version control.

---

## Status

Foundation architecture defined.
Hypervisor operational.
Networking segmented.
Security baseline enforced.

Higher layers build on this foundation.