## Proxmox Hypervisor Recovery Runbook

### Purpose

Document the recovery procedure for the Proxmox VE hypervisor in the event of:

- Disk failure
- Corrupted installation
- Unbootable host
- Full reinstallation scenario
- Accidental configuration loss

This runbook ensures the platform can be restored to operational state.

---

### Scope

Covers:

- Fresh Proxmox installation
- Network restoration
- Storage reattachment
- VM restoration
- Post-recovery validation

Does NOT include:

- Application-level restoration (covered in respective layer runbooks)
- Disaster-recovery multi-site failover

---

### Preconditions

You must have:

- Access to Proxmox installation ISO
- Backup storage location accessible
- VM backups available (`vzdump`)
- Network configuration reference
- pfSense configuration backup

---

### Phase 1 — Reinstall Proxmox

1. Boot from official Proxmox ISO.
2. Perform clean installation on target disk.
3. Set:
   - Hostname
   - Static management IP (temporary acceptable)
   - Correct timezone
4. Complete installation and reboot.

---

### Phase 2 — Restore Host Network Configuration

### Step 1: Validate Interfaces

```bash
ip a
```

Confirm physical NIC naming (important if hardware changed).

### Step 2: Restore Bridge Configuration

Edit:
```bash
/etc/network/interfaces
```

Reapply your trunk configuration (example pattern):
```bash
auto vmbr0
iface vmbr0 inet static
    address <MGMT_IP>
    netmask <NETMASK>
    gateway <GATEWAY>
    bridge_ports <PHYSICAL_NIC>
    bridge_stp off
    bridge_fd 0
    bridge_vlan_aware yes
```
Restart networking:
```bash
systemctl restart networking
```
Validate:
```bash
ip a
bridge vlan show
```
### Phase 3 — Reattach Storage
Check Available Disks
```bash
lsblk
```

If using LVM:
```bash
vgscan
vgchange -ay
```

If using ZFS:
```bash
zpool import
```
If storage was external (NFS/iSCSI), re-add via:

Datacenter → Storage → Add

Confirm storage appears in:
```bash
pvesm status
```

### Phase 4 — Restore Virtual Machines
List Backup Files
```bash
ls /path/to/backup/storage
```
Restore VM
```bash
qmrestore /path/to/backup/vzdump-qemu-<VMID>.vma.zst <NEW_VM_ID>
```
Repeat for:
- pfSense
- Step-CA
- Traefik host
- OpenClaw
- Other infrastructure VMs

### Phase 5 — Restore pfSense First

Order matters.
- Start pfSense VM.
- Access console.

Confirm:
- WAN receives IP
- VLAN interfaces attach properly

Restore pfSense configuration (via WebGUI or console).

Validate:
- Inter-VLAN routing
- Default deny baseline
- WAN access

Only proceed once networking is confirmed stable.

### Phase 6 — Start Core Infrastructure

Start in this order:
- Step-CA
- Traefik
- Monitoring (if deployed)
- Application VMs

Validate:
- TLS certificates valid
- No browser warnings
- OpenClaw reachable
- WebSocket upgrade successful

### Validation Checklist

#### Hypervisor

- [ ] Proxmox Web UI accessible  
- [ ] Correct bridge configuration  
- [ ] Storage mounted  
- [ ] VM list visible  

#### Networking

- [ ] pfSense reachable  
- [ ] VLAN gateways operational  
- [ ] Internet reachable from firewall  

#### Core Services

- [ ] Step-CA running  
- [ ] Traefik serving HTTPS  
- [ ] Certificates trusted  
- [ ] OpenClaw reachable  

---

### Post-Recovery Hardening

- Reconfirm host firewall status  
- Reapply SSH key-based access  
- Validate backup job configuration  
- Verify time synchronization (NTP)  
- Re-enable monitoring alerts  

---

### Backup Strategy Reminder

To avoid full rebuild scenarios:

- Schedule regular `vzdump` backups  
- Backup `/etc/network/interfaces`  
- Backup Proxmox cluster config (if applicable)  
- Store backups off-host  
- Periodically test restore process  

---

### Failure Modes

| Symptom             | Likely Cause                   | Action                         |
|---------------------|--------------------------------|--------------------------------|
| No Web UI           | Network misconfiguration       | Recheck bridge and gateway     |
| VMs missing         | Storage not mounted            | Reattach storage               |
| VLANs not working   | pfSense restore incomplete     | Reassign interfaces            |
| HTTPS broken        | Step-CA not restored           | Restore CA state               |

---

### Recovery Order of Operations (Summary)

1. Proxmox reinstall  
2. Network restore  
3. Storage attach  
4. Restore pfSense  
5. Validate segmentation  
6. Restore core services  
7. Restore applications  

Order deviation may cause cascading failures.

---

### Security Note

This runbook intentionally excludes:

- Passwords  
- Private keys  
- Backup storage credentials  
- CA private material  

Sensitive data must never be stored in Git.

---

### Last Review

- **Version:** v0.1  
- **Reviewed:** February 2026  
- **Next Review:** After first full restore test  