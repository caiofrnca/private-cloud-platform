# ADR-0011: ACME Strategy for Internal TLS Automation

## Status
Accepted — February 2026

---

## Context

The platform requires automated TLS certificate issuance for internal services
(e.g., OpenClaw and future applications) exposed via Traefik.

Requirements:

- Encrypted internal traffic (HTTPS everywhere)
- No self-signed certificate warnings
- Automated certificate lifecycle (renewal without manual intervention)
- GitOps compatibility (future Flux adoption)
- No external CA dependency for internal services
- Trust model aligned with segmented VLAN architecture (10.200.0.0/16)

The platform already operates:

- Step-CA as internal Certificate Authority
- Traefik as reverse proxy / ingress
- Segmented VLAN model with security-first design

A decision was required on how ACME should be implemented.

---

## Decision

The platform will use:

**Step-CA as the internal ACME server**,  
with certificates issued to services via ACME-compatible clients.

Current phase:
- Traefik requests certificates from Step-CA using ACME.

Target phase (GitOps-ready model):
- cert-manager will request certificates from Step-CA via ACME issuer.
- Certificates will be provisioned as Kubernetes secrets.
- Ingress objects will reference these secrets.

This keeps ACME fully internal and automation-friendly.

---

## Architecture Model
```text
Client (LAN / VPN)
↓ HTTPS
Traefik (Ingress)
↓
Service (OpenClaw, future apps)

Step-CA (ACME Server)
↑
ACME Challenge + Certificate Issuance
```

## Rationale

### Why ACME?

- Standardized protocol
- Automates issuance and renewal
- Supported by Traefik and cert-manager
- Eliminates manual certificate management

---

### Why Step-CA as ACME server?

- Internal trust domain
- No dependency on public CAs
- Works without public DNS
- Suitable for segmented private networks
- Supports short-lived certificates
- Supports provisioner-based access control

---

### Why not Let's Encrypt?

- Internal services are not publicly exposed
- No need for external trust
- No dependency on public DNS validation
- Avoids rate limits and internet dependency

---

### Why not manual certificate issuance?

- Not scalable
- Not reproducible
- Incompatible with GitOps automation goals
- High operational risk (expiry outages)

---

## Security Implications

- Root CA must remain protected (preferably offline)
- Intermediate CA used for online signing
- ACME provisioner credentials must be secured
- ACME endpoint accessible only within trusted VLAN(s)
- No private keys stored in Git
- Certificate lifetime policy should be enforced (short validity preferred)

Compromise of Step-CA compromises platform trust.

---

## Operational Implications

- Renewal handled automatically by ACME client
- No manual certificate rotation required
- Certificate expiry monitoring should be added to observability layer
- Backup and restore of Step-CA state is mandatory

---

## Consequences

Positive:

- Automated TLS lifecycle
- Clean GitOps migration path
- Standardized certificate issuance
- Reduced human error
- Reproducible infrastructure

Trade-offs:

- Step-CA becomes critical infrastructure
- Requires proper backup strategy
- Requires trust distribution to client devices

---

## Future Evolution

- Introduce cert-manager + ClusterIssuer (Step-CA)
- Implement mTLS for sensitive services
- Enforce TLS 1.2+ minimum
- Implement certificate expiry alerting
- Shorten certificate validity period

---

## Related Documents

- ADR-0008: Traefik Ingress Strategy
- ADR-0009: Step-CA PKI Model
- Runbook: traefik-stepca-tls.md
- Runbook: openclaw-end-to-end.md