# Data Model: API Chat cho hệ thống Finance Agent

## Entity: ChatSession
**Description**: Represents a conversation session between a user and the AI assistant.

### Fields:
- `id`: String (UUID) - Primary Key, Unique identifier for the chat session
- `userId`: String (UUID) - Foreign Key, Reference to the owning user
- `title`: String - Title for the chat session
- `status`: Enum (active, completed, archived) - Current status of the session
- `createdAt`: DateTime - Timestamp when the session was created
- `updatedAt`: DateTime - Timestamp when the session was last updated
- `lastMessageAt`: DateTime (Nullable) - Timestamp of the last message in the session

### Relationships:
- One ChatSession belongs to one User (Many-to-One)
- One ChatSession has many ChatMessages (One-to-Many)

### Validation Rules:
- userId must reference a valid User entity
- status must be one of: active, completed, archived
- title must not exceed 255 characters
- createdAt and updatedAt must be in ISO 8601 format
- Session status transitions must follow valid state transitions (active → completed/archived, not vice versa)

---

## Entity: ChatMessage
**Description**: Represents an individual message within a chat session.

### Fields:
- `id`: String (UUID) - Primary Key, Unique identifier for the message
- `sessionId`: String (UUID) - Foreign Key, Reference to the parent session
- `senderType`: Enum (user, agent) - Type of the sender
- `senderId`: String (Nullable) - ID of the sender (user ID or agent ID)
- `content`: String - Content of the message
- `metadata`: JSON (Nullable) - Additional metadata associated with the message (e.g., A2A processing status, AI tokens used)
- `correlationId`: String (Nullable) - ID linking to related A2A messages
- `createdAt`: DateTime - Timestamp when the message was sent
- `updatedAt`: DateTime - Timestamp when the message was last updated

### Relationships:
- One ChatMessage belongs to one ChatSession (Many-to-One)
- One ChatMessage can trigger many A2AMessages (One-to-Many)

### Validation Rules:
- sessionId must reference a valid ChatSession entity
- senderType must be either 'user' or 'agent'
- content must be between 1 and 10,000 characters
- metadata must be valid JSON if provided
- correlationId must reference a valid A2AMessage if provided

---

## Entity: A2AMessage
**Description**: Represents a message exchanged between agents in the Agent-to-Agent communication system, specifically related to chat processing.

### Fields:
- `id`: String (UUID) - Primary Key, Unique identifier for the A2A message
- `sourceAgentId`: String - ID of the sending agent
- `targetAgentId`: String - ID of the receiving agent
- `messageType`: Enum (request, response, notification) - Type of the message
- `action`: String - Action or command being performed
- `payload`: JSON - Message content and data
- `correlationId`: String - ID linking related messages in a conversation
- `status`: Enum (pending, processing, completed, failed) - Current status
- `processedAt`: DateTime (Nullable) - Timestamp when the message was processed
- `createdAt`: DateTime - Timestamp when the message was created
- `updatedAt`: DateTime - Timestamp when the message was last updated
- `chatMessageId`: String (UUID) - Foreign Key, Reference to the originating chat message

### Relationships:
- One A2AMessage belongs to one ChatMessage (Many-to-One)

### Validation Rules:
- chatMessageId must reference a valid ChatMessage entity
- messageType must be one of: request, response, notification
- action must not exceed 100 characters
- payload must be valid JSON
- status must be one of: pending, processing, completed, failed
- All UUIDs are stored as strings following RFC 4122 format
- Timestamps are stored in ISO 8601 format in UTC timezone

---

## Entity: Chat Processing Service
**Description**: Represents the service responsible for processing chat messages and coordinating with A2A agents

### Fields:
- `id`: String - Unique identifier for the processing service configuration
- `streamingEnabled`: Boolean - Whether streaming responses are enabled
- `a2aIntegrationEnabled`: Boolean - Whether A2A integration is active
- `sessionTimeoutMinutes`: Number - Minutes before an inactive session expires
- `maxMessageLength`: Number - Maximum length of a single message
- `rateLimitPerMinute`: Number - Maximum messages per minute per user

### Relationships:
- One Chat Processing Service manages many ChatSessions
- One Chat Processing Service coordinates many A2AMessages

### Validation Rules:
- sessionTimeoutMinutes must be between 1 and 1440 (24 hours)
- maxMessageLength must be between 1 and 10000 characters
- rateLimitPerMinute must be greater than 0
- streamingEnabled must be boolean
- a2aIntegrationEnabled must be boolean