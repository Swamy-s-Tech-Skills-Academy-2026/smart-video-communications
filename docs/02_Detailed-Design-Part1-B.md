---
learning_level: "Advanced"
prerequisites: ["./02_Detailed-Design-Part1-A.md"]
estimated_time: "25 minutes"
learning_objectives:
  - "Visualize system interactions using sequence diagrams"
  - "Perform capacity planning calculations for bandwidth and storage"
related_topics:
  builds_upon: ["./02_Detailed-Design-Part1-A.md"]
  enables: ["./02_Detailed-Design-Part1-C.md"]
---

# Detailed Design: Sequences and Capacity - Part 1-B

## 3. Sequence Diagrams

### Flow 1: User Joins Meeting with Consent

This flow ensures compliance before allowing media connection.

```mermaid
sequenceDiagram
    participant User
    participant Client
    participant API as API Gateway
    participant Meet as Meeting Svc
    participant DB as Postgres
    participant Signal as Signaling Svc

    User->>Client: Click "Join Meeting"
    Client->>API: GET /meetings/{id}
    API->>Meet: Validate ID
    Meet-->>Client: Meeting Details + Consent Req

    User->>Client: Accept Consent (Recording/AI)
    Client->>API: POST /users/consent
    API->>DB: Store Consent Log
    API-->>Client: Consent Token

    Client->>API: POST /meetings/{id}/join (w/ Token)
    API->>Meet: Verify Token & Capacity
    Meet->>DB: Create Participant Record
    Meet-->>Client: Signaling Token

    Client->>Signal: Connect (WebSocket)
    Signal-->>Client: Connected (Ready for WebRTC)
```

### Flow 2: Real-Time AI Translation

This flow demonstrates the low-latency streaming path.

```mermaid
sequenceDiagram
    participant Speaker
    participant Media as Media Server (SFU)
    participant AI as Azure AI Speech
    participant Listener

    Speaker->>Media: Stream Audio (RTP)
    par Broadcast Audio
        Media->>Listener: Stream Audio (RTP)
    and Process AI
        Media->>AI: Stream Audio Chunks (gRPC)
        AI->>AI: Speech-to-Text + Translation
        AI-->>Media: Stream Text (Translated)
    end
    
    Media->>Listener: Push Text (WebSocket/DataChannel)
    Listener->>Listener: Render Subtitles
```

## 4. Capacity Planning

### Assumptions
*   **Concurrent Users**: 2,000
*   **Video Quality**: 720p (approx. 1.5 Mbps per stream)
*   **Audio Quality**: 64 kbps
*   **Meeting Duration**: 1 hour average

### Bandwidth Estimation
*   **Per User**: 1.5 Mbps (Up) + 1.5 Mbps (Down) = 3 Mbps.
*   **Total Bandwidth**: 2,000 users * 3 Mbps = **6,000 Mbps (6 Gbps)**.
*   **Implication**: Requires high-throughput networking (Azure Standard/Premium Networking) and potentially CDN for static assets, though media traffic goes through SFU clusters.

### Storage Estimation (Recordings)
*   **Video Size**: 1.5 Mbps * 3600 sec = 5,400 Mb ≈ **675 MB per hour**.
*   **Total Storage Per Hour**: 2,000 users (assuming 500 meetings of 4 people) -> 500 recordings.
*   **Volume**: 500 recordings * 675 MB = **337.5 GB per hour**.
*   **Daily Volume (8 busy hours)**: ~2.7 TB / day.
*   **Implication**: Aggressive lifecycle management (tiering to Cool/Archive) is critical to control costs.

### Compute (AI Services)
*   **Translation**: 2,000 concurrent streams.
*   **Quota**: Standard Azure AI tiers may throttle. Need **Provisioned Throughput Units (PTU)** for guaranteed capacity at this scale.
