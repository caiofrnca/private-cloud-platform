## Reverse Proxy & Internal PKI

This layer provides secure ingress, TLS termination, and internal certificate authority services for the platform.

It consists of:

- **Traefik** — Reverse proxy & ingress controller
- **Step-CA** — Internal certificate authority (PKI)
- **ACME protocol** — Automated certificate issuance
- TLS termination for internal services (e.g., OpenClaw)

---

### 🎯 Objective

Provide:

- Encrypted service access (HTTPS everywhere)
- Automated certificate lifecycle management
- WebSocket-compatible ingress routing
- Zero self-signed warnings inside the trusted network
- Reproducible certificate issuance model

This is the cryptographic trust backbone of the platform.

---

### 🧱 Architecture

```text
Client (LAN/VPN)
   ↓ HTTPS
Traefik (TLS termination)
   ↓
Service (OpenClaw, future apps)
   
Step-CA
   ↑
ACME Certificate Issuance