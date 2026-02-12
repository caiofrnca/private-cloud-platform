# JUSTIFICATION

## Why this exists
This repository documents the build of a **security-first private cloud platform** on a Minisforum UM790, engineered with a phased approach and maintained using **documentation-as-code** and **network-as-code** principles.

The goal is not to “run apps at home.” The goal is to design and operate infrastructure with the same discipline used in production environments:
- clear architecture
- controlled change
- repeatable builds
- measurable outcomes
- security and segmentation by default

## Target outcomes
- Hardened virtualization foundation (Proxmox VE)
- Secure edge and segmented internal network (pfSense + VLANs)
- Clear separation of management, lab, and public-facing zones
- Documented runbooks for operational tasks and recovery
- A structured roadmap toward storage, identity, observability, automation, and DR

## Design philosophy
### 1) Security first
Segmentation is established before expanding services. Least privilege and controlled trust boundaries are default.

### 2) Reproducibility over convenience
Decisions, steps, and validation checks are captured so the platform can be rebuilt or audited.

### 3) “Everything as code” mindset
Where possible:
- network definitions are codified (subnets, zones)
- configs are backed up and versioned (scrubbed)
- automation is preferred over manual repetition

### 4) Professional documentation
This repo is written as if it were internal engineering documentation:
- architecture decisions are explicit
- changes are tracked
- troubleshooting is documented

## Current scope (v0.1)
Active layers:
- `0-foundation/` (hardware + Proxmox)
- `1-networking/` (pfSense + segmentation)
