---
learning_level: "Advanced"
prerequisites: ["./02_Detailed-Design-Part1-D.md"]
estimated_time: "15 minutes"
learning_objectives:
  - "Implement Zero Trust security for real-time media"
  - "Secure WebRTC media streams with DTLS/SRTP"
  - "Design compliant recording and storage workflows"
related_topics:
  builds_upon: ["./02_Detailed-Design-Part1-D.md"]
  enables: ["./02_Detailed-Design-Part1-F.md"]
---

# Detailed Design: Security & Compliance

## 1. Authentication & Authorization

We adopt a **Zero Trust** model where every connection is authenticated.

### A. Identity Provider (IdP)

* **Azure AD / B2C**: Handles user identity (OAuth2/OIDC).
* **Flow**: Client logs in → Gets ID Token → Exchanges for **Short-Lived Join
  Token**.

### B. Short-Lived Tokens

* **Join Token**: JWT signed by the Auth Service, valid for 5-10 minutes. Contains
  `roomId`, `userId`, and `role` (Host/Participant).
* **TURN Credentials**: Ephemeral username/password generated via REST API
  (Time-Limited). Prevents unauthorized relay usage.

## 2. Network Security

### A. Control Plane

* **TLS 1.3**: All Signaling (HTTPS/WSS) traffic is encrypted.
* **WAF (Web Application Firewall)**: Azure Front Door protects against DDoS and
  Layer 7 attacks.
* **Private Link**: Backend services (Redis, SQL, Blob) are accessed only via
  private endpoints, never public internet.

### B. Media Plane

* **DTLS (Datagram Transport Layer Security)**: Key exchange for media streams.
* **SRTP (Secure Real-time Transport Protocol)**: Encrypts the actual audio/video
  payload.
* **E2EE (End-to-End Encryption)**: *Optional*. Keys exchanged between clients; SFU
  cannot decrypt media. (Trade-off: No server-side recording/transcription).

## 3. Compliance & Data Privacy

### A. Recording Consent

* **Workflow**: Host initiates recording → System broadcasts "Recording Started"
  event → Clients display UI indicator.
* **Storage**: Recordings stored in Azure Blob Storage with **Customer-Managed Keys
  (CMK)**.

### B. Data Residency

* **Regional Isolation**: Media traffic stays within the user's region (e.g., EU
  users routed to EU SFU clusters).
* **Metadata**: Stored in geo-replicated databases compliant with GDPR/CCPA.

### C. Audit Logs

* **Events**: Login attempts, Room creation, Recording access, Participant join/leave.
* **Storage**: Azure Monitor / Log Analytics with retention policies.
