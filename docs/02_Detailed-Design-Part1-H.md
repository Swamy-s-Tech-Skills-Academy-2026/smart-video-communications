---
learning_level: "Advanced"
prerequisites: ["./02_Detailed-Design-Part1-G.md"]
estimated_time: "25 minutes"
learning_objectives:
  - "Design network resilience mechanisms using HTTP Tunneling"
  - "Perform advanced capacity planning for massive scale (1B users)"
  - "Architect a Big Data pipeline for Quality of Service (QoS) analytics"
related_topics:
  builds_upon: ["./02_Detailed-Design-Part1-G.md"]
  enables: ["./03_Implementation-Plan.md"]
---

# Detailed Design: Infrastructure, Scalability & Analytics - Part 1-H

## 1. Network Resilience: HTTP Tunneling

While UDP is the preferred transport for real-time media, enterprise firewalls and strict proxies often block non-standard ports or UDP traffic. To ensure **100% connectivity**, we must implement a fallback mechanism.

### The Fallback Strategy

1. **Primary**: UDP (SRTP) via STUN.
2. **Secondary**: TCP (SRTP) via TURN (Port 443).
3. **Tertiary (Last Resort)**: **HTTP Tunneling**.

### HTTP Tunneling Mechanism

* **Concept**: Encapsulate media and signaling packets within standard HTTP/HTTPS requests.
* **Implementation**:
  * The client opens a persistent HTTP connection (or uses long-polling/WebSockets over 443).
  * Media packets are base64 encoded (or binary) and sent as the HTTP body.
  * The server (or a specialized Gateway) unwraps the packets and forwards them to the SFU/Media Server.
* **Trade-offs**:
  * **Pros**: Works through almost any firewall or proxy.
  * **Cons**: High overhead (HTTP headers), TCP Head-of-Line blocking (latency spikes), increased CPU usage.

## 2. Advanced Capacity Planning (The "Billion User" Scenario)

Designing for massive scale requires rigorous math. Let's estimate the capacity for a system serving **1 Billion Users** with **100 Million Daily Group Calls**.

### A. Traffic Estimation

* **Daily Calls**: 100 Million.
* **Peak Traffic**: Assuming a 10-hour active window.
  * $100,000,000 \text{ calls} / (10 \times 3600) \approx 2,777 \text{ calls/sec}$.
  * If each call generates ~20 signaling requests (join, leave, mute, status), that's $\approx 56,000 \text{ requests/sec}$.
* **Design Target**: Round up to **60,000 requests/sec** for the Signaling/API layer.

### B. Storage Estimation (Recordings)

* **Assumptions**:
  * 1% of calls are recorded.
  * Average recording size: 100MB (compressed).
  * Daily Recordings: $100,000,000 \times 0.01 = 1,000,000$ recordings.
* **Daily Storage**: $1,000,000 \times 100 \text{ MB} = 100,000,000 \text{ MB} = 100 \text{ TB/day}$.
* **Annual Storage**: $100 \text{ TB} \times 365 \approx 36.5 \text{ PB/year}$.
* **Strategy**: Tiered Storage (Hot -> Cool -> Archive) is mandatory.

### C. Bandwidth & Latency Targets

* **Latency Budget**:
  * **One-way Trip**: < 150ms is acceptable, but **< 64ms** is the target for "in-room" feel.
  * **Round Trip (RTT)**: < 128ms.
* **Bandwidth per Group Call (100 users)**:
  * If using SFU with Simulcast (High: 1Mbps, Low: 100kbps).
  * Ingress (Server): 100 users $\times$ 1Mbps = 100 Mbps.
  * Egress (Server): 100 users $\times$ (1 High + 5 Thumbnails) $\approx$ 100 $\times$ 1.5Mbps = 150 Mbps.
  * **Total per Call**: ~250 Mbps throughput.

## 3. Analytics & Observability Pipeline

To maintain quality at this scale, we need a dedicated Big Data pipeline to analyze **Quality of Service (QoS)** metrics (jitter, packet loss, RTT) and **User Engagement**.

### Architecture

1. **Data Ingestion**:
   * Clients send QoS events (every 30s) and "Call End" summaries to a **WebSocket Handler**.
   * Handler pushes events to a **Kafka** topic (`telemetry-events`).

1. **Processing Layer**:
   * **Real-time (Flink/Spark Streaming)**: Detects network outages, bad releases, or regional failures. Alerts Ops immediately.
   * **Batch (Hadoop/Spark)**: Aggregates daily trends, user retention, and feature usage.

1. **Storage**:
   * **Time-Series DB (InfluxDB/Prometheus)**: For operational metrics (CPU, Memory, Latency).
   * **Data Lake (S3/HDFS)**: For raw event logs and long-term analysis.

### Key Metrics to Track

* **Mean Opinion Score (MOS)**: Calculated from packet loss and jitter.
* **Join Time**: Time from "Click Join" to "Media Connected".
* **Reconnection Rate**: How often users drop and reconnect.

## 4. Hybrid Cloud Data Strategy

At massive scale, a single database technology or cloud provider might not suffice. We can adopt a **Polyglot & Hybrid** persistence strategy.

* **Metadata (AWS/Azure)**:
  * **Users, Meetings, Schedules**: Hosted on managed Relational Databases (Aurora/SQL Database) for ACID compliance and high availability.
  * **Sharding**: Sharded by `User_ID` or `Org_ID`.

* **Media Storage (Object Store)**:
  * **Recordings**: Stored in S3/Blob Storage. Cheaper and infinitely scalable.

* **Specialized Resources (Oracle/Other)**:
  * If the system serves specific verticals (e.g., Education), static resources or legacy data might reside in a specialized cloud or on-premise data center for compliance or cost reasons.
