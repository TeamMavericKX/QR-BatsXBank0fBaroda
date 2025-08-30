# Zero-Trust Blueprint

## Overview

This Zero-Trust Blueprint defines how QR-BATS secures high-value digital banking transactions by continuously verifying user identity, device posture, data integrity, and workload behavior. It follows the “never trust, always verify” principle, applying policy-driven controls at every step and enforcing least privilege across users, devices, APIs, and services.

## Core Principles

- Never trust, always verify: Every request is authenticated and authorized in real time.
- Assume breach: Design for containment, isolation, and rapid detection.
- Explicit verification: Identity, device, and session context are evaluated on every sensitive action.
- Least privilege: Access is scoped to the minimum required for the task.
- Continuous monitoring: Telemetry informs adaptive policies and step-up challenges.
- Defense in depth: Layered controls across identity, device, data, and workloads.

## Architecture (At a Glance)

![This is the QR-BATS system architecture.](ArchX01.png)

- PEP: Enforces decisions at API Gateway and service layers.
- PDP: Evaluates policies (OPA/Rego) with real-time context.
- PQC: Signs and verifies transaction hashes using post-quantum algorithms.
- Biometric: Continuous, liveness-detected facial/voice verification.
- KMS/HSM: Protects keys and secrets; supports certificate and key rotation.

---

## Identity

Every high-value transaction requires re-authentication with continuous, liveness-detected biometrics. User identity is verified at each access attempt, not just at login. FIDO Alliance standards for passkeys are integrated for initial login, and then QR-BATS for transaction signing.

Enhancements:
- Step-up triggers: velocity spikes, impossible travel, new device, or policy changes.
- Session constraints: short JWT TTLs, rotating refresh tokens, proof-of-possession (DPoP/MTLS-Bound tokens).
- Biometric freshness: enforce recency windows (e.g., ≤ 60s) before approving high-value transfers.

## Device/Session

Device posture is continuously monitored (e.g., via device health attestation gateways, as per Sub-theme 3). The PQC signing key is hardware-bound (TPM/Secure Enclave). Any deviation in device health or session behavior (e.g., impossible travel, velocity spikes detected by AI) triggers re-attestation or transaction blocking.

Signals and checks:
- OS integrity, boot state, jailbreak/root detection, secure lock screen.
- App integrity: code signing, runtime protection, anti-tamper, emulator detection.
- Network security: certificate pinning, TLS 1.3, mTLS to gateway.
- Session analytics: geovelocity, IP reputation, device fingerprint stability.

## Data

All transaction data is encrypted in transit and at rest. The transaction hash is cryptographically signed using PQC, ensuring data integrity and authenticity. Access to sensitive data is strictly on a need-to-know, least-privilege basis.

Controls:
- Transport: TLS 1.3 + mTLS; DNSSEC; HSTS; certificate pinning.
- At rest: AES-256-GCM; envelope encryption via KMS/HSM; row/field encryption for PII.
- Integrity: PQC signature (e.g., CRYSTALS-Dilithium) over canonical transaction hashes.
- Data governance: classification, masking, tokenization, and access via ABAC/RBAC.

## Workload/API

APIs for high-value transactions are secured with mTLS and granular access controls. Each API call is authorized based on the verified identity, device, and session context. Microservices architecture ensures isolation and reduces attack surface.

Best practices:
- Per-route authorization policies at the gateway and service mesh.
- OPA sidecars or external authorizers for consistent policy enforcement.
- Rate limiting, WAF, JWT validation, schema validation, and strong input sanitization.
- Service-to-service mTLS via mesh (e.g., Istio/Linkerd); least-privilege service accounts.

---

## Policy Model

- Inputs to PDP:
  - user: authn method, assurance level, groups/roles
  - device: attestation status, secure hardware, posture score
  - session: age, IP/ASN, geo, risk score
  - transaction: amount, currency, destination risk, purpose, created_at
  - biometric: liveness score, modality, freshness
- Outcomes:
  - permit/deny with obligations (e.g., require fresh biometric, enforce limit, log-only).
  - reason codes for auditability.

### Example OPA (Rego) Policy Snippet

```rego
package qr_bats.tx

default allow := false
deny_reason := msg {
  not allow
  msg := reason[_]
}

allow {
  input.user.authn.method == "fido_passkey"
  input.biometric.liveness_score >= 0.95
  now := time.now_ns()
  now - input.biometric.last_capture_ns <= 60 * 1000 * 1000 * 1000  # <= 60s
  input.device.attested == true
  input.device.secure_hardware == true
  input.session.risk_score <= 50
  input.transaction.amount <= input.policy.thresholds.max_without_stepup
}

# Step-up for higher amounts
allow {
  input.transaction.amount > input.policy.thresholds.max_without_stepup
  input.transaction.amount <= input.policy.thresholds.max_with_stepup
  input.biometric.step_up_performed == true
  input.device.attested == true
  input.session.risk_score <= 30
}

reason[msg] { input.user.authn.method != "fido_passkey"; msg := "authn_method_insufficient" }
reason[msg] { input.device.attested != true; msg := "device_not_attested" }
reason[msg] { input.biometric.liveness_score < 0.95; msg := "biometric_low_liveness" }
reason[msg] { time.now_ns() - input.biometric.last_capture_ns > 60 * 1e9; msg := "biometric_stale" }
```

---

## Enforcement Points

| Layer             | Control Examples                                     |
|-------------------|-------------------------------------------------------|
| Client            | Biometric SDK (liveness), device integrity checks     |
| Edge/Gateway (PEP)| mTLS, JWT/DPoP, OPA decision, WAF, rate limits        |
| Service           | OPA sidecar, ABAC, schema validation, secrets via KMS |
| Data              | Field encryption, masking, row-level security         |
| CI/CD             | IaC policy checks, SCA/DAST, signed artifacts (SLSA)  |

---

## Monitoring and Telemetry

- Metrics: policy eval latency, auth success rate, biometric FAR/FRR, PQC verify latency, error rates.
- Logs: structured, immutable, signed; forwarded to SIEM with correlation IDs.
- Tracing: distributed tracing across gateway → services → verifiers.
- Alerts: anomaly detections (impossible travel, device drift, API abuse).

## Incident Response (IR) Playbook

1. Detect: SIEM alert or anomaly threshold breach.
2. Contain: auto-quarantine device/session; enforce step-up or block.
3. Eradicate: rotate keys/tokens, revoke certs, invalidate sessions.
4. Recover: restore services; run post-incident reviews and policy updates.
5. Notify: audit and compliance stakeholders with evidence and timelines.

## Compliance Alignment

- NIST SP 800-207 (Zero Trust Architecture)
- ISO/IEC 27001 (ISMS controls)
- PCI DSS (for card-related flows, if applicable)
- GDPR/DPDP: data minimization, lawful basis, right to erasure (where applicable)

## KPIs and SLOs

- Policy decision latency: p95 ≤ 50 ms
- Biometric verification latency: p95 ≤ 300 ms
- PQC signature verification latency: p95 ≤ 150 ms
- False Acceptance Rate (FAR) ≤ target; False Rejection Rate (FRR) ≤ target
- MTTR for security incidents ≤ 2 hours

## Configuration Guardrails

- Short-lived tokens (e.g., access ≤ 10 min; refresh ≤ 24 h); bind to mTLS.
- Certificate rotation and pinning; enforce TLS 1.3 only.
- Biometric freshness window ≤ 60 seconds for high-value actions.
- Device attestation required for transaction signing; deny if posture unknown.
- Explicit deny-by-default; all permissions via policy.

---

## Threat Model (Summary)

- Credential stuffing → mitigated by passkeys + biometric step-up.
- MITM/SSL stripping → TLS 1.3 + pinning + mTLS.
- Device compromise/rooting → attestation, runtime protection, key in Secure Enclave/TPM.
- Replay/signature misuse → nonce/challenge + PQC signing of transaction hash.
- Insider or lateral movement → microsegmentation, least privilege, audit trails.

---

## Getting Started (Implementation Steps)

1. Integrate FIDO2 passkeys for login; enforce biometric step-up per policy.
2. Bind PQC key material to Secure Enclave/TPM; sign canonical transaction hashes.
3. Deploy API Gateway with mTLS, OPA external authorizer, rate limits, and WAF.
4. Instrument services for structured logs, metrics, and traces; ship to SIEM.
5. Define initial OPA policies (thresholds, freshness windows); iterate via canary releases.
6. Enable device posture ingestion (MDM/attestation) and risk scoring feedback loops.

---

## Glossary

- PEP: Policy Enforcement Point—applies policy decisions on requests.
- PDP: Policy Decision Point—evaluates policy (OPA/Rego) using request context.
- PQC: Post-Quantum Cryptography—algorithms resistant to quantum attacks.
- Liveness: Biometric checks ensuring a real, present user (not a spoof).
- UEBA: User and Entity Behavior Analytics for anomaly detection.
