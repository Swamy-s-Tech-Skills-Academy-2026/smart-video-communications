---
learning_level: "Advanced"
prerequisites: ["./01_System-Design-Overview-Part1-B.md"]
estimated_time: "25 minutes"
learning_objectives:
  - "Address concurrency challenges with WebRTC and SignalR"
  - "Optimize AI latency using streaming APIs"
  - "Implement data privacy and consent workflows"
related_topics:
  builds_upon: ["./01_System-Design-Overview-Part1-B.md"]
  enables: ["./01_System-Design-Overview-Part1-D.md"]
---

# System Design: AI-Enhanced Video Conferencing - Part 1-C

## 4. Key Discussion Points (Interview Focus)

### Handling Concurrency & Real-Time Media

* **Challenge**: Supporting 2,000 concurrent users with low latency.
* **Solution**:
  * **WebRTC**: Use WebRTC for peer-to-peer (P2P) media streaming for small groups (1:1 or small teams).
  * **SFU (Selective Forwarding Unit)**: For larger meetings, use an SFU (like Mediasoup or Jitsi) to route media streams centrally. This reduces client bandwidth.
  * **SignalR / WebSockets**: For signaling (setting up the call) and real-time chat/events.
  * **Horizontal Scaling**: Stateless signaling servers scaled behind a load balancer. Sticky sessions may be needed for WebSocket connections.

### AI Latency & Streaming

* **Challenge**: Real-time translation must not introduce noticeable lag (lip-sync issues).
* **Solution**:
  * **Streaming APIs**: Use gRPC or WebSocket-based streaming APIs for Speech-to-Text (STT). Do not wait for a full sentence to finish; process audio chunks (e.g., 100ms).
  * **Edge Deployment**: Deploy AI models closer to the user (if using custom models) or use region-specific Azure AI endpoints.
  * **Optimistic UI**: Display tentative transcripts immediately and correct them as the model refines confidence.

### Data Privacy & Consent

* **Challenge**: AI processing involves sending voice/text to cloud services.
* **Solution**:
  * **Explicit Consent**: "This meeting is being recorded/analyzed." Pop-up must block join until accepted.
  * **PII Redaction**: Use Azure AI Content Safety to detect and redact PII (names, phone numbers) from transcripts *before* storage.
  * **Ephemeral Processing**: Ensure audio sent to AI services is not stored by the provider (zero data retention policy).

### Storage Costs & Lifecycle

* **Challenge**: Storing HD video recordings is expensive.
* **Solution**:
  * **Tiering**:
    * **Hot**: First 30 days (instant access).
    * **Cool**: 30-90 days (slightly lower cost, millisecond access).
    * **Archive**: > 90 days (lowest cost, hours to retrieve).
  * **Transcoding**: Compress videos after the meeting ends (e.g., convert raw stream to H.264/H.265).
  * **Retention Policy**: Auto-delete recordings after X days based on tenant configuration.
