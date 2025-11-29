---
learning_level: "Advanced"
prerequisites: ["./02_Detailed-Design-Part1-C.md"]
estimated_time: "20 minutes"
learning_objectives:
  - "Define Service Level Objectives (SLOs) for real-time media"
  - "Design operational runbooks for common failure scenarios"
  - "Implement comprehensive observability for SFU and Signaling"
related_topics:
  builds_upon: ["./02_Detailed-Design-Part1-C.md"]
  enables: ["./02_Detailed-Design-Part1-E.md"]
---

# Detailed Design: Operational Excellence

## 1. Service Level Objectives (SLOs)

For a real-time system, availability alone is insufficient. We must track latency
and quality.

| Metric | Target (SLO) | Measurement Window |
| :--- | :--- | :--- |
| **Signaling Availability** | 99.9% | Monthly |
| **Media Latency (P95)** | < 250 ms | Rolling 5-min |
| **Packet Loss** | < 2% | Per Session |
| **Join Success Rate** | > 99% | Daily |
| **TURN Success Rate** | > 99% | Daily |

**Alerting Thresholds**:

* **Critical**: SFU CPU > 80%, Packet Loss > 5% (sustained 1 min).
* **Warning**: TURN Bandwidth > 70% capacity, Join Latency > 2s.

## 2. Observability Strategy

We employ a multi-layer monitoring approach to capture system health and user experience.

### A. Client-Side Telemetry

* **Mechanism**: WebRTC `getStats()` API.
* **Metrics**: RTT (Round Trip Time), Jitter, Packet Loss, Bitrate, Frame Rate.
* **Transport**: Sent via DataChannel to Signaling Server every 10-30 seconds.

### B. Server-Side Metrics (Prometheus)

* **Signaling**: Active connections, Message rate, Redis latency, Token validation
  failures.
* **SFU**: Aggregate forwarding bitrate, Active publishers/subscribers, CPU/Memory
  usage.
* **TURN**: Egress bandwidth, Active allocations, Auth failures.

### C. Distributed Tracing (OpenTelemetry)

* Trace the full `join_room` flow: Client → Front Door → API Gateway →
  Signaling → Redis → SFU.
* Identify bottlenecks in room creation or media negotiation.

### D. Local Observability (.NET Aspire Dashboard)

* **Tool**: **.NET Aspire Dashboard**.
* **Usage**: Provides a real-time view of logs, traces, and metrics during local development and testing.
* **Benefit**: Simplifies debugging of distributed microservices (Signaling, API, Workers) without needing a full cloud setup.

## 3. Operational Runbooks

Standardized procedures for incident response.

### Playbook: SFU Saturation

**Trigger**: SFU Node CPU > 85% or Packet Loss > 3%.

1. **Automated Action**: Horizontal Pod Autoscaler (HPA) adds new SFU pods.
2. **Traffic Shift**: Signaling Server stops assigning new rooms to the saturated
   region/node.
3. **Investigation**: Check for "noisy neighbor" rooms (e.g., 50+ active speakers).
4. **Mitigation**: Enforce stricter bitrate limits on existing publishers in that
   node.

### Playbook: High TURN Usage

**Trigger**: TURN Egress Bandwidth > Threshold.

1. **Analysis**: Identify if a specific region or ISP is forcing relay (Symmetric
   NAT).
2. **Scaling**: Scale out TURN VM Scale Set (VMSS).
3. **Optimization**: Verify STUN configuration on clients (ensure P2P is attempted
   first).

### Playbook: Recording Failure

**Trigger**: Recording worker error rate > 1%.

1. **Fallback**: Enable client-side local recording (if supported) or audio-only
   backup.
2. **Recovery**: Re-queue failed chunks for post-processing.
3. **Notification**: Alert users of potential delay in recording availability.
