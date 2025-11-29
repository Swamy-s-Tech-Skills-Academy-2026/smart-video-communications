# Cursor Rules for Smart Video Communications

## Project Overview

Smart Video Communications is a cloud-native video conferencing platform built with:

- **.NET 10** for signaling and APIs
- **.NET Aspire 13** for orchestration
- **Python** for AI services
- **WebRTC** for real-time media
- **Azure AKS** for infrastructure

The platform supports real-time audio/video, SFU integration, recording, transcription, translation, and AI-powered meeting insights.

## Documentation Structure

The project follows a structured documentation flow:

1. **01_Requirements.md** - Functional and non-functional requirements, SLOs
2. **02_System-Design-Overview.md** - High-level architecture, WebRTC fundamentals
3. **03_Detailed-Design.md** - Complete technical specifications (APIs, data models, security, workflows)
4. **04_Implementation-Plan.md** - Technology stack, microservices architecture, CI/CD

**Always reference these documents when making architectural or design decisions.**

## Technology Stack

### Control Plane (Signaling & APIs)

- **Language/Framework**: .NET 10 (ASP.NET Core)
- **Orchestration**: .NET Aspire 13
- **Real-Time Engine**: SignalR
- **Database**: Azure SQL Database or PostgreSQL (metadata), Redis (ephemeral state)
- **Authentication**: Azure AD / B2C (OAuth2/OIDC)

### Media Plane

- **SFU**: Mediasoup (Node.js/C++) on Kubernetes
- **TURN Server**: Coturn on VM Scale Sets
- **Protocols**: WebRTC (ICE, DTLS, SRTP) over UDP (primary), TCP/TLS (fallback)

### AI Services

- **Runtime**: Python 3.11+
- **Framework**: FastAPI
- **Libraries**: `azure-cognitiveservices-speech`, `openai`, `langchain`
- **Deployment**: Kubernetes Pods (autoscaled)

### Infrastructure

- **Orchestration**: Azure Kubernetes Service (AKS)
- **IaC**: Bicep or Terraform (via Azure Developer CLI)
- **Secrets**: Azure Key Vault with Managed Identities

## Architecture Patterns

### Microservices

The system is decomposed into domain-specific services:

- **User Service**: Auth, Profiles, Tenant Management
- **Meeting Service**: Scheduling, Room Lifecycle, Settings
- **Signaling Service**: Real-time negotiation, Presence
- **Media Controller**: SFU allocation, Load Balancing
- **Recording Service**: Stream ingestion, Transcoding, Uploads
- **Notification Service**: Email, Push, SMS
- **Analytics Service**: Telemetry ingestion, Aggregation

### Scalability Patterns

- **Small groups (≤5)**: P2P Mesh topology
- **Medium groups (5-50)**: SFU topology
- **Large groups (50+)**: SFU with simulcast
- **Live streaming**: CDN-based (HLS/DASH) for millions of viewers

### Security

- **Zero Trust** model - every connection authenticated
- **TLS 1.3** for all signaling
- **DTLS/SRTP** for media encryption
- **Optional E2EE** for end-to-end encryption
- **WAF** protection via Azure Front Door

## Key Requirements

### Performance

- Media latency (P95): < 250ms
- Join time: < 2 seconds
- Packet loss: < 2%
- Signaling availability: 99.9%

### Scalability

- Initial: 2,000 concurrent users per hour
- Future: 1 billion users, 100 million daily calls
- Auto-scaling based on load

### Security & Compliance

- GDPR/CCPA compliant
- Explicit consent for recording/AI processing
- Data residency controls
- Audit logging for all critical events

## Coding Standards

### .NET Code

- Use **.NET 10** features and patterns
- Follow **ASP.NET Core** best practices
- Use **SignalR** for real-time communication
- Implement proper **async/await** patterns
- Use **dependency injection** throughout
- Follow **SOLID principles**

### Python Code (AI Services)

- Use **Python 3.11+**
- **FastAPI** for API endpoints
- **Type hints** for all functions
- **Async/await** for I/O operations
- Proper **error handling** and logging

### API Design

- RESTful APIs for management operations
- WebSocket/SignalR for real-time signaling
- Follow OpenAPI/Swagger specifications
- Version APIs (v1, v2, etc.)
- Implement rate limiting

### Database

- Use **Entity Framework Core** for .NET
- **PostgreSQL** or **Azure SQL** for metadata
- **Redis** for ephemeral state (room participants, presence)
- Implement proper **migrations**
- Use **connection pooling**

### Error Handling

- Use structured exception handling
- Return appropriate HTTP status codes
- Log errors with correlation IDs
- Implement retry policies for transient failures
- Provide meaningful error messages (without exposing internals)

### Logging

- Use structured logging (JSON format)
- Include correlation IDs for request tracking
- Log levels: DEBUG, INFO, WARN, ERROR
- Use **Serilog** or similar for .NET
- Centralized logging via Azure Monitor

## File Structure Expectations

```
smart-video-communications/
├── docs/                    # Documentation
│   ├── 01_Requirements.md
│   ├── 02_System-Design-Overview.md
│   ├── 03_Detailed-Design.md
│   ├── 04_Implementation-Plan.md
│   └── Implementation-Readiness-Assessment.md
├── src/                     # Source code
│   ├── Services/           # Microservices
│   ├── Shared/             # Shared libraries
│   └── Infrastructure/     # Infrastructure code
├── tests/                   # Test projects
├── .cursor/                 # Cursor rules
└── README.md
```

## Development Guidelines

### When Implementing Features

1. **Check requirements** in `docs/01_Requirements.md`
2. **Review architecture** in `docs/02_System-Design-Overview.md`
3. **Follow detailed design** in `docs/03_Detailed-Design.md`
4. **Use tech stack** from `docs/04_Implementation-Plan.md`

### When Creating APIs

- Define endpoints in `03_Detailed-Design.md` Part A
- Use proper HTTP methods (GET, POST, PUT, DELETE)
- Include request/response schemas
- Implement authentication/authorization
- Add rate limiting
- Document with OpenAPI/Swagger

### When Working with Data

- Follow data models in `03_Detailed-Design.md` Part F-A
- Use Redis for ephemeral state (room participants, presence)
- Use PostgreSQL/SQL for persistent data
- Implement proper indexes
- Consider sharding for scale

### When Implementing Real-Time Features

- Use **SignalR** for signaling
- Follow WebSocket events in `03_Detailed-Design.md` Part F-B
- Implement proper connection management
- Handle reconnection scenarios
- Use Redis backplane for SignalR scaling

### When Working with Media

- Use **WebRTC** for peer-to-peer connections
- Implement **SFU** for larger groups
- Support **STUN/TURN** for NAT traversal
- Implement **HTTP tunneling** as fallback
- Handle dynamic switching (P2P ↔ SFU)

### When Implementing AI Features

- Use **Python** workers for AI processing
- Stream audio to Azure Speech API
- Implement real-time transcription
- Support multiple languages
- Handle consent before processing

## Testing Requirements

### Unit Tests

- Minimum 80% code coverage
- Test all business logic
- Mock external dependencies
- Use xUnit for .NET, pytest for Python

### Integration Tests

- Test API endpoints
- Test database interactions
- Test SignalR hubs
- Test Redis operations

### Load Tests

- Use Python + aiortc for synthetic load testing
- Test with thousands of concurrent users
- Verify SFU autoscaling
- Measure latency and packet loss

### Chaos Engineering

- Test pod failures
- Test network latency/jitter
- Test Redis outages
- Verify "Make before Break" logic

## Deployment

### CI/CD

- Use **GitHub Actions** for CI/CD
- **GitOps** approach (ArgoCD/Flux)
- **Canary deployments** (5% traffic initially)
- Container scanning (Trivy/Prisma)
- Static analysis (SonarQube)

### Infrastructure

- Use **Bicep** or **Terraform** for IaC
- Deploy to **Azure Kubernetes Service (AKS)**
- Use **Azure Container Registry (ACR)** for images
- Implement **Horizontal Pod Autoscaler (HPA)**
- Use **Azure Key Vault** for secrets

## Important Constraints

1. **Scale**: Support 2,000 concurrent users initially, scale to 1B users
2. **Latency**: Media latency must be < 250ms (P95)
3. **Availability**: 99.9% for signaling, 99.99% for core services
4. **Compliance**: Must comply with GDPR/CCPA
5. **Cost**: Use tiered storage (Hot → Cool → Archive) to control costs

## Common Patterns

### Authentication Flow

1. User logs in via Azure AD/B2C
2. Gets ID Token
3. Exchanges for short-lived Join Token (5-10 minutes)
4. Uses Join Token to connect to meeting

### Meeting Join Flow

1. Client requests join via REST API
2. Server validates token and capacity
3. Returns signaling token and ICE servers
4. Client connects to SignalR hub
5. Client establishes WebRTC connection (P2P or SFU)

### Recording Flow

1. SFU streams RTP packets to Logger Service
2. Logger buffers and writes chunks to temp storage
3. On meeting end, File Creator merges chunks
4. FFmpeg transcodes to final MP4
5. Uploads to permanent storage (Blob)
6. Updates database with recording URL

## Best Practices

1. **Always check documentation** before implementing features
2. **Follow the architecture** defined in design documents
3. **Use the specified technology stack**
4. **Implement proper error handling** and logging
5. **Write tests** for all new code
6. **Document APIs** with OpenAPI/Swagger
7. **Use async/await** for I/O operations
8. **Implement retry policies** for transient failures
9. **Monitor performance** and set up alerts
10. **Follow security best practices** (Zero Trust, encryption)

## Questions to Ask

When implementing features, always consider:

- Does this meet the requirements in `01_Requirements.md`?
- Does this follow the architecture in `02_System-Design-Overview.md`?
- Are there detailed specs in `03_Detailed-Design.md`?
- Is this using the tech stack from `04_Implementation-Plan.md`?
- Does this handle errors gracefully?
- Is this properly logged and monitored?
- Does this meet security requirements?
- Is this compliant with GDPR/CCPA?

---

**Remember**: This is an enterprise-grade, production-ready system. Always prioritize:

1. **Reliability** - System must be highly available
2. **Performance** - Low latency is critical for real-time media
3. **Security** - Zero Trust, encryption, compliance
4. **Scalability** - Must handle massive scale
5. **Observability** - Comprehensive monitoring and logging
