## pfSense Backup & Restore Runbook

### Purpose

Document the procedure to:

- Back up pfSense configuration
- Restore pfSense after failure
- Reassign interfaces if hardware changes
- Validate VLAN and firewall baseline after restore

This ensures network segmentation and routing can be recovered safely.

---

### Scope

Covers:

- Configuration export
- Configuration restore
- Interface reassignment
- VLAN validation
- Firewall rule validation

Does **not** include:

- Full hypervisor recovery (see Proxmox recovery runbook)
- Application-layer restoration

---

### Preconditions

You must have:

- Admin access to pfSense WebGUI or console
- Latest configuration backup file (`config.xml`)
- Knowledge of interface mappings (WAN/LAN/VLANs)
- Access to Proxmox host if VM-based

---

### Backup Procedure

### Step 1: Export Configuration (WebGUI)

1. Navigate to:
* Diagnostics → Backup & Restore

2. Select:
- Backup area: **All**
3. Click **Download configuration**.

Store the file securely.

---

### Step 2: Store Backup Securely

- Store off-host
- Store versioned copy (date-based naming)
- Do **not** commit unredacted secrets to Git

#### Recommended naming:

* config-YYYY-MM-DD.xml


---

### Restore Procedure

### Step 1: Access pfSense

If VM-based:

- Start pfSense VM from Proxmox
- Access via WebGUI or console

If fresh installation:

- Install same pfSense version
- Assign temporary WAN/LAN
- Access WebGUI

---

### Step 2: Restore Configuration

Navigate to:

* Diagnostics → Backup & Restore


Upload saved `config.xml`.

Click **Restore Configuration**.

System will reboot automatically.

---

### Step 3: Reassign Interfaces (If Required)

If NIC order changed:

1. Access console.
2. Choose:
* Assign Interfaces
3. Reassign:
- WAN
- LAN
- VLAN trunk interface

Confirm correct interface names (e.g., `vtnet0`, `vtnet1`).

---

### Step 4: Validate VLAN Interfaces

Navigate to:
* Interfaces → Assignments → VLANs


Confirm:

- VLAN IDs correct
- Parent interface correct
- IP gateways correct (10.200.x.1 model)

---

### Step 5: Validate Firewall Baseline

Navigate to:
* Firewall → Rules


Confirm:

- Default deny east-west
- Explicit allow rules present
- Rule order intact

---

### Validation Checklist

#### Core Connectivity

- [ ] WAN receives IP
- [ ] LAN gateway reachable
- [ ] VLAN gateways operational
- [ ] Internet reachable from firewall

#### Segmentation

- [ ] Inter-VLAN restrictions enforced
- [ ] Default deny confirmed
- [ ] Explicit allow matrix intact

#### Services

- [ ] Step-CA reachable
- [ ] Traefik reachable
- [ ] Applications accessible

---

### Common Failure Modes

| Symptom                 | Likely Cause                    | Action                            |
|-------------------------|---------------------------------|-----------------------------------|
| No WAN IP               | Incorrect interface mapping     | Reassign WAN                      |
| VLAN not reachable      | Wrong parent interface          | Reconfigure VLAN trunk            |
| Rules missing           | Restored wrong config file      | Verify backup version             |
| Cannot access WebGUI    | Wrong LAN interface assignment  | Reassign LAN via console          |

---

### Recovery Order (If Full Network Outage)

1. Restore pfSense VM
2. Reassign interfaces
3. Validate WAN
4. Validate LAN gateway
5. Validate VLAN gateways
6. Confirm firewall baseline
7. Restore dependent services

---

### Security Note

This runbook intentionally excludes:

- Administrative passwords
- VPN private keys
- API keys
- Sensitive secrets inside `config.xml`

Backups must be stored securely.

---

### Backup Policy Reminder

- Backup before major changes
- Backup before upgrades
- Maintain dated versions
- Store off-host
- Periodically test restore process

---

### Last Review

- **Version:** v0.1  
- **Reviewed:** February 2026  
- **Next Review:** After first live restore test  