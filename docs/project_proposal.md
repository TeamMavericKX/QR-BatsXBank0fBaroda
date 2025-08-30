# QR-BATS: Quantum-Resilient, Biometric-Attested Transaction Signing

Project Proposal for Bank of Baroda Zero-Trust Digital Banking Hackathon

---

## Theme: Fraud & Transaction Trust
Strong integration with Identity & Session Security, and Data Protection & Privacy.

### Use-case Mapping
- Fraud & Transaction Trust
  - Geo-fenced high-value transactions: Quantum-resilient cryptographic signing + continuous biometric attestation for high-value TXNs, ideal for geo-fenced scenarios.
  - Real-time ATO response: Continuous, liveness-detected biometrics and device posture monitoring for adaptive responses beyond static checks/OTPs.
- Identity & Session Security
  - Biometric passkeys: FIDO passkeys for initial login, complemented by QR-BATS for transaction signing.
  - Continuous session auth: Ongoing verification during high-value flows with fresh biometrics.
  - Device posture for field agents: Hardware-bound PQC keys (TPM/Secure Enclave); real-time posture checks for diverse device environments.
- Data Protection & Privacy
  - PQC foundations: Hybrid PQC+ECC to ensure integrity and authenticity of transaction data and pave the way for quantum-safe communications.

---

## Problem Statement & Target Persona
High-value digital banking transactions face rising quantum threats, deepfakes, and social engineering—causing financial loss and trust erosion. Traditional MFA (OTPs/static factors) is insufficient against modern adversaries. Target users include:
- High-net-worth individuals, corporate treasurers, and field agents initiating large transfers or sensitive investment orders—requiring unassailable security with low friction.

---

## What QR-BATS Delivers
- Post-Quantum signing on-device: Canonical transaction hash signed using PQC (e.g., Dilithium/ML-DSA), hardware-bound to Secure Enclave/TPM.
- Continuous, liveness-detected biometrics: Live face/voice attestation to bind user presence to each high-value action.
- Zero-Trust enforcement: Real-time, policy-driven decisions per API call (identity, device posture, session risk).
- Non-repudiation: Cryptographic proof + biometric attestation for irrefutable approvals.

---

## Architecture Diagram
- High-level architecture and data flows:
  - https://github.com/TeamMavericKX/QR-BatsXBank0fBaroda/blob/main/docs/ArchX02.png
  - https://github.com/TeamMavericKX/QR-BatsXBank0fBaroda/blob/main/docs/Archx01.png

---

## Demo Video
Watch the demo to see seamless biometric attestation, quantum-resilient signing, and real-time fraud prevention in action.

- Video link: https://drive.proton.me/urls/WET25P3PCC#s7YAZNWbm0q6

---

## Repository
- GitHub (Team Org): https://github.com/TeamMavericKX/QR-BatsXBank0fBaroda.git

---

## Zero-Trust Blueprint
Comprehensive breakdown across Identity, Device/Session, Data, and Workload/API pillars.

- Link: https://github.com/TeamMavericKX/QR-BatsXBank0fBaroda/blob/main/docs/zero_trust_blueprint.md

---

## Risk & Threat Model
Top-3 risks (Quantum algorithm vulnerability, Biometric spoofing/deepfakes, Key compromise) with concrete mitigations (crypto agility + hybrid PQC/ECC, multi-modal + liveness + behavioral biometrics, strict key management).

- Link: https://github.com/TeamMavericKX/QR-BatsXBank0fBaroda/blob/main/docs/risk_threat_model.md

---

## Bill of Materials

### APIs
- Bank of Baroda Core Banking APIs: Initiation, balances, and core transactional flows.
- Biometric SDKs (FaceTec, VoiceIt.ai): Liveness detection and biometric capture (face and voice).
- Open Quantum Safe (OQS) / OpenSSL: PQC algorithms (e.g., Dilithium, Falcon; hybrid KEMs like ML-KEM/Kyber).
- Open Policy Agent (OPA): Policy evaluation at PDP for Zero-Trust authorization.

### Datasets
- Encrypted biometric templates for verification (no raw biometrics persisted).
- Transaction history for behavioral analytics and fraud detection.
- Device posture and attestation metadata (health, integrity, secure hardware flags).
- PQC PKI data (public keys, cert chains, rotations).

### Cloud Services (AWS)
- EC2: Backend services and compute workloads.
- Lambda: Serverless functions for event-driven tasks.
- RDS (PostgreSQL): Transaction logs, user profiles.
- S3: Object storage (artifacts, export logs).
- KMS: Key management and envelope encryption.
- CloudWatch: Monitoring, logs, alerts.
- API Gateway: Endpoint management, routing, initial security.
- Route 53: DNS.

---

## Phased Roadmap
- Phase 1 (MVP): PQC-enabled signing (OQS) + facial liveness; pilot with test accounts.
- Phase 2: Voice biometrics, real-time core banking integration, expanded policy set.
- Phase 3: Scale-out, performance tuning, device posture integrations, enterprise rollout.

---

## Getting Started (Developed During The Dev Timeline) 
- Mobile (React Native/Flutter): Capture liveness biometrics → sign transaction hash via PQC → send to API Gateway.
- Backend (Flask/FastAPI or Spring Boot): Validate PQC signature, verify biometric attestation, enforce OPA policies, update core banking.
- Basic steps:
  1. Clone repo: `git clone https://github.com/TeamMavericKX/QR-BatsXBank0fBaroda.git`
  2. Backend: `cd backend-services && make dev` (sample command; see service README)
  3. Mobile: `cd mobile-app && npm install` or `flutter pub get` → run on device/emulator.
  4. Configure secrets via environment or Vault; run gateway + PDP; verify end-to-end test flow.

---

## License / IP Attestation
We confirm the originality of this project. Code and documentation were developed during the hackathon. Open-source components are used under their respective licenses. IP belongs to the team per hackathon rules and T&Cs.

---

## The Team
- Kishore Muruganantham — https://github.com/KishoreMuruganantham
- Mugundhan Y — https://github.com/MugundhanY
- Mukunth AP — https://github.com/MukundhArul
- J. Prince Raj — https://github.com/the-ai-developer

---

## Call to Action
Pilot QR-BATS with Bank of Baroda for high-value transactions. Reduce deepfake-based fraud, future-proof against quantum threats, and deliver trust with minimal user friction.
