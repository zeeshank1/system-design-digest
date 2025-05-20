
# Chat System – Basic Design Overview

## 1. Core Requirements
- **Real-time messaging** between users (1:1 and group chats)
- **Message storage and retrieval**
- **User authentication**
- **Message delivery status** (sent, delivered, read)
- **Scalability** for large numbers of users

## 2. High-Level Components
- **Client App**: Web/mobile interfaces for sending and receiving messages.
- **API Gateway**: Fronts all client requests, routes to backend services.
- **Authentication Service**: Manages user login and tokens (JWT/ OAuth).
- **Chat Service**: Handles message send/receive, group management, delivery status.
- **Message Store**: Database (SQL/NoSQL) for persistent message history.
- **Real-Time Server**: WebSocket/long-polling server for instant delivery.
- **Notification Service**: Pushes notifications for new messages.

## 3. Key Design Considerations
- **Data Model**:
    - User: user_id, username, profile info
    - Message: message_id, sender_id, receiver_id(s), timestamp, content, status
    - Chat/Group: chat_id, participant_ids, metadata

- **Scalability**: 
    - Use horizontal scaling for Chat & Real-Time servers.
    - Partition messages by chat/user (sharding).
    - Cache frequently accessed data (e.g., Redis).

- **Reliability**:
    - Store messages durably before acknowledging delivery.
    - Use message queues for delivery retries.

- **Security**:
    - Encrypt messages in transit (TLS).
    - Use access tokens for authentication/authorization.

## 4. Basic Sequence Diagram

1. User logs in → Auth Service validates and returns token.
2. User connects to real-time server with token.
3. User sends a message → Chat Service stores it in DB → Real-Time Server pushes to recipient(s).
4. Recipient receives message → Client sends read receipt.
