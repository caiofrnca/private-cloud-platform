# 01 - Storage Layout

## Purpose

Define the storage architecture principles for the hypervisor layer.

This document describes structural intent only.
No disk sizes, device identifiers, or operational metrics are exposed.

---

## Design Principles

- Separation of concerns
- Snapshot capability by default
- Predictable I/O behavior
- Clear isolation between system and workloads
- Backup-aware layout

---

## Storage Domains

### 1. System Domain

Reserved for:

- Hypervisor operating system
- Core platform components
- Base system services

This domain is isolated from workload storage.

---

### 2. Workload Domain

Used for:

- Virtual machine disks
- Container storage
- Templates and base images

This domain is snapshot-capable and optimized for VM workloads.

---

### 3. Backup Domain

Dedicated dataset for:

- Scheduled VM backups
- Pre-change snapshots
- Configuration archives

Backups are logically separated from active workload storage.

---

## Filesystem Strategy

- Snapshot-capable filesystem required
- Compression enabled
- Access-time updates disabled
- Consistent mount configuration

Filesystem configuration is maintained internally.

---

## Snapshot Policy

Snapshots must be taken:

- Before major upgrades
- Before infrastructure changes
- Before firewall or networking modifications

Snapshots are not a substitute for backups.

---

## Backup Policy (Baseline)

- Scheduled backups required
- Retention policy defined separately
- Restore testing performed periodically
- Off-platform replication planned in later phase

---

## Performance Considerations

- Workloads separated to prevent I/O contention
- No uncontrolled disk overcommit
- Storage growth managed intentionally

---

## Security Controls

- No storage services exposed externally
- Backup artifacts excluded from public repositories
- Sensitive configuration files never stored in public version control

---

## Scope

This document defines structural storage intent only.

Operational details such as:
- Disk capacity
- Pool names
- Device paths
- Retention schedules

are maintained in secure configuration records.
