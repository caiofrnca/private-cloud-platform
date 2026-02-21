## Traefik TLS & Ingress Runbook

### Purpose

Document the configuration, validation, and recovery procedures for:

- Traefik reverse proxy
- TLS termination
- ACME integration with Step-CA
- WebSocket upgrade handling

Traefik is responsible for secure ingress into the platform.

---

### Scope

Covers:

- Traefik startup validation
- TLS certificate validation
- ACME configuration checks
- WebSocket support verification
- Troubleshooting TLS failures

Does **not** include:

- Step-CA recovery (see Step-CA runbook)
- pfSense networking recovery
- Application-layer debugging

---

### Architecture Overview

Traffic flow:
```text
→ Browser
→ HTTPS (Traefik)
→ Step-CA issued certificate
→ Traefik reverse proxy
→ Internal service (e.g., OpenClaw)
```


Traefik performs:

- TLS termination
- ACME certificate request/renewal
- HTTP → HTTPS redirection
- WebSocket forwarding

---

### Configuration Location

Typical deployment paths:

- Docker Compose
- Static configuration file (`traefik.yml`)
- Dynamic configuration files
- Kubernetes CRDs (if applicable)

Common TLS configuration includes:

```yaml
certificatesResolvers:
  stepca:
    acme:
      caServer: https://<stepca-host>/acme/acme/directory
      storage: /data/acme.json
      keyType: RSA4096
```

Validate Traefik Running

If systemd-based:

systemctl status traefik

If Docker-based:

docker ps
docker logs <traefik-container>

Confirm:

No startup errors

ACME resolver initialized

No certificate parsing errors

Validate ACME Connectivity

Test ACME endpoint manually:

curl https://<stepca-host>/acme/acme/directory

If unreachable:

Verify DNS resolution

Verify firewall rules

Verify Step-CA is running

Validate TLS Certificate

From client machine:

openssl s_client -connect <domain>:443

Confirm:

Certificate issuer matches internal CA

Full certificate chain present

No verification errors

Validate Browser Behavior

Check:

No browser TLS warnings

Correct domain in certificate

Automatic HTTPS redirection working

Validate WebSocket Upgrade

For applications like OpenClaw:

Open browser developer tools.

Navigate to Network tab.

Confirm WebSocket connection upgrades successfully.

Ensure no 101 Switching Protocols errors.

If failure:

Verify entryPoints configuration

Verify forwardHeaders

Verify backend service port

Common Failure Modes
Symptom	Likely Cause	Action
TLS certificate invalid	Step-CA unreachable	Validate ACME endpoint
Certificate not renewing	ACME storage corrupted	Restore acme.json
HTTP works, HTTPS fails	TLS entryPoint misconfigured	Verify Traefik static config
WebSocket disconnects	Missing headers or upgrade config	Check router and middleware config
Browser shows unknown CA	Root CA not trusted on client	Install root certificate
Recovery Procedure

If TLS completely broken:

Confirm Step-CA operational.

Restart Traefik:

systemctl restart traefik

Or:

docker restart <traefik-container>

Verify ACME storage file exists:

ls -l /path/to/acme.json

Revalidate certificate issuance.

Confirm application access.

Validation Checklist
Service

 Traefik running

 No startup errors

 ACME resolver initialized

TLS

 Valid certificate served

 Correct issuer (internal CA)

 No browser warnings

Application

 Backend service reachable

 HTTPS working

 WebSocket upgrades successful

Security Notes

Do not commit acme.json to Git.

Restrict access to ACME storage file.

Ensure private keys are stored securely.

Enforce HTTPS-only access.

Backup Reminder

Backup acme.json

Backup static/dynamic configuration files

Backup Docker Compose (if used)

Validate restore periodically

Last Review

Version: v0.1

Reviewed: February 2026

Next Review: After first TLS failure simulation test
