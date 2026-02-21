## Step-CA Backup & Restore Runbook

### Purpose

Document the procedure to:

- Back up Step-CA state
- Restore Step-CA after failure
- Validate certificate trust chain
- Prevent platform-wide TLS outage

Step-CA is critical infrastructure.  
Loss of CA state invalidates trust across all services.

---

### Scope

Covers:

- Step-CA data backup
- Step-CA restoration
- Container/VM restart
- Certificate validation
- Trust chain verification

Does **not** include:

- Root CA private key generation
- Full hypervisor recovery (see Proxmox runbook)
- Application-specific TLS debugging

---

### Preconditions

You must have:

- Access to Step-CA host (VM or container)
- Access to Step-CA data directory backup
- Root CA public certificate
- Intermediate CA certificate
- Provisioner configuration reference

---

### Identify Step-CA Data Location

Typical data directory:

```bash
/var/lib/step-ca
```
Or custom directory defined in ca.json.

Confirm location:
Look for:
cat /path/to/ca.json
```
Look for:
```jason
"db": {
  "type": "badger",
  "dataSource": "/var/lib/step-ca/db"
}
```

Backup Procedure

Stop Step-CA before backup (recommended):

systemctl stop step-ca

Or if running in Docker:

docker stop <container_name>

Archive the data directory:

tar -czf stepca-backup-YYYY-MM-DD.tar.gz /var/lib/step-ca

Restart service:

systemctl start step-ca

Store backup securely off-host.

Restore Procedure
Step 1: Stop Step-CA
systemctl stop step-ca

Or:

docker stop <container_name>
Step 2: Restore Data Directory

Remove corrupted data (if necessary):

rm -rf /var/lib/step-ca

Extract backup:

tar -xzf stepca-backup-YYYY-MM-DD.tar.gz -C /

Ensure correct ownership:

chown -R step:step /var/lib/step-ca

(Adjust user/group if different.)

Step 3: Start Step-CA
systemctl start step-ca

Or:

docker start <container_name>
Step 4: Validate Service Status
systemctl status step-ca

Or:

docker logs <container_name>

Confirm no startup errors.

Step 5: Validate ACME Endpoint

Test ACME directory:

curl https://<stepca-host>/acme/acme/directory

Should return JSON directory metadata.

Step 6: Validate Certificate Issuance

Test issuing a certificate (non-production test domain):

step ca certificate test.internal test.crt test.key

Confirm certificate is issued successfully.

Step 7: Validate Trust Chain

From a client machine:

openssl s_client -connect <service-domain>:443

Confirm:

Certificate chain complete

Issuer is your internal CA

No validation errors

Validation Checklist
Service Status

 Step-CA running

 No startup errors

 ACME endpoint reachable

Trust

 Root CA trusted on clients

 Intermediate CA valid

 Certificate issuance functional

Platform

 Traefik certificates valid

 No browser TLS warnings

 Applications accessible

Common Failure Modes
Symptom	Likely Cause	Action
Certificates invalid	Wrong data directory restored	Restore correct backup
ACME failing	Provisioner misconfiguration	Verify ca.json provisioners
Browser TLS warning	Root CA not trusted	Reinstall root certificate
Service not starting	Permissions incorrect	Fix ownership and permissions
Recovery Order (If Full TLS Outage)

Restore Step-CA data

Start Step-CA

Validate ACME directory

Validate certificate issuance

Restart Traefik

Confirm application HTTPS access

Security Note

This runbook intentionally excludes:

Root CA private key

Provisioner private keys

Passwords

Encrypted key material

Sensitive CA material must never be committed to Git.

Backup Policy Reminder

Backup before upgrades

Backup before provisioner changes

Store backups off-host

Test restore procedure periodically

Step-CA backup is mandatory before major infrastructure changes.

Last Review

Version: v0.1

Reviewed: February 2026

Next Review: After first restore validation test