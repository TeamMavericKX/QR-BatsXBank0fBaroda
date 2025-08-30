# 🛡️ QR-BATS: Quantum-Resilient, Biometric-Attested Transaction Signing

This repository contains the official submission for the Bank of Baroda Zero-Trust Digital Banking Hackathon. Our project, QR-BATS, provides a next-generation security architecture for high-value digital transfers, directly addressing 

Sub-theme 18: Risk-Based Investment Orders and Sub-theme 22: Geo-Fenced High-Value TXNs.

## 📖 Table of Contents
- [The Problem](#the-problem)
- [Our Solution: QR-BATS](#our-solution-qr-bats)
- [Key Features](#key-features)
- [Architecture Diagram](#architecture-diagram)
- [Zero-Trust Blueprint](#zero-trust-blueprint)
- [Technology Stack](#technology-stack)
- [The Team](#the-team)
- [License](#license)

## 💡 The Problem
High-value digital banking is at a critical inflection point. Transactions are increasingly targeted by sophisticated threats that render traditional security obsolete. The core issues are:

- **Emerging Quantum Threats**: Current encryption standards are vulnerable to future quantum computers. Cybersecurity Ventures forecasts that quantum attacks could neutralize today's encryption within two years, leading to a potential 'Y2Q' (Year to Quantum) event by 2031.
- **Advanced Fraud**: Sophisticated deepfake attacks, phishing, and social engineering can easily bypass traditional Multi-Factor Authentication (MFA) methods like OTPs.
- **Concentrated High-Value Fraud**: While the overall number of fraud cases may be declining, the financial impact of each incident is skyrocketing. RBI reports indicate a sharp jump in the amount involved in fraud to ₹36,014 crore, highlighting a focus on high-value targets.

## ✨ Our Solution: QR-BATS
We propose the 

**Quantum-Resilient, Biometric-Attested Transaction Signing (QR-BATS)** system—a dynamic, multi-layered security protocol built on Zero-Trust principles.

Our solution transforms transaction signing into an unassailable, user-centric process:

1. **Initiation**: A user initiates a high-value transfer, and the system generates a unique transaction hash.
2. **Live Biometric Attestation**: The app prompts the user for a liveness-detected biometric scan (e.g., facial recognition with anti-spoofing) to prove they are physically present.
3. **Hardware-Secured Signing**: This biometric data unlocks a private key stored within the device's Hardware Security Module (e.g., TPM or Secure Enclave).
4. **Quantum-Resistant Signature**: The key signs the transaction hash using a Post-Quantum Cryptography (PQC) algorithm like CRYSTALS-Dilithium.
5. **Zero-Trust Verification**: The PQC-signed payload is sent to the bank's backend, where a Zero-Trust Policy Engine verifies the signature, biometric attestation, and device posture before approving the transaction.

This future-proofs the transaction against quantum attacks while ensuring it is irrefutably authorized by the legitimate user.

## 🚀 Key Features
- **Future-Proof Security**: Provides robust protection against emerging quantum threats, ensuring the long-term integrity of high-value transactions.
- **Superior Fraud Prevention**: Effectively neutralizes deepfake, phishing, and social engineering attacks through continuous, liveness-detected biometric attestation.
- **Enhanced User Experience**: Streamlines high-value transaction authorization by replacing cumbersome OTPs with seamless biometric verification.
- **Strong Non-Repudiation**: The combination of a PQC signature and irrefutable biometric proof provides undeniable evidence of transaction authorization, reducing disputes.
- **Comprehensive Zero-Trust**: A holistic security model integrating the Identity, Device/Session, Data, and Workload/API pillars of Zero-Trust.

## 🏗️ Architecture Diagram
This diagram illustrates the end-to-end flow of the QR-BATS system, from the user's device to the bank's core infrastructure.

![Architecture Diagram](https://github.com/the-ai-developer/QR-BatsXBank0fBaroda/blob/main/docs/ArchX02.png))

## 🔐 Zero-Trust Blueprint
Our architecture is built on the core principle of "never trust, always verify" for every single high-value transaction.

- **Identity**: Every transaction requires re-authentication with liveness-detected biometrics. Identity is verified at each access attempt, not just at login.
- **Device/Session**: Device posture is continuously monitored. The PQC signing key is hardware-bound in a TPM or Secure Enclave, ensuring it cannot be exfiltrated.
- **Data**: All transaction data is encrypted in transit and at rest. The core transaction hash is signed using PQC, ensuring ultimate data integrity and authenticity.
- **Workload/API**: All APIs are secured with mTLS, and every API call is authorized based on the verified context of the user, device, and session.

## ⚙️ Technology Stack

| Category          | Technology                                                                 |
|-------------------|----------------------------------------------------------------------------|
| Cryptography     | Open Quantum Safe (OQS) Library (Dilithium, Falcon), OpenSSL               |
| Biometrics       | FaceTec (Facial Liveness), VoiceIt.ai (Voice Biometrics)                   |
| Mobile Development | React Native / Flutter                                                     |
| Backend          | Python (Flask/FastAPI), Java (Spring Boot)                                 |
| Database         | PostgreSQL, Redis                                                          |
| Cloud Platform   | AWS (EC2, Lambda, RDS, S3, KMS, CloudWatch)                               |
| Zero-Trust       | Custom PEP/PDP Services, Open Policy Agent (OPA)                           |
| Security & Ops   | HashiCorp Vault, Prometheus, Grafana                                       |

## 👥 The Team
- **Kishore Muruganantham** – [https://github.com/KishoreMuruganantham](https://github.com/KishoreMuruganantham)
- **Mugundhan Y** – [https://github.com/MugundhanY](https://github.com/MugundhanY)
- **Mukunth AP** – [https://github.com/MukundhArul](https://github.com/MukundhArul)
- **J. Prince Raj** – [https://github.com/the-ai-developer](https://github.com/the-ai-developer)

## 📜 License
This project is licensed under the MIT License. All code was developed during the hackathon period, and the intellectual property belongs to the team as per hackathon rules.
