# Risk & Threat Model

## Overview
This risk and threat model outlines the priority risks for QR-BATS and how we mitigate them using Zero-Trust controls, PQC, biometrics, and strong key management. It aligns with STRIDE and MITRE ATT&CK-style thinking and is reviewed continuously to adapt to new threats (especially quantum and AI-driven).

## Top-3 Risks & Mitigations

### 1) Quantum Algorithm Vulnerability
- Threat Vector: Future quantum computer attack (harvest-now, decrypt-later)
- Likelihood: Medium (long-term)
- Impact: High (catastrophic if exploited)
- Detection/Monitoring:
  - Maintain a cryptographic inventory (algorithms, key sizes, locations)
  - Monitor NIST PQC guidance and library advisories (OQS/liboqs, BoringSSL/OpenSSL)
  - Crypto-agility drills and staged rollouts in lower environments
- Mitigations:
  - Cryptographic agility framework to swap algorithms without app rewrites
  - Hybrid schemes now: X25519 + ML-KEM (Kyber) for KEM; ECDSA/Ed25519 + ML-DSA (Dilithium) for signatures
  - TLS 1.3 with PQC-hybrid ciphersuites where supported; record agility flags in telemetry
  - Key lifecycle hygiene: short-lived certs/keys, automated rotation, centralized KMS/HSM
  - Backward-compatible data formats (versioned envelopes) to support re-encryption and re-signing

### 2) Biometric Spoofing / Deepfake Evolution
- Threat Vector: Advanced AI-generated media (video/voice), presentation/injection attacks
- Likelihood: Medium
- Impact: High (transaction authorization bypass)
- Detection/Monitoring:
  - Track FAR/FRR and liveness scores over time; drift detection for model performance
  - Quality gating (SNR for voice; face occlusion/lighting checks)
  - Injection detection: block emulator/screen-replay; enforce secure OS prompts (Class 3 biometrics)
- Mitigations:
  - Multi-modal biometrics (face + voice) with randomized challenge/response prompts
  - Device-native secure biometrics (Face ID/Android BiometricPrompt Class 3) + SDK liveness
  - Behavioral biometrics (typing cadence, gait, device motion) as a secondary signal
  - Anti-spoof updates: regular SDK model refresh; A/B canaries; vendor diversity for resilience
  - Step-up controls: require fresh biometric (≤ 60s) for high-value or high-risk contexts

### 3) Key Compromise
- Threat Vector: Insider threat, malware, supply-chain compromise
- Likelihood: Low
- Impact: Critical (signing/verification trust erosion)
- Detection/Monitoring:
  - HSM/KMS audit logs with real-time alerting and four-eyes approvals
  - Anomalous signing rate detection; attestation failures; unexpected geo/IP
  - Code integrity attestation for client and backend artifacts
- Mitigations:
  - Hardware-backed keys (Secure Enclave/TPM on device; HSM/KMS server-side)
  - Strict key management: separation of duties, quorum approvals, and frequent rotation (e.g., ≤ 7 days for sensitive app keys)
  - mTLS with cert pinning; DPoP/MTLS-bound tokens to prevent token replay
  - Secret zeroization on policy breach; rapid kill-switch and CRL/OCSP updates
  - Regular security audits, pentests, and supply-chain controls (SLSA, signed artifacts)

---

## Risk Register (Summary)

| Risk                           | Threat Vector                         | Likelihood | Impact | Severity | Key Mitigations                                                                                 | Owner               | Review |
|--------------------------------|---------------------------------------|------------|--------|---------|--------------------------------------------------------------------------------------------------|---------------------|--------|
| Quantum Algorithm Vulnerability| Harvest-now, decrypt-later            | Medium     | High   | High    | PQC hybrid, crypto agility, short-lived keys, TLS 1.3 PQC-hybrid, KMS/HSM                        | Crypto/Sec Eng      | Qtrly  |
| Biometric Spoofing/Deepfake    | AI-generated media, injection attacks | Medium     | High   | High    | Multi-modal + liveness, device-native biometrics, behavioral signals, step-up, SDK updates      | Biometrics/ML Eng   | Monthly|
| Key Compromise                 | Insider/malware/supply-chain          | Low        | Critical| High   | HSM/KMS, rotation ≤ 7d, approvals, mTLS pinning, DPoP, audits/pentests, kill-switch             | Platform/Sec Eng    | Monthly|

---

## Risk Heat Map

|            | Impact: Low | Impact: Medium | Impact: High | Impact: Critical |
|------------|-------------|----------------|--------------|------------------|
| Likelihood: Low    |   L      |   L-M          |   M          |   H              |
| Likelihood: Medium |   L-M    |   M            |   H          |   H              |
| Likelihood: High   |   M      |   H            |   H          |   H              |

- Placement (current): Quantum → Medium/High, Biometrics → Medium/High, Key Compromise → Low/Critical.

---

## Detection & Telemetry
- Policy and auth metrics: decision latency, success/failure rates, step-up frequency
- Biometrics: FAR/FRR, liveness score distribution, model drift indicators
- Crypto: PQC-hybrid negotiation rate, failed verifications, cert/key rotation SLAs
- Keys/HSM: admin action logs, key usage anomalies, failed attestation counts
- API security: rate anomalies, schema violations, WAF hits, mTLS handshake failures
- All logs are structured, signed, and correlated via trace IDs; shipped to SIEM with alerting

---

## Validation & Testing
- PQC interop tests across mobile, gateway, and services (ML-KEM + ML-DSA hybrids)
- Biometric red-team tests: presentation/injection (3D masks, TTS/VC) in controlled exercises
- Chaos/security game days for key rotation, CRL distribution, and kill-switch procedures
- Secure SDLC: SAST, DAST, SCA; supply-chain signing and verification (SBOM + provenance)

---

## Incident Playbooks (Abbrev.)
- Quantum weakness disclosure:
  - Freeze vulnerable algos → enable hybrid-only → rotate affected keys → re-sign critical artifacts/logs
- Biometric spoofing campaign:
  - Elevate thresholds → enforce multi-modal step-up → roll SDK hotfix → expand behavior signals
- Suspected key compromise:
  - Revoke/rotate keys → block device attestations as needed → force re-enrollment → forensics + postmortem

---

## References & Standards
- NIST SP 800-207 (Zero Trust), NIST SP 800-63-3 (Digital Identity), ISO/IEC 27005 (Risk), OWASP ASVS
- NIST PQC standards: ML-KEM (Kyber) and ML-DSA (Dilithium)
- FIDO Alliance (passkeys), WebAuthn/CTAP
- MITRE ATT&CK (threat techniques), ASVspoof benchmarks (voice anti-spoofing)

---

## Review Cadence & Ownership
- Monthly risk review with Security, Platform, and Biometrics teams
- Quarterly crypto-agility and PQC readiness drills
- SLAs: key rotation ≤ 7 days (sensitive keys), incident MTTR ≤ 2 hours, policy decision p95 ≤ 50 ms

---
