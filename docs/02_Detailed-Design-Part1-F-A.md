---
learning_level: "Advanced"
prerequisites: ["./01_System-Design-Overview-Part1-B.md"]
estimated_time: "25 minutes"
learning_objectives:
  - "Define the core Data Models for a Video Conferencing System"
  - "Understand the separation between Persistent and Ephemeral data"
related_topics:
  builds_upon: ["./01_System-Design-Overview-Part1-B.md"]
  enables: ["./02_Detailed-Design-Part1-F-B.md"]
---

# Detailed Design: Data Models & API Contracts - Part 1-F-A

## 1. Core Data Models

The system requires a robust data model to handle users, rooms, and sessions.
We separate **Persistent Data** (SQL/Cosmos) from **Ephemeral Data** (Redis).

### A. Persistent Models (SQL / Cosmos DB)

These models store long-term history and configuration.

#### `User`

* **id** (UUID): Unique identifier.
* **email** (String): User email.
* **displayName** (String): Public name.
* **tenantId** (UUID): For multi-tenant support.
* **createdAt** (Timestamp).

#### `Room`

* **id** (UUID): Unique meeting identifier.
* **hostUserId** (UUID): Owner of the meeting.
* **settings** (JSON):
  * `isRecordingEnabled`: boolean
  * `maxParticipants`: integer
  * `allowedRoles`: ["host", "guest"]
* **status** (Enum): `SCHEDULED`, `ACTIVE`, `COMPLETED`.
* **createdAt** (Timestamp).

#### `Session` (Meeting Instance)

* **id** (UUID): Unique session ID (a room can have multiple sessions over time).
* **roomId** (UUID): FK to Room.
* **startTime** (Timestamp).
* **endTime** (Timestamp).
* **recordingUrl** (String): Path to the final recording in Blob Storage.

### B. Ephemeral Models (Redis)

These models exist only while a meeting is active.

#### `RoomState` (Hash)

* **Key**: `room:{roomId}`
* **Fields**:
  * `sfuRegion`: "us-east-1" (Sticky region for the meeting)
  * `participantCount`: Integer
  * `isRecording`: Boolean

#### `ParticipantState` (Hash)

* **Key**: `room:{roomId}:participants`
* **Fields**: `userId` -> JSON
  * `connectionId`: SignalR/WebSocket connection ID.
  * `role`: "host" | "attendee".
  * `isMuted`: Boolean.
  * `isVideoOn`: Boolean.
  * `joinedAt`: Timestamp.

