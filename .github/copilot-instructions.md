# GitHub Copilot Instructions

This repository contains the source code and documentation for the **Smart Video Communications** platform.

## Project Context

- **Domain**: Real-time video conferencing, AI-powered insights, and collaboration.
- **Architecture**: Cloud-native microservices on Azure.
- **Key Technologies**:
  - **Control Plane**: .NET 10, ASP.NET Core, .NET Aspire 13.
  - **Media Plane**: WebRTC, Mediasoup (Node.js/C++), Coturn.
  - **AI Services**: Python (FastAPI), Azure AI Speech, Azure OpenAI.
  - **Infrastructure**: Azure Kubernetes Service (AKS), Azure SQL, Redis, Azure Blob Storage.

## Coding Guidelines

### General
- Follow **Clean Architecture** principles.
- Ensure all public APIs are documented with XML comments (C#) or Docstrings (Python).
- Use **Async/Await** patterns for all I/O bound operations.

### .NET / C#
- Target **.NET 10**.
- Use **.NET Aspire 13** for orchestration and service defaults.
- Prefer `record` types for DTOs and immutable data structures.
- Use **SignalR** for real-time signaling logic.

### Python (AI Workers)
- Use **FastAPI** for service endpoints.
- Type hinting is mandatory.
- Use `aiohttp` or `httpx` for asynchronous HTTP requests.

### Frontend / Client
- Use TypeScript and React (if applicable).
- Manage WebRTC state carefully (handle connection drops, ICE restarts).

## Documentation
- Update `docs/` when architectural decisions change.
- Use Mermaid diagrams for complex flows.

## Testing
- Write unit tests for business logic.
- Use integration tests for API endpoints.
