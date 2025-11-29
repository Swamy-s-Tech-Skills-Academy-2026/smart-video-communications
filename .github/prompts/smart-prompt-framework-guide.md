# S.M.A.R.T. Prompt Framework for GitHub Copilot Coding Agents

**Smart Video Communications Edition** - Framework for creating high-quality coding agent instructions aligned with enterprise-grade video conferencing platform development standards.

---

## 🎯 **The S.M.A.R.T. Framework**

Use this framework to create highly effective coding agent instructions:

```text
S - Specific Role Definition (Senior .NET Developer, WebRTC Engineer, AI Services Developer, etc.)
M - Mission-Critical Requirements (What must be accomplished with measurable outcomes)
A - Audience-Aware Communication (Team expertise level, architectural maturity, domain context)
R - Response Format Control (Code structure, architecture patterns, documentation style)
T - Task-Oriented Constraints (Technology stack, architectural patterns, forbidden actions)
```

---

## 🏛️ **Smart Video Communications Alignment**

When creating prompts, consider:

- **Domain Context**: Real-time video conferencing, WebRTC media streaming, AI-powered transcription/translation
- **Architecture Patterns**: Microservices, SFU topology, P2P mesh, event-driven architecture
- **Technology Stack**: .NET 10, .NET Aspire 13, SignalR, WebRTC, Python (FastAPI), Azure AKS
- **Performance Requirements**: Media latency < 250ms (P95), join time < 2 seconds, packet loss < 2%
- **Scalability**: Support 2,000 concurrent users initially, scale to 1B users

## 🏗️ **Advanced Problem Statement Template**

Use this enhanced template for coding agent tasks:

```markdown
## ROLE DEFINITION

You are a [Specific Role] specializing in [Technology Stack] with expertise in [Domain Areas]

## MISSION

[Clear, specific objective with measurable outcomes]

## CONTEXT

[Brief overview of current situation and progress made]

## CURRENT STATUS

- **Progress Made**: [Specific achievements and metrics]
- **Main Issue**: [Root cause analysis]
- **Files Affected**: [List specific files]

## REMAINING WORK

### 1. [Priority Task Name] (Priority N)

- **Problem**: [Specific technical issue]
- **Current Error**: [Exact error messages]
- **Solution Approach**: [Concrete implementation steps]
- **Files to Modify**: [Specific file paths]

## TECHNICAL CONSTRAINTS

- **🚨 CRITICAL**: [Non-negotiable requirements]
- **Framework**: [Technology stack requirements]
- **Dependencies**: [Package/version constraints]

## RESPONSE FORMAT REQUIREMENTS

- [Specific code structure expectations]
- [Documentation requirements]
- [Testing requirements]
- [Build/deployment considerations]

## WHAT NOT TO DO

- ❌ [Explicit forbidden actions with reasoning]

## WHAT TO DO

- ✅ [Explicit required actions with priority]

## SUCCESS CRITERIA

[Measurable outcomes with acceptance criteria]

## QUALITY STANDARDS

- [Code quality requirements]
- [Performance expectations]
- [Security considerations]
- [Maintainability standards]
```

## 🎭 **Role-Based Specialization Examples**

### **For .NET/SignalR Development:**

```markdown
ROLE: You are a Senior .NET Developer specializing in .NET 10, ASP.NET Core, SignalR, and real-time signaling architecture

EXPERTISE FOCUS: SignalR hubs, WebSocket connection management, Redis backplane scaling, async/await patterns, .NET Aspire 13 orchestration

OUTPUT REQUIREMENTS: Production-ready C# code with comprehensive error handling, SignalR hub implementations, unit tests with proper mocking patterns, and enterprise-grade documentation

MANDATORY VALIDATION:
- ✅ dotnet build --configuration Release (0 errors required)
- ✅ dotnet test --configuration Release (0 failures required)
- ✅ SignalR connection handling tested with Redis backplane
- ✅ Async/await patterns properly implemented
```

### **For WebRTC/Media Plane Development:**

```markdown
ROLE: You are a Senior WebRTC Engineer specializing in real-time media streaming, SFU architecture, and WebRTC protocol implementation

EXPERTISE FOCUS: Mediasoup integration, ICE/DTLS/SRTP protocols, TURN server configuration, adaptive bitrate streaming, simulcast

OUTPUT REQUIREMENTS: Production-ready Node.js/C++ code for SFU, WebRTC client integration, media quality optimization, and comprehensive testing

MANDATORY VALIDATION:
- ✅ WebRTC connection establishment tested
- ✅ Media latency < 250ms (P95) verified
- ✅ Packet loss < 2% under load
- ✅ SFU scaling and load balancing validated
```

### **For Python AI Services Development:**

```markdown
ROLE: You are a Senior Python Developer specializing in FastAPI, Azure AI Speech services, and real-time AI processing

EXPERTISE FOCUS: FastAPI async endpoints, Azure Speech-to-Text streaming, real-time transcription, multi-language translation, consent handling

OUTPUT REQUIREMENTS: Production-ready Python 3.11+ code with type hints, async/await patterns, comprehensive error handling, and AI service integration

MANDATORY VALIDATION:
- ✅ FastAPI endpoints tested with async operations
- ✅ Azure Speech API integration validated
- ✅ Real-time transcription latency < 500ms
- ✅ Consent management properly implemented
```

### **For DevOps/Infrastructure:**

```markdown
ROLE: You are a DevOps Engineer specializing in Azure Kubernetes Service (AKS), .NET Aspire deployment, and CI/CD pipelines

EXPERTISE FOCUS: AKS cluster management, .NET Aspire AppHost orchestration, GitHub Actions workflows, Bicep/Terraform IaC, container orchestration

OUTPUT REQUIREMENTS: Build scripts, deployment configurations, infrastructure as code, monitoring setup, and GitOps workflows

MANDATORY VALIDATION:
- ✅ All build scripts execute successfully
- ✅ Infrastructure templates validate without errors
- ✅ Deployment processes complete end-to-end
- ✅ Canary deployment strategy implemented
```

### **For Enterprise Architecture & System Design:**

```markdown
ROLE: You are a Lead Enterprise Architect specializing in scalable microservices architecture, distributed systems design, and cloud-native video conferencing platforms

EXPERTISE FOCUS:
- SOLID principles and architectural patterns (layered, hexagonal, event-driven)
- Microservices decomposition and domain-driven design
- Cloud-native architecture and infrastructure as code
- System resilience, scalability, and observability
- Cross-cutting concerns: security, compliance, monitoring

OUTPUT REQUIREMENTS:
- Architecture Decision Records (ADRs) documenting trade-offs
- System design diagrams with components and integration points
- Reference implementations in .NET 10 and Python
- Migration strategies and cost optimization recommendations
- Security, compliance (GDPR/CCPA), and operational considerations

SUCCESS CRITERIA:
- ✅ Architecture aligns with organizational constraints
- ✅ Trade-offs clearly documented with reasoning
- ✅ Scalability and resilience characteristics defined
- ✅ Implementation examples demonstrate pattern application
- ✅ Performance targets met (latency, packet loss, availability)
```

## 🚨 **Critical Constraint Guidelines**

### **Framework/Package Versions:**

```markdown
- 🚨 CRITICAL: Use .NET 10 ONLY - DO NOT downgrade to .NET 8 or .NET 9
- 🚨 CRITICAL: Use .NET Aspire 13 for orchestration
- 🚨 CRITICAL: Use Python 3.11+ for AI services
- ❌ DO NOT modify Directory.Packages.props without explicit approval
- ❌ DO NOT downgrade package versions
- ❌ DO NOT use .NET 9 or earlier versions
```

### **File and Folder Naming:**

```markdown
- 🚨 CRITICAL: Never start file or folder names with `00_`
- ✅ REQUIRED: Always start numbered files/folders with `01_` or higher
- ✅ Use consistent numbering sequence: `01_`, `02_`, `03_`, etc.
- ❌ DO NOT create files like `00_Introduction.md` or `00_Workspace-Report.md`
- ✅ DO create files like `01_Requirements.md` or `01_Project-Setup.md`
```

### **Build Requirements:**

```markdown
When building, use: cd /path/to/project && dotnet build
Ensure all projects target net10.0 framework
For Python services, ensure Python 3.11+ is used
```

### **Architecture Constraints:**

```markdown
- ✅ MUST follow microservices architecture per docs/04_Implementation-Plan.md
- ✅ MUST use SignalR for real-time signaling (not raw WebSockets)
- ✅ MUST use Redis for ephemeral state (room participants, presence)
- ✅ MUST use PostgreSQL or Azure SQL for persistent metadata
- ❌ DO NOT create shared databases between microservices
- ❌ DO NOT bypass SignalR for signaling (use SignalR hubs)
```

## ✅ **Effective Instruction Patterns**

### **DO - Be Specific and Explicit:**

- ✅ "Create SignalR hub for meeting room signaling with Redis backplane support"
- ✅ "Implement WebRTC connection establishment with ICE candidate exchange via SignalR"
- ✅ "Add FastAPI endpoint for real-time transcription with Azure Speech API streaming"
- ✅ "Create Entity Framework Core migration for Meeting entity with proper indexes"

### **DON'T - Be Vague:**

- ❌ "Fix the signaling"
- ❌ "Make it work"
- ❌ "Update the code"
- ❌ "Add WebRTC support"

## 📝 **Constraint Language Examples**

### **Strong Constraint Language That Works:**

```markdown
🚨 ABSOLUTELY DO NOT modify Directory.Packages.props or downgrade any packages.

The following packages MUST remain at their current .NET 10 versions:
- Microsoft.AspNetCore.SignalR: Latest .NET 10 compatible version
- Microsoft.EntityFrameworkCore: Latest .NET 10 compatible version
- StackExchange.Redis: Latest version

CRITICAL: Any attempt to change these versions will require task restart.

🚨 CRITICAL: Never create files or folders starting with `00_`. Always use `01_` or higher.
```

### **Weak Language That Doesn't Work:**

```markdown
Please try to maintain .NET 10 compatibility
Prefer keeping current package versions
Try to avoid using 00_ prefix
```

## 🎯 **Advanced Prompt Design Patterns**

### **Multi-Layered Prompt Architecture:**

```markdown
SYSTEM LAYER:
You are a [Specialist Role] with expertise in [Technology Stack] and [Domain Expertise].

CONTEXT LAYER:  
[Project context: Smart Video Communications platform, current situation, business requirements]

TASK LAYER:
[Specific implementation task with clear deliverables]

SPECIFICATION LAYER:
[Detailed technical requirements, constraints, and acceptance criteria]
```

### **Conditional Logic for Complex Scenarios:**

```markdown
LOGIC FRAMEWORK:
IF issue_type == "signalr_scaling":
THEN approach: Implement Redis backplane for SignalR scaling
AND include: Connection management and reconnection handling

ELIF issue_type == "webrtc_connection":  
THEN approach: Implement ICE candidate exchange via SignalR
AND include: STUN/TURN server configuration and fallback logic

ELIF issue_type == "ai_transcription":
THEN approach: Stream audio to Azure Speech API with consent handling
AND include: Real-time transcription delivery via SignalR

ELIF issue_type == "sfu_allocation":
THEN approach: Implement Media Controller service for SFU allocation
AND include: Load balancing and autoscaling logic
```

### **Progressive Refinement Pattern:**

```markdown
BASE PROMPT: [Core role and task definition]

REFINEMENT 1: Add specific technical constraints (.NET 10, SignalR, WebRTC)
REFINEMENT 2: Define output format requirements (code structure, documentation)
REFINEMENT 3: Include quality standards and acceptance criteria (latency, packet loss)
REFINEMENT 4: Add monitoring and validation requirements (observability, testing)

FINAL VALIDATION: Ensure all constraints are explicitly stated
```

## 📊 **Output Format Control**

### **For Code Generation Tasks:**

```markdown
OUTPUT REQUIREMENTS:
- Production-ready code with comprehensive error handling
- Unit tests with proper mocking patterns (avoiding extension methods)
- XML documentation for public APIs
- Consistent code style following project conventions
- No explanatory comments in code - let the code be self-documenting
- Include integration points and dependency injection setup
- Follow async/await patterns for all I/O operations
- Implement structured logging with correlation IDs
```

### **For Infrastructure Tasks:**

```markdown
OUTPUT REQUIREMENTS:
- Valid configuration files (JSON/YAML/XML as specified)
- Infrastructure as Code (Bicep/Terraform) with proper variable definitions
- Deployment scripts with error handling and rollback procedures
- Monitoring and alerting configurations (Azure Monitor integration)
- Documentation with setup and troubleshooting guides
- .NET Aspire AppHost orchestration for local development
```

### **For Debugging Tasks:**

```markdown
OUTPUT REQUIREMENTS:
- Root cause analysis with supporting evidence
- Step-by-step resolution procedures
- Test cases to validate the fix
- Prevention strategies for similar issues
- Performance impact assessment (latency, packet loss)
- Observability improvements (logging, tracing)
```

## 🤖 **AI Services Evaluation & Observability Framework**

When designing AI service tasks, include evaluation and observability requirements:

### **AI Transcription Service Evaluation Template:**

```markdown
## EVALUATION FRAMEWORK

### Evaluation Metrics
- **Accuracy**: Transcription word error rate (target: <5%)
- **Latency**: End-to-end transcription delay (target: <500ms)
- **Language Support**: Multi-language transcription accuracy (target: >90% per language)
- **Consent Compliance**: 100% consent verification before processing

### Test Dataset
- **Size**: Minimum 50 diverse audio samples covering edge cases
- **Coverage**: Include normal speech, background noise, multiple languages, accents
- **Distribution**: Representative of production meeting scenarios

### Success Criteria
- Transcription accuracy exceeds target threshold
- Latency remains within acceptable range
- All languages supported meet accuracy targets
- Consent management properly enforced

### Evaluation Execution
- Run evaluations against complete test dataset
- Capture detailed metrics and failure analysis
- Document performance improvements and regressions
- Validate changes don't degrade existing functionality
```

### **AI Services Tracing & Observability Template:**

```markdown
## TRACING & OBSERVABILITY REQUIREMENTS

### Trace Coverage
- Audio stream ingestion and processing
- Transcription generation and delivery
- Translation service invocations
- Consent verification and logging
- Performance metrics (latency, accuracy, cost)

### Tracing Implementation
- Structured logging with correlation IDs
- OpenTelemetry instrumentation for observability
- Azure Monitor integration for metrics and logs
- Real-time monitoring dashboards

### Observable Signals
- Transcription latency and accuracy
- Service health and availability
- Token usage and cost tracking
- Error rates and failure patterns
- User satisfaction and feedback signals

### Analysis & Improvement
- Identify bottlenecks and failure modes
- Detect drift in transcription quality
- Measure business impact of improvements
- Plan optimizations based on observed patterns
```

## 🎯 **Success Indicators**

### **Agent is working correctly when:**

- ✅ It acknowledges constraints explicitly (.NET 10, file naming, architecture)
- ✅ It asks clarifying questions about boundaries
- ✅ It references documentation (docs/01_Requirements.md, etc.)
- ✅ It focuses on code changes following architectural patterns
- ✅ It provides detailed progress updates
- ✅ It implements proper async/await patterns
- ✅ It includes SignalR/WebRTC best practices

### **Agent needs restart when:**

- ❌ It immediately modifies forbidden files
- ❌ It changes package versions without asking
- ❌ It ignores explicit constraints (.NET 10, file naming)
- ❌ It creates files with `00_` prefix
- ❌ It takes wrong architectural approach (bypasses SignalR, creates shared DBs)
- ❌ It ignores performance requirements (latency, packet loss)

## 🔄 **Agent Restart Protocol**

### **When to restart the coding agent:**

- Agent modifies forbidden files (like Directory.Packages.props)
- Agent downgrades package versions or framework
- Agent changes target framework from .NET 10
- Agent creates files/folders with `00_` prefix
- Agent takes wrong architectural approach (bypasses SignalR, creates shared databases)
- Agent ignores performance requirements

### **How to restart:**

1. Close current pull request
2. Create new pull request with more explicit constraints
3. Include specific examples of what went wrong
4. Add stronger constraint language
5. Reference relevant documentation files

## 🏗️ **Smart Video Communications Architecture Patterns**

When tasks span multiple architectural domains, apply these patterns:

### **Pattern: Microservices Architecture for Video Conferencing**

```markdown
ARCHITECTURAL PATTERN: Event-Driven Microservices for Real-Time Media

SERVICES INVOLVED:
- User Service: Auth, Profiles, Tenant Management
- Meeting Service: Scheduling, Room Lifecycle, Settings
- Signaling Service: Real-time negotiation, Presence
- Media Controller: SFU allocation, Load Balancing
- Recording Service: Stream ingestion, Transcoding
- AI Services: Transcription, Translation, Insights

IMPLEMENTATION REQUIREMENTS:
- Independent deployment and scaling per service
- Asynchronous communication via events (Azure Service Bus)
- Database per service (no shared databases)
- SignalR for real-time signaling (not raw WebSockets)
- Redis for ephemeral state (room participants, presence)
- PostgreSQL/Azure SQL for persistent metadata
- Distributed tracing across services
- Monitoring and alerting per service

QUALITY GATES:
✅ Services can be deployed independently
✅ Event contracts validated before deployment
✅ Failure scenarios handled gracefully (Make before Break)
✅ End-to-end tracing captures cross-service flows
✅ No database coupling between services
✅ Performance targets met (latency < 250ms, packet loss < 2%)
```

### **Pattern: Real-Time Signaling with SignalR**

```markdown
ARCHITECTURAL PATTERN: SignalR Hub Architecture for WebRTC Signaling

CHARACTERISTICS:
- SignalR hubs for WebSocket abstraction
- Redis backplane for horizontal scaling
- Connection management and reconnection handling
- ICE candidate exchange via SignalR
- SDP offer/answer exchange
- Presence and participant management

IMPLEMENTATION REQUIREMENTS:
- SignalR hub per domain (MeetingHub, SignalingHub)
- Redis backplane configuration for scaling
- Connection state management
- Reconnection logic with exponential backoff
- Message serialization (JSON)
- Authentication/authorization per connection

QUALITY GATES:
✅ SignalR connections scale horizontally with Redis
✅ Reconnection handled gracefully
✅ ICE candidates exchanged successfully
✅ Presence updates in real-time
✅ Connection failures don't break meetings
```

### **Pattern: WebRTC Media Plane Architecture**

```markdown
ARCHITECTURAL PATTERN: SFU (Selective Forwarding Unit) for Media Routing

CHARACTERISTICS:
- Mediasoup for SFU implementation
- Dynamic topology switching (P2P ↔ SFU)
- Adaptive bitrate streaming
- Simulcast for large groups
- TURN server for NAT traversal

IMPLEMENTATION REQUIREMENTS:
- Mediasoup integration on Kubernetes
- Media Controller for SFU allocation
- Load balancing across SFU instances
- ICE/DTLS/SRTP protocol implementation
- HTTP tunneling fallback for restrictive networks
- Quality metrics collection (latency, packet loss)

QUALITY GATES:
✅ Media latency < 250ms (P95)
✅ Packet loss < 2%
✅ Dynamic topology switching works seamlessly
✅ SFU autoscaling responds to load
✅ TURN server handles NAT traversal
```

## 📋 **Universal PR Success Template**

Include this template in EVERY coding agent PR for consistent validation:

```markdown
## 🎯 MANDATORY SUCCESS CRITERIA (NON-NEGOTIABLE)

### Build Requirements

```powershell
# MUST PASS: Full solution build with zero errors
dotnet build --configuration Release --verbosity normal
# Expected Result: "Build succeeded. 0 Error(s)"
```

### Test Requirements

```powershell
# MUST PASS: All existing unit tests
dotnet test --configuration Release --logger "console;verbosity=normal"
# Expected Result: "Test Run Successful. Total tests: X, Passed: X, Failed: 0, Skipped: 0"
```

### Performance Requirements (if applicable)

- ✅ Media latency < 250ms (P95) verified
- ✅ Packet loss < 2% under load
- ✅ Join time < 2 seconds
- ✅ Signaling availability 99.9%

## 📋 FINAL CHECKLIST

Before marking this PR ready for review:

- [ ] ✅ `dotnet build` succeeds with 0 errors across entire solution
- [ ] ✅ `dotnet test` passes with 0 failures across all test projects
- [ ] ✅ All original issues resolved completely
- [ ] ✅ No package downgrades or framework changes (.NET 10 maintained)
- [ ] ✅ No files/folders created with `00_` prefix
- [ ] ✅ All existing functionality preserved
- [ ] ✅ Production-ready error handling implemented
- [ ] ✅ Async/await patterns properly used
- [ ] ✅ Structured logging with correlation IDs implemented
- [ ] ✅ Performance targets met (if applicable)

**CRITICAL**: Do not mark this PR as ready for review until ALL build and test validations pass successfully.

---

## 🚀 **Smart Video Communications S.M.A.R.T. Example**

```markdown
ROLE: You are a Senior .NET Developer specializing in real-time video conferencing systems, SignalR signaling architecture, and .NET Aspire microservices orchestration

MISSION: Implement the Signaling Service - an enterprise-grade, real-time signaling service for WebRTC connection establishment using .NET 10, SignalR, Redis backplane, and Azure AKS deployment

AUDIENCE: Development team with expertise in:
- .NET 10, ASP.NET Core, and SignalR
- Real-time communication patterns
- Azure cloud services (Redis, Service Bus, Key Vault)
- Microservices architecture and .NET Aspire 13

RESPONSE FORMAT:
- Production-ready C# code with comprehensive error handling
- SignalR hub implementation with Redis backplane
- Azure Bicep infrastructure templates for automated deployment
- .NET Aspire AppHost orchestration for local development
- Enterprise-grade documentation with architecture diagrams

TASK CONSTRAINTS:
- 🚨 CRITICAL: Use .NET 10 ONLY - DO NOT downgrade to .NET 9 or .NET 8
- 🚨 CRITICAL: Use SignalR for signaling (not raw WebSockets)
- 🚨 CRITICAL: Never create files/folders with `00_` prefix - always use `01_` or higher
- Architecture: Microservices with SignalR hubs, Redis backplane, PostgreSQL for metadata
- Quality Standards: Zero build warnings, 100% test pass rate, latency < 250ms
- Technology Stack: .NET 10, .NET Aspire 13, SignalR, Redis, PostgreSQL
- Performance: Signaling availability 99.9%, connection establishment < 2 seconds
```

## 📚 **Best Practices Summary**

1. **Be Specific**: Define exact roles, technologies, and constraints
2. **Set Clear Boundaries**: Use strong constraint language
3. **Define Success**: Include measurable outcomes and validation steps
4. **Control Output**: Specify exactly what format and quality you expect
5. **Plan for Failure**: Include restart protocols and troubleshooting
6. **Validate Everything**: Always include build and test requirements
7. **Document Thoroughly**: Ensure all decisions and constraints are recorded
8. **Align with Architecture**: Reference documentation (docs/01-04) and architectural patterns
9. **Enable Observability**: Include tracing and evaluation requirements
10. **Performance First**: Always consider latency, packet loss, and scalability requirements
11. **File Naming**: Never use `00_` prefix, always start with `01_` or higher

---

## ⚡ **Quick Reference Checklist**

Use this checklist before submitting any coding agent task:

### **Role Definition**

- [ ] Specific role/expertise clearly stated
- [ ] Technology stack and frameworks identified (.NET 10, SignalR, WebRTC, etc.)
- [ ] Expected audience knowledge level documented
- [ ] Domain context provided (video conferencing, real-time media)

### **Task Clarity**

- [ ] Mission and objectives clearly defined
- [ ] Success criteria are measurable (latency, packet loss, availability)
- [ ] Scope is appropriately sized
- [ ] Priority and sequencing defined

### **Technical Requirements**

- [ ] Framework and version constraints specified (.NET 10, Python 3.11+)
- [ ] Architectural patterns identified (microservices, SignalR, SFU)
- [ ] Dependencies listed explicitly
- [ ] Integration points documented (SignalR, Redis, PostgreSQL, Azure services)

### **Constraints & Boundaries**

- [ ] Forbidden actions explicitly listed (❌)
- [ ] Required actions explicitly listed (✅)
- [ ] File modification boundaries defined
- [ ] File naming rules specified (no `00_` prefix)
- [ ] Architectural decision constraints included

### **Quality & Validation**

- [ ] Code quality standards specified
- [ ] Build/test requirements included
- [ ] Performance expectations defined (latency < 250ms, packet loss < 2%)
- [ ] Security considerations addressed (Zero Trust, encryption, GDPR/CCPA)

### **AI Services Specifics** (if applicable)

- [ ] Evaluation metrics defined (transcription accuracy, latency)
- [ ] Test dataset requirements specified
- [ ] Tracing requirements included
- [ ] Consent management documented

### **Output Expectations**

- [ ] Code format and style specified
- [ ] Documentation requirements defined
- [ ] Testing approach specified
- [ ] Deployment considerations included (.NET Aspire, AKS)

---

## 📋 **FINAL VALIDATION CHECKLIST**

Before submitting ANY coding agent PR or task completion:

- [ ] ✅ All technical constraints acknowledged (.NET 10, file naming, architecture)
- [ ] ✅ Success criteria clearly measurable
- [ ] ✅ Build passes without errors/warnings
- [ ] ✅ Tests pass with 0 failures
- [ ] ✅ No forbidden files modified
- [ ] ✅ No files/folders created with `00_` prefix
- [ ] ✅ Architectural patterns applied correctly (SignalR, microservices)
- [ ] ✅ Documentation is complete and accurate
- [ ] ✅ Performance targets met (if applicable)
- [ ] ✅ Code review readiness criteria met

---

## 🎓 **Smart Video Communications Documentation Integration**

Always reference the project documentation when creating prompts:

### **Documentation Flow:**

1. **docs/01_Requirements.md** - Functional and non-functional requirements, SLOs
2. **docs/02_System-Design-Overview.md** - High-level architecture, WebRTC fundamentals
3. **docs/03_Detailed-Design.md** - Complete technical specifications (APIs, data models, security, workflows)
4. **docs/04_Implementation-Plan.md** - Technology stack, microservices architecture, CI/CD

### **When Creating Prompts:**

- ✅ Reference specific documentation sections
- ✅ Align with documented architecture patterns
- ✅ Follow technology stack from docs/04_Implementation-Plan.md
- ✅ Meet requirements from docs/01_Requirements.md
- ✅ Implement designs from docs/03_Detailed-Design.md

This framework ensures consistent, high-quality results from GitHub Copilot coding agents while preventing common issues and maintaining enterprise-grade architectural standards for the Smart Video Communications platform.
