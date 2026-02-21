# Firewall Baseline - Phase 1

## Purpose

Define the minimum firewall posture required for a secure
single-node segmented environment.

This document describes policy intent only.
No rule IDs, IP ranges, or operational endpoints are exposed.

---

## Security Principles

- Default-deny between network segments
- Explicit allow rules only
- No unrestricted lateral movement
- Centralized routing and inspection
- No public exposure of management interfaces

---

## Baseline Policy Model

### 1. Intra-Segment Traffic

- Allowed by policy
- Subject to future refinement if required

---

### 2. Inter-Segment Traffic

- Denied by default
- Explicitly allowed only where required
- Logged when denied

---

### 3. Internet Access

- Outbound access allowed per segment purpose
- Inbound access denied unless explicitly published
- No management services exposed externally

---

### 4. Management Plane

- Restricted to designated management network
- Administrative access limited by network location
- No internet-facing management endpoints

---

## Logging & Monitoring

- Denied inter-segment traffic logged
- Critical rule hits logged
- Periodic review required

---

## Change Control

Any firewall modification must:

- Have a defined business or technical justification
- Be documented before implementation
- Be tested in a controlled manner
- Include rollback capability

---

## Phase 1 Posture

- Segmentation active
- Management isolated
- East-west traffic minimized
- No external exposure

---

## Scope

This document defines policy intent only.

Detailed firewall rules and configuration exports
are maintained separately and are not published.
