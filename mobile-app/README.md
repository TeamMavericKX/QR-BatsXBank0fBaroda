# Mobile Application (React Native/Flutter)

## Overview

The mobile application serves as the primary user interface for interacting with the QR-BATS system. Developed using cross-platform frameworks like React Native or Flutter, it ensures a consistent user experience across both iOS and Android devices. The app is responsible for initiating high-value transactions, capturing biometric attestations, and securely communicating with the backend services.

## Key Functionalities

- Transaction Initiation: Users can initiate high-value fund transfers or other sensitive banking operations directly from the mobile app.
- Biometric Capture: Integrates with Biometric SDKs (FaceTec, VoiceIt.ai) to capture continuous, liveness-detected facial and/or voice biometrics for user authentication and transaction signing.
- PQC Signing: Incorporates a Post-Quantum Cryptography (PQC) Signing Module (OQS) to cryptographically sign transaction hashes on the user's device, leveraging hardware security features like TPM/Secure Enclave.
- Secure Communication: Establishes secure, authenticated communication channels with the backend API Gateway to transmit signed transactions and biometric data.
- User Experience: Designed for intuitive navigation and a seamless user experience, minimizing friction while maximizing security.

## Implementation Details

The mobile application will utilize either React Native or Flutter for its cross-platform capabilities, allowing for a single codebase to target multiple operating systems. The choice between React Native and Flutter will be based on factors such as developer expertise, performance requirements, and ecosystem maturity.

Integration with biometric SDKs will be crucial for capturing high-assurance biometric data. The PQC signing module will be implemented to interact with the device's hardware security features, ensuring that the private keys used for signing are protected against software-based attacks.

## Requirements

- Cross-Platform Compatibility: Must run seamlessly on both iOS and Android devices.
- Biometric Integration: Requires robust integration with third-party biometric SDKs for facial and voice recognition.
- Hardware Security Utilization: Ability to leverage device-specific hardware security modules (TPM/Secure Enclave) for key storage and cryptographic operations.
- Offline Capabilities (Limited): While primarily online, certain functionalities like initial biometric capture or transaction preparation might have limited offline capabilities.
- Performance: Optimized for fast and responsive user interactions, especially during biometric capture and transaction signing.
- Security: Adherence to mobile security best practices, including secure data storage, secure communication protocols, and protection against reverse engineering.
