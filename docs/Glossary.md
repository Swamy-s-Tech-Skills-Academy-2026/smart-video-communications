# Glossary

> **Purpose**: This glossary defines technical terms, acronyms, and domain-specific concepts used throughout the Smart Video Communications platform documentation.

---

## Table of Contents

- [WebRTC & Media Terms](#webrtc--media-terms)
- [Architecture & Infrastructure Terms](#architecture--infrastructure-terms)
- [Security & Authentication Terms](#security--authentication-terms)
- [AI & Machine Learning Terms](#ai--machine-learning-terms)
- [Protocols & Standards](#protocols--standards)
- [Azure & Cloud Terms](#azure--cloud-terms)
- [Development & DevOps Terms](#development--devops-terms)
- [General Software Engineering Terms](#general-software-engineering-terms)

---

## WebRTC & Media Terms

### ABR (Adaptive Bitrate Streaming)
A technique that automatically adjusts video quality based on network conditions. The player monitors available bandwidth and switches between different quality levels (e.g., 1080p, 720p, 480p) to maintain smooth playback.

**Context**: Used in live streaming architecture (HLS/DASH) to handle varying network conditions.

### CDN (Content Delivery Network)
A distributed network of servers that deliver content (video, images, web pages) to users based on geographic proximity. Reduces latency by serving content from edge locations closest to users.

**Context**: Used for live streaming to millions of viewers (HLS/DASH delivery).

### Codec
A technology for encoding and decoding digital media (audio/video). Common codecs include H.264, VP8, VP9, Opus, and AAC.

**Context**: Specified in SDP offers/answers during WebRTC connection establishment.

### DTLS (Datagram Transport Layer Security)
A security protocol that provides encryption for UDP-based communications. Used in WebRTC to secure media streams.

**Context**: WebRTC uses DTLS to encrypt media traffic over UDP, ensuring secure real-time communication.

### E2EE (End-to-End Encryption)
A security model where data is encrypted on the sender's device and only decrypted on the receiver's device. No intermediate servers can decrypt the data.

**Context**: Optional feature for maximum privacy in video calls, where even the SFU cannot decrypt media.

### HLS (HTTP Live Streaming)
A streaming protocol developed by Apple that breaks video into small chunks and delivers them via HTTP. Uses manifest files (.m3u8) to describe available segments.

**Context**: Used for live streaming to millions of viewers when WebRTC is not scalable.

### ICE (Interactive Connectivity Establishment)
A framework used by WebRTC to establish peer-to-peer connections through NATs and firewalls. Combines STUN and TURN to find the best network path.

**Context**: Core mechanism for WebRTC connection establishment, involving candidate gathering and connectivity checks.

### MCU (Multipoint Control Unit)
A media server that mixes all incoming video/audio streams into a single composite stream and sends it back to participants. Reduces client bandwidth but requires high server CPU.

**Context**: Legacy approach for group calls; not used in this system (SFU is preferred).

### Mediasoup
An open-source SFU implementation written in Node.js/C++. Provides granular control over media routing and is designed for Kubernetes deployment.

**Context**: Selected as the SFU solution for the Smart Video Communications platform.

### Mesh Topology (P2P)
A network architecture where every participant connects directly to every other participant. Each user sends and receives streams from all other participants.

**Context**: Used for small groups (≤5 participants) to minimize server costs and latency.

### NAT (Network Address Translator)
A networking technique that maps private IP addresses to public IP addresses. Most devices sit behind NATs, which complicates direct peer-to-peer connections.

**Context**: NAT traversal is a key challenge solved by STUN and TURN servers in WebRTC.

### P2P (Peer-to-Peer)
A communication model where two or more devices connect directly without an intermediary server for media routing.

**Context**: Used for 1:1 calls and small groups (≤5) in mesh topology.

### RTP (Real-time Transport Protocol)
A network protocol for delivering audio and video over IP networks. Provides sequence numbers, timestamps, and payload type identification.

**Context**: WebRTC uses RTP (encrypted as SRTP) for media packet delivery.

### RTCPeerConnection
A Web API that represents a WebRTC connection between a local computer and a remote peer. Handles media capture, encoding, and network connectivity.

**Context**: Core API used in client SDKs to establish WebRTC connections.

### SDP (Session Description Protocol)
A format for describing multimedia communication sessions. Contains information about codecs, network addresses, and session metadata.

**Context**: Used in WebRTC offer/answer exchange to negotiate media capabilities between peers.

### SFU (Selective Forwarding Unit)
A media server that receives media streams from participants and selectively forwards them to other participants without mixing. Each participant receives individual streams.

**Context**: Used for medium to large groups (5-50 participants) to reduce client bandwidth while maintaining low latency.

### Simulcast
A technique where a sender publishes multiple quality levels (high, medium, low) of the same stream. The receiver or SFU selects the appropriate quality based on network conditions.

**Context**: Used in SFU architecture for large groups (50+) to handle varying network conditions.

### SRTP (Secure Real-time Transport Protocol)
An extension of RTP that provides encryption, authentication, and integrity for media streams.

**Context**: WebRTC uses SRTP over UDP for secure media delivery.

### STUN (Session Traversal Utilities for NAT)
A protocol that helps clients discover their public IP address and port. Used in WebRTC to find server-reflexive candidates for NAT traversal.

**Context**: First step in ICE candidate gathering; helps establish direct peer-to-peer connections.

### TURN (Traversal Using Relays around NAT)
A protocol that provides relay servers for media traffic when direct peer-to-peer connections fail. Acts as a media proxy.

**Context**: Fallback mechanism in WebRTC when STUN fails (e.g., symmetric NATs or restrictive firewalls).

### WebRTC (Web Real-Time Communication)
A collection of standards and APIs that enable real-time communication (audio, video, data) directly between web browsers and mobile applications without plugins.

**Context**: Core technology for real-time media streaming in the Smart Video Communications platform.

---

## Architecture & Infrastructure Terms

### AKS (Azure Kubernetes Service)
A managed Kubernetes service on Azure that simplifies deployment, management, and scaling of containerized applications.

**Context**: Primary orchestration platform for deploying microservices, SFU, and AI services.

### API Gateway
A service that acts as a single entry point for client requests. Handles routing, authentication, rate limiting, and request/response transformation.

**Context**: Azure API Management (APIM) serves as the API gateway for the platform.

### Control Plane
The part of the system responsible for signaling, session management, and orchestration. Handles call setup, participant management, and coordination.

**Context**: Built with .NET 10, SignalR, and Redis. Separate from the media plane.

### Data Plane (Media Plane)
The part of the system responsible for actual media (audio/video) routing and processing. Handles RTP streams, transcoding, and media delivery.

**Context**: Consists of SFU (Mediasoup), TURN servers, and media processing services.

### Horizontal Pod Autoscaler (HPA)
A Kubernetes feature that automatically scales the number of pod replicas based on CPU, memory, or custom metrics.

**Context**: Used to automatically scale SFU pods and other services based on load.

### IaC (Infrastructure as Code)
The practice of managing and provisioning infrastructure through code (scripts, templates) rather than manual configuration.

**Context**: Bicep or Terraform templates define AKS clusters, storage, and networking resources.

### Microservices
An architectural pattern where an application is built as a collection of loosely coupled, independently deployable services. Each service owns its data and communicates via APIs.

**Context**: The platform is decomposed into domain-specific microservices (User Service, Meeting Service, Signaling Service, etc.).

### Service Mesh
A dedicated infrastructure layer for managing service-to-service communication. Provides features like load balancing, service discovery, and observability.

**Context**: Can be used for inter-service communication, though not explicitly mentioned in current design.

### VMSS (Virtual Machine Scale Sets)
An Azure compute resource that allows you to create and manage a group of identical, load-balanced VMs that automatically scale based on demand.

**Context**: Used for deploying TURN servers (Coturn) for high network throughput.

---

## Security & Authentication Terms

### Azure AD / Entra ID
Microsoft's cloud-based identity and access management service. Provides authentication and authorization for users and applications.

**Context**: Used for user authentication and single sign-on (SSO) in the platform.

### Azure AD B2C
A customer identity and access management solution for consumer-facing applications. Provides social login, custom branding, and user management.

**Context**: Alternative to Azure AD for customer-facing authentication.

### JWT (JSON Web Token)
A compact, URL-safe token format for securely transmitting information between parties. Contains claims (user ID, permissions) and is digitally signed.

**Context**: Short-lived join tokens (5-10 minutes) are JWTs used to authenticate users joining meetings.

### OAuth2
An authorization framework that allows applications to obtain limited access to user accounts. Defines flows for authorization and token exchange.

**Context**: Used for authentication flows with Azure AD/B2C.

### OIDC (OpenID Connect)
An authentication layer built on top of OAuth2. Provides identity information (ID tokens) in addition to access tokens.

**Context**: Used for user authentication, providing ID tokens that are exchanged for join tokens.

### RBAC (Role-Based Access Control)
An access control method that assigns permissions to users based on their roles (e.g., host, participant, viewer).

**Context**: Used for meeting access control and permission management.

### TLS (Transport Layer Security)
A cryptographic protocol that provides secure communication over a network. Successor to SSL.

**Context**: TLS 1.3 is used for all signaling traffic (WebSocket connections).

### WAF (Web Application Firewall)
A security service that monitors and filters HTTP/HTTPS traffic to protect web applications from attacks.

**Context**: Azure Front Door provides WAF protection for the platform.

### Zero Trust
A security model that assumes no implicit trust based on network location. Every connection must be authenticated and authorized.

**Context**: Core security principle for the platform - every connection is authenticated regardless of network location.

---

## AI & Machine Learning Terms

### Azure Cognitive Services Speech SDK
A Microsoft service that provides speech-to-text, text-to-speech, and translation capabilities via APIs.

**Context**: Used for real-time transcription and translation in meetings.

### Consent Management
The process of obtaining and managing user consent for data processing, recording, and AI analysis. Critical for GDPR/CCPA compliance.

**Context**: Users must explicitly consent before recording or AI processing can occur.

### PII (Personally Identifiable Information)
Information that can be used to identify an individual (names, phone numbers, email addresses, etc.).

**Context**: PII redaction is performed on transcripts before storage to protect privacy.

### RAG (Retrieval-Augmented Generation)
An AI technique that combines information retrieval with language model generation to provide more accurate and contextual responses.

**Context**: Mentioned in AI services libraries but not explicitly used in current design.

### STT (Speech-to-Text)
The process of converting spoken language into written text. Also known as transcription.

**Context**: Real-time STT is provided by Azure Speech API for meeting transcripts.

---

## Protocols & Standards

### DASH (Dynamic Adaptive Streaming over HTTP)
A streaming protocol that enables adaptive bitrate streaming over HTTP. Similar to HLS but standardized by MPEG.

**Context**: Alternative to HLS for live streaming to millions of viewers.

### gRPC
A high-performance, open-source RPC (Remote Procedure Call) framework. Uses HTTP/2 and Protocol Buffers for efficient communication.

**Context**: Can be used for streaming APIs (e.g., real-time transcription) instead of REST.

### HTTP/2
The second major version of the HTTP protocol. Provides multiplexing, header compression, and server push for improved performance.

**Context**: Used by gRPC and modern web APIs.

### OpenAPI (Swagger)
A specification for describing RESTful APIs. Provides a standard format for API documentation, code generation, and testing.

**Context**: APIs should be documented using OpenAPI/Swagger specifications.

### REST (Representational State Transfer)
An architectural style for designing web services. Uses standard HTTP methods (GET, POST, PUT, DELETE) and stateless communication.

**Context**: RESTful APIs are used for meeting management, user management, and other non-real-time operations.

### WebSocket
A communication protocol that provides full-duplex communication over a single TCP connection. Enables real-time, bidirectional communication.

**Context**: SignalR uses WebSocket (with fallbacks) for real-time signaling and chat.

### WSS (WebSocket Secure)
WebSocket over TLS/SSL, providing encrypted WebSocket connections.

**Context**: All WebSocket connections (SignalR) use WSS for security.

---

## Azure & Cloud Terms

### ACR (Azure Container Registry)
A managed Docker container registry service on Azure. Stores and manages container images for deployment.

**Context**: Container images are pushed to ACR before deployment to AKS.

### Azure Blob Storage
Azure's object storage service for storing unstructured data (files, images, videos). Provides hot, cool, and archive tiers.

**Context**: Used for storing meeting recordings, transcripts, and shared files.

### Azure Front Door
A global CDN and application delivery network service. Provides WAF, SSL termination, and load balancing.

**Context**: Entry point for all client traffic, providing DDoS protection and global distribution.

### Azure Key Vault
A cloud service for securely storing and accessing secrets (API keys, passwords, certificates). Provides access control and audit logging.

**Context**: Used for storing connection strings, API keys, and certificates with managed identities.

### Azure Monitor
A comprehensive monitoring and observability platform for Azure resources. Collects metrics, logs, and traces.

**Context**: Centralized logging and monitoring for all services.

### Azure SQL Database
A managed relational database service on Azure. Provides high availability, automatic backups, and scaling.

**Context**: Option for persistent metadata storage (alternative to PostgreSQL).

### Managed Identity
An Azure feature that provides automatically managed identities for Azure resources. Eliminates the need for storing credentials.

**Context**: Services use managed identities to access Azure Key Vault and other Azure resources securely.

### Private Link
An Azure service that provides private connectivity to Azure services. Traffic stays on the Microsoft network.

**Context**: Can be used to secure connections between services without exposing them to the public internet.

---

## Development & DevOps Terms

### .NET Aspire
A cloud-native application model for .NET that simplifies building observable, distributed applications. Provides service discovery, configuration, and orchestration.

**Context**: .NET Aspire 13 is used for orchestrating microservices and local development.

### ArgoCD
A GitOps continuous delivery tool for Kubernetes. Automatically syncs application deployments based on Git repository state.

**Context**: Used for GitOps-based deployment to AKS clusters.

### Canary Deployment
A deployment strategy where a new version is gradually rolled out to a small percentage of users (e.g., 5%) before full deployment.

**Context**: New Signaling/SFU versions are deployed using canary strategy (5% traffic initially).

### CI/CD (Continuous Integration/Continuous Deployment)
A set of practices that automate building, testing, and deploying software. CI focuses on code integration, CD focuses on automated deployment.

**Context**: GitHub Actions provides CI/CD pipelines for the platform.

### Flux
An alternative GitOps tool for Kubernetes. Automatically syncs cluster state with Git repository.

**Context**: Alternative to ArgoCD for GitOps deployments.

### GitOps
A methodology for managing infrastructure and application deployments using Git as the single source of truth. Changes are made via Git commits, and automation syncs the cluster.

**Context**: Deployment strategy using ArgoCD/Flux to sync Helm charts from Git to AKS.

### Helm
A package manager for Kubernetes. Defines, installs, and upgrades Kubernetes applications using charts.

**Context**: Helm charts define application deployments for AKS.

### SignalR
A Microsoft library for building real-time web applications. Provides WebSocket abstraction with automatic fallbacks and scaling support via Redis backplane.

**Context**: Used for real-time signaling, presence, and chat in the platform.

---

## General Software Engineering Terms

### Async/Await
A programming pattern for handling asynchronous operations. Allows code to appear synchronous while executing asynchronously.

**Context**: All I/O operations (database, API calls) should use async/await patterns in .NET and Python.

### Correlation ID
A unique identifier attached to a request that flows through multiple services. Enables tracing requests across distributed systems.

**Context**: Used in structured logging to track requests across microservices.

### Dependency Injection
A design pattern where dependencies are provided to a class rather than created by it. Improves testability and flexibility.

**Context**: Used throughout .NET services for managing dependencies.

### Entity Framework Core
An object-relational mapping (ORM) framework for .NET. Simplifies database access and provides migration support.

**Context**: Used for database access in .NET services (PostgreSQL/Azure SQL).

### FastAPI
A modern, fast web framework for building APIs with Python. Provides automatic API documentation and type validation.

**Context**: Used for Python AI services (transcription, translation).

### Horizontal Scaling
A scaling strategy that adds more instances (servers, pods) to handle increased load. Also known as "scaling out."

**Context**: Microservices and SFU pods scale horizontally based on load.

### Load Balancing
The distribution of incoming network traffic across multiple servers to ensure no single server is overwhelmed.

**Context**: Load balancers distribute traffic across multiple SFU instances and signaling servers.

### Observability
The ability to understand a system's internal state by examining its outputs (logs, metrics, traces). Comprises logging, metrics, and distributed tracing.

**Context**: Comprehensive observability is required for production operations (Prometheus, Grafana, Azure Monitor).

### SLO (Service Level Objective)
A target level of service reliability or performance. Defines measurable goals (e.g., 99.9% availability, < 250ms latency).

**Context**: Defined for signaling availability (99.9%), media latency (< 250ms P95), and other critical metrics.

### SOLID Principles
Five design principles for object-oriented programming: Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, and Dependency Inversion.

**Context**: Coding standards require following SOLID principles in .NET code.

### Structured Logging
A logging approach that outputs logs in a structured format (JSON) rather than plain text. Enables better parsing, filtering, and analysis.

**Context**: All services use structured logging (JSON format) with correlation IDs.

### Vertical Scaling
A scaling strategy that increases the resources (CPU, memory) of existing instances. Also known as "scaling up."

**Context**: Less preferred than horizontal scaling for cloud-native applications.

---

## Additional Terms

### Ephemeral State
Temporary data that exists only during a session and is not persisted. Examples include active room participants, current SFU assignments, and presence information.

**Context**: Stored in Redis, not in persistent databases.

### Persistent State
Data that must be stored permanently, such as user profiles, meeting metadata, and historical records.

**Context**: Stored in PostgreSQL or Azure SQL Database.

### QoS (Quality of Service)
Metrics that measure the quality of a communication session, such as latency, packet loss, jitter, and bandwidth.

**Context**: Monitored for media streams to ensure acceptable quality (< 250ms latency, < 2% packet loss).

### RTT (Round-Trip Time)
The time it takes for a packet to travel from source to destination and back. A key metric for network performance.

**Context**: Monitored to detect network issues and adjust quality accordingly.

---

## How to Use This Glossary

1. **Search by Category**: Terms are organized by domain (WebRTC, Architecture, Security, etc.) for easy browsing.

2. **Context-Aware Definitions**: Each term includes context specific to the Smart Video Communications platform.

3. **Cross-References**: Related terms are mentioned within definitions to help understand connections.

4. **Documentation References**: Terms link back to relevant sections in the main documentation.

---

**Last Updated**: 29 November 2025  
**Maintained By**: Smart Video Communications Documentation Team

