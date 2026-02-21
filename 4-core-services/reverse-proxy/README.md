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
```

### 🔐 Components
#### Step-CA (Internal PKI)
Role:
- Root / Intermediate CA
- Issues certificates for internal services
- Acts as ACME server

Responsibilities:
- Maintain certificate trust chain
- Manage provisioners (ACME / JWK)
- Enforce certificate lifetime policy

⚠ Private keys are never stored in this repository.

### ACME Integration
Used for automated certificate issuance.
Possible models:
- cert-manager (Kubernetes-native)
- Traefik ACME resolver
- Manual issuance via step ca certificate

### Traefik
Role:
- TLS termination
- Reverse proxy
- WebSocket support
- Routing to internal services

Responsibilities:
- Redirect HTTP → HTTPS
- Attach correct certificate
- Enforce entrypoints (websecure)
- Apply middleware (headers, rate limiting)

### ✅ Current Working Implementation

- HTTPS served via Traefik
- Certificates issued by Step-CA
- OpenClaw reachable via:

Browser → HTTPS (Traefik + Step-CA cert)
→ OpenClaw UI
→ WebSocket upgrade
→ Gateway connected

WebSocket upgrade validated.

### 📂 Related Directories
```text
1-networking/certificates/
4-core-services/reverse-proxy/traefik/
docs/runbooks/traefik-stepca-tls.md
docs/runbooks/openclaw-end-to-end.md
```

### 🚧 Future Improvements
- Implement cert-manager + Step-CA issuer (GitOps model)
- Enforce TLS minimum version policy
- Add mTLS for sensitive services
- Automate certificate expiry monitoring
- Introduce WAF middleware

### 🔐 Security Model
- No direct service exposure without ingress
- No self-signed certs in production paths
- Root CA offline (recommended model)
- Intermediate CA used for service issuance
- Default deny east-west between VLANs