# 1 - Networking Layer

## Purpose

Define the networking architecture for the private-cloud-platform.

This layer provides segmentation, traffic control, and secure routing
between internal services and external networks.

No operational endpoints or sensitive addressing details are exposed in this document.

---

## Design Principles

- Segmentation by default
- Least-privilege communication
- Centralized routing and policy enforcement
- No public exposure of management services
- Reproducible configuration model

---

## Architecture Overview

- Single physical uplink
- VLAN-aware trunk bridge on hypervisor
- Dedicated firewall VM performs:
  - Inter-network routing
  - Policy enforcement
  - Internet gateway control

All inter-segment traffic passes through the firewall.

---

## Segmentation Model

Logical networks are separated by purpose.

Each segment:

- Has a defined operational role
- Is restricted by explicit firewall policy
- Does not allow unrestricted lateral movement

Default stance:
- Deny inter-segment traffic
- Allow only required flows

---

## Internet Access Model

- Outbound access controlled per segment
- Inbound access denied unless explicitly required
- No direct exposure of management interfaces

---

## Management Plane

- Management traffic isolated
- Administrative access limited by network location
- Remote access permitted only through secure gateway

---

## Change Management

Network changes must:

- Be documented
- Follow defined security principles
- Include rollback capability
- Avoid undocumented manual modifications

---

## Status

Segmentation active.
Firewall enforced.
Management isolated.
No public exposure.