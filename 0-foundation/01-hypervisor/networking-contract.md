# 01 - Networking Contract

## Purpose

Define the networking architecture contract for the hypervisor layer.

This document describes structure and design intent only.
No operational endpoints or sensitive data are included.

---

## Design Principles

- Segmentation by default
- Least-privilege communication
- Centralized policy enforcement
- No public exposure of management services
- Reproducible configuration model

---

## Topology Model

- Single physical uplink
- VLAN-aware trunk bridge on hypervisor
- Dedicated firewall VM performs routing and policy enforcement
- All inter-network traffic passes through firewall

---

## Network Segmentation

Logical networks are separated using VLANs.

Each network:

- Has a defined purpose
- Has controlled access rules
- Does not permit unrestricted lateral movement

Inter-network access must be explicitly allowed.

Default stance:
- Deny inter-segment traffic
- Allow only required flows

---

## Management Plane

- Management network logically isolated
- Hypervisor UI restricted to management segment
- No direct external exposure
- Remote access permitted only through secure gateway (e.g., VPN)

---

## Addressing Model

- Private address space only (RFC1918)
- No public IP addressing inside platform
- Address plan defined separately from this contract

This document does not expose subnet values.

---

## Traffic Flow Rules

All traffic categories must follow:

1. Intra-segment traffic allowed by policy
2. Inter-segment traffic requires explicit rule
3. Internet-bound traffic subject to firewall inspection
4. East-west traffic minimized

---

## Security Controls

- Default-deny firewall baseline
- Stateful inspection enabled
- Logging enabled for denied flows
- Administrative access restricted by network location

---

## Change Management

Network changes must:

- Be documented
- Be version-controlled where possible
- Include rollback capability
- Avoid manual undocumented modifications

---

## Scope

This contract defines architectural boundaries only.

Implementation details (VLAN IDs, IP ranges, firewall rules)
are maintained in secure configuration repositories
and are not published in public documentation.
