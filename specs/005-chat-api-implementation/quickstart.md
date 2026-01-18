# Quick Start Guide: API Chat cho hệ thống Finance Agent

## Prerequisites

Before implementing the chat API functionality, ensure you have the following already set up from previous milestones:

- Node.js v20 or higher
- pnpm package manager (v9 or higher)
- MySQL database (set up in milestone 002)
- Redis for caching (optional but recommended for production)
- AI service API key (OpenAI or equivalent)
- A code editor (VS Code recommended)

## Getting Started

### 1. Navigate to the Backend Directory

The chat API is implemented as part of the existing backend application in the `finance-agent-app` directory:

```bash
cd finance-agent-app/backend
```

### 2. Install Dependencies (if not already done)

If you haven't installed dependencies yet:

```bash
pnpm install
```

### 3. Set Up Environment Variables

Ensure your `.env` file includes the necessary variables for the chat API:

```env
NODE_ENV=development
PORT=3000
DATABASE_URL=mysql://user:password@localhost:3306/finance_agent_db
OPENAI_API_KEY=your_openai_api_key_here
JWT_SECRET=your_jwt_secret_here
REDIS_HOST=localhost
REDIS_PORT=6379
CHAT_SESSION_TIMEOUT_MINUTES=120
MAX_MESSAGE_LENGTH=10000
```

### 4. Run Database Migrations

Since the chat entities are new, run migrations to create the required tables:

```bash
./migrate.sh
```

### 5. Start the Backend Service

To start the backend with chat API functionality:

```bash
pnpm run start:dev
```

The chat API will be available at `http://localhost:3000/api/chat`.

## API Endpoints

### Chat Operations

#### POST `/api/chat/send`
Send a message to the AI assistant and receive a response.

**Request Body:**
```json
{
  "sessionId": "uuid-string",
  "message": "Hello, I'd like to analyze my portfolio",
  "metadata": {}
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "message-uuid",
    "sessionId": "session-uuid",
    "senderType": "agent",
    "content": "I'd be happy to help you analyze your portfolio...",
    "createdAt": "2026-01-18T08:22:06.607Z"
  }
}
```

#### GET `/api/chat/stream/:sessionId`
Stream AI responses using Server-Sent Events (SSE).

**Headers:**
```
Accept: text/event-stream
Cache-Control: no-cache
Connection: keep-alive
```

**Event Stream:**
```
data: {"type": "chunk", "content": "I'd be happy"}
data: {"type": "chunk", "content": " to help you"}
data: {"type": "complete", "finalMessageId": "msg-uuid"}
```

#### POST `/api/chat/session`
Create a new chat session.

**Request Body:**
```json
{
  "title": "Portfolio Analysis Discussion"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "session-uuid",
    "title": "Portfolio Analysis Discussion",
    "status": "active",
    "createdAt": "2026-01-18T08:22:06.607Z"
  }
}
```

#### GET `/api/chat/history/:sessionId`
Retrieve chat history for a session.

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "msg-uuid",
      "senderType": "user",
      "content": "Hello, I'd like to analyze my portfolio",
      "createdAt": "2026-01-18T08:21:06.607Z"
    },
    {
      "id": "msg-uuid-2",
      "senderType": "agent",
      "content": "I'd be happy to help you analyze your portfolio...",
      "createdAt": "2026-01-18T08:21:08.123Z"
    }
  ]
}
```

## Implementation Details

### Chat Module Structure

The chat module follows the NestJS modular architecture:

```
finance-agent-app/backend/src/api/chat/
├── chat.controller.ts    # API endpoint handlers
├── chat.service.ts       # Business logic for chat operations
├── chat.module.ts        # Module definition and imports
└── dto/
    └── send-message.dto.ts # Data transfer objects for request validation
```

### Key Components

1. **Chat Controller**: Handles all chat-related HTTP requests
2. **Chat Service**: Contains the core business logic for managing chat sessions and messages
3. **AI Service Integration**: Connects to external AI providers for generating responses
4. **A2A Orchestration**: Integrates with the existing A2A system for portfolio analysis
5. **Message Persistence**: Stores chat messages in the database for session continuity

### A2A Integration

The chat system automatically detects financial content in user messages and triggers appropriate A2A agents. The system identifies keywords related to portfolios, investments, stocks, funds, and other financial instruments, then routes relevant information to specialized analysis agents.

## Security Considerations

- All chat endpoints require JWT authentication (inherited from milestone 002)
- Financial data is encrypted in transit using TLS
- Session management follows JWT best practices (inherited from milestone 002)
- Rate limiting is applied to prevent API abuse
- Input validation is performed on all message content
- All financial data is treated as read-only per compliance requirements

## Performance Tuning

For high-traffic deployments:

- Configure Redis connection pooling
- Adjust database connection pool size
- Enable caching for frequently accessed data
- Monitor memory usage during long chat sessions
- Optimize database queries for message retrieval

## Testing

Run the test suite to verify chat functionality:

```bash
# Unit tests for chat components
cd finance-agent-app/backend
pnpm run test -- --testPathPattern=chat

# Integration tests for API endpoints
pnpm run test:integration

# End-to-end tests for complete chat workflows
pnpm run test:e2e
```

## Troubleshooting

### Common Issues

1. **Streaming Issues**: If SSE streaming is not working:
   - Check that the client accepts `text/event-stream` content type
   - Verify that proxy servers aren't buffering the response
   - Confirm that the client handles disconnect events properly

2. **A2A Integration Problems**: If A2A agents aren't being triggered:
   - Verify that the A2A orchestrator service is running
   - Check that agent registry entries exist for financial analysis agents
   - Confirm that message content contains recognizable financial keywords

3. **Authentication Failures**: If authentication fails:
   - Verify that JWT tokens are properly formatted
   - Check that the token hasn't expired
   - Confirm that the user account is active

### Getting Help

- Check the API documentation at `http://localhost:3000/api/docs`
- Review the existing tests to understand expected behavior
- Consult the project constitution for architectural guidelines
- Examine the data models in `backend/src/models/` for entity relationships

## Next Steps

1. Integrate with the frontend ChatInterface components
2. Implement advanced A2A agent triggers based on message content
3. Add support for file uploads in chat (for portfolio import)
4. Enhance the streaming response with richer metadata
5. Implement chat history search and filtering capabilities